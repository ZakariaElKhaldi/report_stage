# Booking & Consultation Service DBML

DBML schema for the Booking & Consultation Service (E-learning Platform Side).

```dbml
// Booking & Consultation Service DBML
Project booking_service {
  database_type: 'PostgreSQL'
}

Table consultation_service_types { // Defines the types of consultations offered
  id uuid [pk, default: `uuid_generate_v4()`]
  name varchar [not null, unique]
  description text
  default_duration_minutes int [not null]
  // price_per_session decimal(10,2) // Pricing might be in Billing service
  is_active boolean [default: true]
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table consultant_profiles { // Profile of consultants offering services
  // This might largely mirror data from IAM (user_id, name)
  // but can hold booking-specific details.
  consultant_user_id uuid [pk, note: 'User ID from IAM service, must have Consultant role']
  bio text
  specializations text[] // Array of strings or link to specialization tags
  // average_rating float // Could be calculated and stored here
  is_available_for_booking boolean [default: true]
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table consultant_availability_slots { // When consultants are available
  id uuid [pk, default: `uuid_generate_v4()`]
  consultant_user_id uuid [ref: > consultant_profiles.consultant_user_id, not null]
  start_time timestamp [not null]
  end_time timestamp [not null]
  // is_recurring boolean [default: false]
  // recurrence_rule varchar // e.g., iCalendar RRULE
  status varchar [default: 'available', note: 'e.g., available, booked, unavailable']
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (consultant_user_id, start_time, end_time)
    (status)
  }
}

Table consultation_requests { // Requests made by users
  id uuid [pk, default: `uuid_generate_v4()`]
  requester_user_id uuid [not null, note: 'User ID from IAM service']
  // company_id uuid [null, note: 'Company ID from IAM, if request is on behalf of company']
  consultation_service_type_id uuid [ref: > consultation_service_types.id, not null]
  preferred_consultant_user_id uuid [ref: > consultant_profiles.consultant_user_id, null]
  // preferred_start_time timestamp
  // preferred_end_time timestamp
  request_details text [note: 'Problem description, topics to cover']
  status varchar [not null, default: 'pending', note: 'e.g., pending_assignment, pending_confirmation, scheduled, cancelled, completed, rejected']
  requested_at timestamp [default: `now()`]
  updated_at timestamp
  // assigned_consultant_user_id uuid [ref: > consultant_profiles.consultant_user_id, null]
  // scheduled_session_id uuid [ref: > scheduled_sessions.id, null, unique] // Link to the actual session
}

Table scheduled_sessions { // Confirmed and scheduled consultation sessions
  id uuid [pk, default: `uuid_generate_v4()`]
  consultation_request_id uuid [ref: > consultation_requests.id, not null, unique]
  requester_user_id uuid [not null, note: 'User ID from IAM service']
  consultant_user_id uuid [ref: > consultant_profiles.consultant_user_id, not null]
  consultation_service_type_id uuid [ref: > consultation_service_types.id, not null]
  scheduled_start_time timestamp [not null]
  scheduled_end_time timestamp [not null]
  meeting_platform_session_id varchar [null, note: 'ID from your custom Meeting Platform']
  meeting_platform_join_url text [null]
  status varchar [not null, default: 'scheduled', note: 'e.g., scheduled, in_progress, completed, cancelled_by_user, cancelled_by_consultant, no_show_user, no_show_consultant']
  // outcome_notes text // Notes by consultant after session (could be in Meeting Platform)
  // billed_amount decimal(10,2) // Billing details likely come from Meeting Platform via event
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (consultant_user_id, scheduled_start_time)
    (requester_user_id, scheduled_start_time)
    (status)
  }
}
```