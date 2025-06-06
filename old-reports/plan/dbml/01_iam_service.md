# IAM Service DBML

DBML schema for the Identity & Access Management Service.

```dbml
// IAM Service DBML
Project iam_service {
  database_type: 'PostgreSQL (Supabase Auth)'
}

Table users { // Primarily managed by Supabase Auth, extended here if needed
  id uuid [pk] // Supabase auth.users.id
  email varchar [unique] // Supabase auth.users.email
  // password_hash varchar // Managed by Supabase Auth
  full_name varchar
  // language varchar // Moved to user_settings for better separation
  // is_deleted boolean // Supabase handles user deletion/disabling
  // created_at timestamp // Supabase auth.users.created_at
  // updated_at timestamp // Supabase auth.users.updated_at
  // created_by uuid [note: 'ID of admin/system creating user'] // Optional audit
  // updated_by uuid // Optional audit
  raw_app_meta_data jsonb // Supabase: for app-specific, non-sensitive data (e.g., initial role hint)
  raw_user_meta_data jsonb // Supabase: for user-specific preferences
}

Table roles {
  id uuid [pk, default: `uuid_generate_v4()`]
  name varchar [unique, not null, note: 'e.g., solo_learner, enterprise_employee, enterprise_admin, course_creator, consultant, support_agent, platform_admin']
  description text
  created_at timestamp [default: `now()`]
}

Table user_roles { // Links users to their roles
  user_id uuid [ref: > users.id, not null]
  role_id uuid [ref: > roles.id, not null]
  created_at timestamp [default: `now()`]
  // assigned_by uuid [ref: > users.id] // Optional: who assigned the role

  indexes {
    (user_id, role_id) [pk]
  }
}

Table companies {
  id uuid [pk, default: `uuid_generate_v4()`]
  name varchar [not null]
  // email varchar [unique, note: 'Primary contact email for the company'] // Maybe store on a company_contact user
  // subscription_status varchar // This is now owned by Billing Service, IAM might cache a basic status
  current_iam_status varchar [default: 'active', note: 'e.g., active, suspended_by_admin, pending_verification']
  created_at timestamp [default: `now()`]
  updated_at timestamp
  is_deleted boolean [default: false]
  deleted_at timestamp
  // created_by uuid [ref: > users.id] // Optional
  // updated_by uuid [ref: > users.id] // Optional
}

Table company_users { // Links enterprise employees to their companies
  user_id uuid [ref: > users.id, not null]
  company_id uuid [ref: > companies.id, not null]
  // role_in_company varchar [note: 'e.g., member, manager - specific to company context, distinct from platform roles']
  joined_at timestamp [default: `now()`]
  is_active_in_company boolean [default: true] // If user is removed from company but not platform

  indexes {
    (user_id, company_id) [pk]
  }
}

Table user_settings { // User-specific preferences
  user_id uuid [pk, ref: > users.id]
  preferred_language varchar [default: 'en', not null] // e.g., 'en', 'fr'
  theme varchar [default: 'light', note: 'e.g., light, dark']
  notifications_enabled_email boolean [default: true]
  notifications_enabled_in_app boolean [default: true]
  // accessibility_options jsonb // For future flexible accessibility settings
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

// API Keys (if managed by IAM for user/company API access to the platform)
Table api_keys {
  id uuid [pk, default: `uuid_generate_v4()`]
  user_id uuid [ref: > users.id, null, note: 'If key is for a specific user']
  company_id uuid [ref: > companies.id, null, note: 'If key is for a company']
  name varchar [not null]
  key_prefix varchar(8) [unique, not null, note: 'First few chars of the key for identification']
  hashed_key varchar [unique, not null, note: 'Hashed version of the API key']
  permissions jsonb [note: 'Permissions scope for this key']
  expires_at timestamp
  last_used_at timestamp
  created_at timestamp [default: `now()`]
  is_active boolean [default: true]

  indexes {
    (user_id)
    (company_id)
  }
}

// Referral System (If tightly coupled with user identity)
Table referral_codes {
  id uuid [pk, default: `uuid_generate_v4()`]
  user_id uuid [ref: > users.id, not null, unique, note: 'Each user can have one primary referral code']
  code varchar [unique, not null]
  // discount_percentage float // Discount details might be better in Billing/Promo service
  is_active boolean [default: true]
  created_at timestamp [default: `now()`]
  expires_at timestamp
}

Table referrals { // Tracks who referred whom
  id uuid [pk, default: `uuid_generate_v4()`]
  referrer_user_id uuid [ref: > users.id, not null]
  referred_user_id uuid [ref: > users.id, not null, unique, note: 'A user can only be referred once']
  referral_code_used_id uuid [ref: > referral_codes.id, not null]
  status varchar [default: 'pending', note: 'e.g., pending, completed_signup, subscription_activated']
  // reward_granted_to_referrer boolean [default: false] // Reward logic in Billing/Promo
  // reward_granted_to_referred boolean [default: false] // Discount logic in Billing/Promo
  created_at timestamp [default: `now()`]
  converted_at timestamp [note: 'When the referral led to a desired action']
}
```