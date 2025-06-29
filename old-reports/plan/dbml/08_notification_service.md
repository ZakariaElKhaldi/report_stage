# Notification Service DBML

DBML schema for the Notification Service.

```dbml
// Notification Service DBML
Project notification_service {
  database_type: 'PostgreSQL'
}

Table notification_types { // Defines the types of notifications the system can send
  id uuid [pk, default: `uuid_generate_v4()`]
  event_name varchar [unique, not null, note: 'e.g., USER_REGISTERED, COURSE_ENROLLED, PAYMENT_SUCCEEDED, CONSULTATION_REMINDER']
  description text
  default_channels text[] [not null, default: '{"in_app", "email"}', note: 'Default channels for this type, e.g., in_app, email, sms']
  // is_user_configurable boolean [default: true, note: 'Can users disable this notification type?']
  // priority varchar [default: 'medium', note: 'e.g., low, medium, high']
  created_at timestamp [default: `now()`]
}

Table notification_templates { // Stores templates for different languages and channels
  id uuid [pk, default: `uuid_generate_v4()`]
  notification_type_id uuid [ref: > notification_types.id, not null]
  language_code varchar(10) [not null, note: 'e.g., en, fr-CA']
  channel varchar [not null, note: 'e.g., email, in_app, sms'] // Template per channel
  subject_template text [null, note: 'For email subject, uses template variables like {{user_name}}']
  body_template text [not null, note: 'For email/sms body or in-app message, uses template variables']
  // template_variables_schema jsonb [null, note: 'Schema of expected variables for validation (e.g., {"user_name": "string", "course_title": "string"})']
  is_active boolean [default: true]
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (notification_type_id, language_code, channel) [unique]
  }
}

Table notifications_sent { // Log of individual notifications that have been processed/sent
  id uuid [pk, default: `uuid_generate_v4()`]
  recipient_user_id uuid [not null, note: 'User ID from IAM service']
  notification_type_id uuid [ref: > notification_types.id, not null]
  // triggering_event_id varchar [null, note: 'Original event ID that triggered this notification, for idempotency/tracing']
  // entity_type varchar [null, note: 'e.g., course, subscription, consultation_session']
  // entity_id uuid [null, note: 'ID of the related entity']
  // data_payload jsonb [null, note: 'The actual data used to render the template variables']
  created_at timestamp [default: `now()`] // When the notification was generated by this service
  // read_at timestamp [null, note: 'For in-app notifications, when the user read it']
  // is_archived boolean [default: false, note: 'For in-app notifications, if user archived it']
}

Table notification_deliveries { // Tracks delivery status for each channel of a sent notification
  id uuid [pk, default: `uuid_generate_v4()`]
  notification_sent_id uuid [ref: > notifications_sent.id, not null]
  channel varchar [not null, note: 'e.g., email, in_app, sms']
  status varchar [not null, default: 'pending', note: 'e.g., pending, queued, sent, failed, delivered, opened, clicked']
  // external_message_id varchar [null, note: 'ID from the email/SMS provider']
  // failure_reason text [null]
  // attempts int [default: 0]
  last_attempted_at timestamp
  // delivered_at timestamp [null, note: 'Actual delivery time if provider confirms']
  created_at timestamp [default: `now()`] // When this specific delivery attempt was initiated
  updated_at timestamp

  indexes {
    (notification_sent_id, channel) [unique]
    (status, last_attempted_at) // For retry queues or monitoring
  }
}
```