# Platform Configuration & Admin Service DBML

DBML schema for the Platform Configuration & Admin Service.

```dbml
// Platform Configuration & Admin Service DBML
Project platform_config_service {
  database_type: 'PostgreSQL'
}

Table platform_settings { // Global settings for the entire platform
  // id int [pk, default: 1] // Often a single row table
  // CHECK (id = 1)
  setting_key varchar [pk, note: 'e.g., DEFAULT_LANGUAGE, SUPPORTED_LANGUAGES, MAINTENANCE_MODE, TOS_URL']
  setting_value text [not null]
  value_type varchar [not null, default: 'string', note: 'e.g., string, json, boolean, integer, array_string']
  description text
  // is_editable_by_admin boolean [default: true]
  last_updated_by_user_id uuid [null, note: 'Admin User ID from IAM service']
  updated_at timestamp [default: `now()`]

  // Example rows:
  // ('DEFAULT_LANGUAGE', 'en', 'string', 'Default language for new users')
  // ('SUPPORTED_LANGUAGES', '["en", "fr"]', 'array_string', 'List of BCP 47 language codes supported')
  // ('MAINTENANCE_MODE', 'false', 'boolean', 'Is the platform in maintenance mode?')
  // ('PAYMENT_GATEWAY_CONFIG', '{"publicKey": "pk_...", "secretKeyName": "stripe_secret"}', 'json', 'Configuration for payment gateway integration')
}


Table company_settings { // Company-specific overrides or settings
  company_id uuid [pk, note: 'Company ID from IAM service']
  // branding_logo_url text [null] // URL to company logo (might be stored in Media Service)
  // branding_primary_color varchar(7) [null, note: 'e.g., #RRGGBB']
  // custom_css_url text [null]
  // feature_flags_override jsonb [null, note: 'Overrides for global feature flags, e.g., {"enable_beta_dashboard": true}']
  // certificate_custom_template text [null, note: 'Custom template ID or content for certificates']
  // allowed_course_creator_user_ids uuid[] [null, note: 'Specific users within this company allowed to create courses, if applicable']
  last_updated_by_user_id uuid [null, note: 'Admin User ID from IAM service (platform admin or company admin)']
  updated_at timestamp [default: `now()`]

  // This table can be a key-value store per company as well, similar to platform_settings
  // Or have fixed columns for common settings.
  // Example using fixed columns:
  brand_logo_media_id uuid [null, note: 'Media ID from Media Service']
  primary_color_hex varchar(7) [null, note: '#RRGGBB']
  // default_employee_roles uuid[] [null, note: 'Role IDs from IAM to assign by default to new company users']
}

Table feature_flags { // For A/B testing or phased rollouts
  id uuid [pk, default: `uuid_generate_v4()`]
  flag_key varchar [unique, not null, note: 'e.g., NEW_COURSE_PLAYER_UX, AI_RECOMMENDATIONS_ENABLED']
  description text
  is_globally_enabled boolean [default: false]
  // rollout_percentage int [default: 0, note: '0-100, for percentage-based rollout']
  // target_user_ids uuid[] [null, note: 'Specific user_ids this flag applies to']
  // target_company_ids uuid[] [null, note: 'Specific company_ids this flag applies to']
  // target_attributes jsonb [null, note: 'e.g., {"country": "US", "user_segment": "power_users"}']
  created_at timestamp [default: `now()`]
  updated_at timestamp
  // created_by_user_id uuid [null, note: 'Admin User ID from IAM service']
}
```