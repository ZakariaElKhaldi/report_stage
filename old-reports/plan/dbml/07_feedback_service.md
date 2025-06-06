# Feedback Service DBML

DBML schema for the Feedback Service.

```dbml
// Feedback Service DBML
Project feedback_service {
  database_type: 'PostgreSQL'
}

Table course_ratings {
  // id uuid [pk, default: `uuid_generate_v4()`] // Optional explicit PK
  user_id uuid [not null, note: 'User ID from IAM service']
  course_id uuid [not null, note: 'Course ID from Course service (could be course_version_id if ratings are version-specific)']
  rating int [not null, note: 'e.g., 1 to 5 stars']
  // CHECK (rating >= 1 AND rating <= 5)
  created_at timestamp [default: `now()`]
  updated_at timestamp [note: 'If users can change their rating']

  indexes {
    (user_id, course_id) [pk] // User can rate a course once
    (course_id, rating) // For calculating average ratings
  }
}

Table course_reviews {
  id uuid [pk, default: `uuid_generate_v4()`]
  user_id uuid [not null, note: 'User ID from IAM service']
  course_id uuid [not null, note: 'Course ID from Course service (or course_version_id)']
  // rating_id uuid [ref: > course_ratings.id, null, unique, note: 'If review is always tied to a rating submission']
  review_title varchar [null]
  review_text text [not null]
  status varchar [not null, default: 'pending_moderation', note: 'e.g., pending_moderation, approved, rejected, flagged']
  // moderation_notes text [null, note: 'Notes by moderator']
  // moderated_by_user_id uuid [null, note: 'Admin/Moderator User ID from IAM service']
  // moderated_at timestamp [null]
  created_at timestamp [default: `now()`]
  updated_at timestamp [note: 'If users can edit their reviews before approval, or if status changes']

  indexes {
    (user_id, course_id) [unique, name: 'uq_user_course_review'] // User can review a course once
    (course_id, status) // For fetching approved reviews for a course
    (status) // For moderation queues
  }
}

// Optional: For reviews/feedback on consultations if that's a feature
Table consultation_feedback {
  id uuid [pk, default: `uuid_generate_v4()`]
  scheduled_session_id uuid [not null, unique, note: 'ID of the session from Booking Service']
  reviewer_user_id uuid [not null, note: 'User ID of the person giving feedback (usually the requester)']
  consultant_user_id uuid [not null, note: 'User ID of the consultant being reviewed']
  rating int [null, note: 'e.g., 1 to 5 stars for the consultation']
  // CHECK (rating IS NULL OR (rating >= 1 AND rating <= 5))
  feedback_text text
  is_public boolean [default: false, note: 'If feedback can be shown on consultant profile']
  status varchar [default: 'submitted', note: 'e.g., submitted, under_review (if moderated)']
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (consultant_user_id, status, is_public) // For displaying public feedback on consultant profiles
  }
}
```