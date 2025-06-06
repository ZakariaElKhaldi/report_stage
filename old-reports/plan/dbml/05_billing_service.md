# Billing & Subscription Service DBML

DBML schema for the Billing & Subscription Service.

```dbml
// Billing & Subscription Service DBML
Project billing_service {
  database_type: 'PostgreSQL'
}

Table subscription_plans { // Defines available subscription plans
  id uuid [pk, default: `uuid_generate_v4()`]
  name varchar [not null, unique]
  plan_type varchar [not null, note: 'e.g., solo_learner, enterprise_small, enterprise_medium, enterprise_large']
  description text
  // For solo learners
  price_monthly decimal(10,2)
  price_yearly decimal(10,2)
  // For enterprises (could be per user, or tiered)
  price_per_user_monthly decimal(10,2) [null, note: 'If plan is per-user based for enterprise']
  price_per_user_yearly decimal(10,2) [null, note: 'If plan is per-user based for enterprise']
  // min_users int [null, note: 'For enterprise plans']
  // max_users int [null, note: 'For enterprise plans']
  // included_features jsonb [note: 'e.g., {"reporting_level": "advanced", "sso": true}']
  currency_code varchar(3) [not null, default: 'USD'] // ISO 4217
  is_active boolean [default: true, not null] // Can admin deactivate plans?
  trial_period_days int [default: 0]
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table user_subscriptions { // Active subscriptions for solo users or companies
  id uuid [pk, default: `uuid_generate_v4()`]
  user_id uuid [null, note: 'User ID from IAM service (for solo learners)']
  company_id uuid [null, note: 'Company ID from IAM service (for enterprises)']
  subscription_plan_id uuid [ref: > subscription_plans.id, not null]
  status varchar [not null, default: 'trialing', note: 'e.g., trialing, active, past_due, cancelled, expired, payment_failed']
  start_date date [not null]
  end_date date [null, note: 'For fixed-term or if explicitly cancelled for a future date']
  current_period_start date [not null]
  current_period_end date [not null]
  // next_billing_date date [not null] // Can be derived or explicitly stored
  // quantity int [default: 1, note: 'For enterprise plans, number of licensed users/seats']
  // trial_ends_at timestamp [null]
  payment_gateway_customer_id varchar [null, note: 'Customer ID from payment gateway like Stripe']
  payment_gateway_subscription_id varchar [null, unique, note: 'Subscription ID from payment gateway']
  // discount_id uuid [ref: > applied_discounts.id, null] // If a discount is active
  created_at timestamp [default: `now()`]
  updated_at timestamp
  cancelled_at timestamp [null]
  cancellation_reason text [null]

  indexes {
    (user_id, status) // If user_id is not null
    (company_id, status) // If company_id is not null
    (payment_gateway_subscription_id)
    (status, current_period_end) // For finding subscriptions needing renewal/action
  }
  // Constraint: Either user_id or company_id must be non-null, but not both.
  // CHECK ((user_id IS NOT NULL AND company_id IS NULL) OR (user_id IS NULL AND company_id IS NOT NULL))
}

Table payments { // Records of actual payment transactions
  id uuid [pk, default: `uuid_generate_v4()`]
  user_subscription_id uuid [ref: > user_subscriptions.id, null, note: 'If payment is for a subscription']
  // user_id uuid [note: 'User ID from IAM, for one-off purchases if any, or context']
  // company_id uuid [note: 'Company ID from IAM, for context']
  amount_decimal decimal(10,2) [not null]
  currency_code varchar(3) [not null] // ISO 4217
  status varchar [not null, note: 'e.g., pending, succeeded, failed, refunded, disputed']
  payment_method_type varchar [null, note: 'e.g., card, bank_transfer']
  payment_gateway_charge_id varchar [unique, not null, note: 'Charge/Transaction ID from payment gateway']
  // payment_gateway_payment_intent_id varchar [unique, null] // For Stripe Payment Intents
  paid_at timestamp [null, note: 'When payment was confirmed successful']
  // failure_reason text [null, note: 'If payment failed']
  // related_invoice_id uuid [ref: > invoices.id, null]
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (user_subscription_id)
    (payment_gateway_charge_id)
    (status)
  }
}

Table invoices { // Formal invoice records
  id uuid [pk, default: `uuid_generate_v4()`]
  user_subscription_id uuid [ref: > user_subscriptions.id, null] // The subscription this invoice is for
  user_id uuid [null, note: 'User ID from IAM service, for recipient context']
  company_id uuid [null, note: 'Company ID from IAM service, for recipient context']
  invoice_number varchar [unique, not null] // Generated unique invoice number
  status varchar [not null, note: 'e.g., draft, open, paid, void, uncollectible']
  amount_due_decimal decimal(10,2) [not null]
  amount_paid_decimal decimal(10,2) [default: 0.00]
  // amount_remaining_decimal decimal(10,2) // Can be calculated
  currency_code varchar(3) [not null]
  issue_date date [not null]
  due_date date [not null]
  paid_date date [null]
  // pdf_url text [null, note: 'URL to the generated PDF of the invoice'] // PDF generation is another concern
  // payment_gateway_invoice_id varchar [unique, null, note: 'Invoice ID from payment gateway if it generates one']
  // notes text
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (user_subscription_id)
    (status, due_date)
    // (user_id) // If querying invoices by user
    // (company_id) // If querying invoices by company
  }
}

Table invoice_items {
  id uuid [pk, default: `uuid_generate_v4()`]
  invoice_id uuid [ref: > invoices.id, not null]
  description text [not null]
  quantity int [not null, default: 1]
  unit_price_decimal decimal(10,2) [not null]
  total_price_decimal decimal(10,2) [not null] // quantity * unit_price
  // subscription_plan_id uuid [ref: > subscription_plans.id, null, note: 'If item relates to a plan']
  // course_id uuid [null, note: 'If for a specific course purchase (less relevant with your subscription model)']
  created_at timestamp [default: `now()`]
}


Table payment_methods { // Tokenized payment methods stored by payment gateway
  id uuid [pk, default: `uuid_generate_v4()`]
  user_id uuid [null, note: 'User ID from IAM service']
  company_id uuid [null, note: 'Company ID from IAM service']
  payment_gateway_customer_id varchar [not null, note: 'Customer ID from payment gateway']
  payment_gateway_method_id varchar [unique, not null, note: 'Payment method ID from gateway (e.g., Stripe pm_xxx or card_xxx)']
  method_type varchar [not null, note: 'e.g., card, sepa_debit']
  brand varchar [null, note: 'e.g., Visa, Mastercard for cards']
  last4 varchar [null, note: 'Last 4 digits for cards']
  exp_month int [null]
  exp_year int [null]
  is_default boolean [default: false]
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (payment_gateway_customer_id)
    (user_id, is_default)
    (company_id, is_default)
  }
}

// For discounts/promo codes (if Billing service manages application)
Table promo_codes {
  id uuid [pk, default: `uuid_generate_v4()`]
  code varchar [unique, not null]
  discount_type varchar [not null, note: 'e.g., percentage, fixed_amount']
  discount_value decimal(10,2) [not null]
  // applicable_plan_ids uuid[] [null, note: 'Array of subscription_plan_ids it applies to, null for all']
  // applicable_to varchar [note: 'e.g., new_users_only, all_users']
  max_redemptions int [null] // Max total redemptions
  // redemptions_count int [default: 0]
  valid_from timestamp [null]
  expires_at timestamp [null]
  is_active boolean [default: true]
  created_at timestamp [default: `now()`]
}

Table applied_discounts { // Tracks when a promo code is applied to a subscription
  id uuid [pk, default: `uuid_generate_v4()`]
  user_subscription_id uuid [ref: > user_subscriptions.id, not null]
  promo_code_id uuid [ref: > promo_codes.id, not null]
  // discount_applied_amount decimal(10,2) [not null, note: 'Actual amount discounted at time of application']
  applied_at timestamp [default: `now()`]
  // duration varchar [note: 'e.g., once, forever, N_months']
  // related_invoice_id uuid [ref: > invoices.id, null]

  indexes {
    (user_subscription_id, promo_code_id) [unique]
  }
}
```