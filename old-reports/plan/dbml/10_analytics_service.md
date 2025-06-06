# Analytics & Reporting Service DBML

Example DBML schema for the Analytics & Reporting Service. This service often contains derived or aggregated data consumed from events or other service replicas. The actual schema is highly dependent on specific reporting needs.

```dbml
// Analytics & Reporting Service DBML (Example - highly customizable)
Project analytics_service {
  database_type: 'PostgreSQL (or specialized analytics DB like ClickHouse, BigQuery)'
}

// Example: Aggregated daily active users
Table daily_active_users_summary {
  summary_date date [pk]
  active_user_count int [not null]
  // solo_learner_active_count int
  // enterprise_employee_active_count int
  // new_registrations_on_day int
  created_at timestamp [default: `now()`]
}

// Example: Aggregated course engagement
Table course_engagement_summary {
  // id uuid [pk] // or composite key
  course_id uuid [not null, note: 'Course ID from Course service']
  summary_period_start date [not null]
  summary_period_end date [not null]
  total_enrollments_in_period int [default: 0]
  total_completions_in_period int [default: 0]
  average_time_spent_seconds int [null]
  average_quiz_score_percentage float [null]
  // last_calculated_at timestamp [default: `now()`]

  indexes {
    (course_id, summary_period_start, summary_period_end) [pk, name: 'pk_course_engagement']
  }
}

// Example: Company-level learning summary
Table company_learning_summary {
  company_id uuid [not null, note: 'Company ID from IAM service']
  summary_date date [not null]
  active_learners_count int [default: 0]
  total_courses_completed_by_employees int [default: 0]
  total_learning_time_seconds_by_employees bigint [default: 0]
  // last_calculated_at timestamp [default: `now()`]

  indexes {
    (company_id, summary_date) [pk, name: 'pk_company_learning']
  }
}

// Example: Storing raw events for later processing (could also be in a data lake / event store like Kafka)
Table raw_events_log {
  id uuid [pk, default: `uuid_generate_v4()`]
  event_timestamp timestamp [not null, default: `now()`]
  event_source_service varchar [not null] // e.g., IAM, CourseService, BillingService
  event_type varchar [not null] // e.g., USER_LOGIN, LESSON_COMPLETED, PAYMENT_SUCCEEDED
  user_id uuid [null]
  company_id uuid [null]
  entity_id uuid [null] // ID of the primary entity related to the event
  payload jsonb // The full event payload
  // processed_at timestamp [null] // If this table is used as a staging area

  indexes {
    (event_timestamp)
    (event_type)
    (user_id)
  }
}
```