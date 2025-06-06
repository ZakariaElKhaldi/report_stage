# Learner Progress & Certification Service DBML

DBML schema for the Learner Progress & Certification Service (Corrected Version).

```dbml
// Learner Progress & Certification Service DBML (Corrected)
Project learner_progress_service {
  database_type: 'PostgreSQL'
}

Table enrollments { // User enrollment in a specific course version
  // id uuid [pk, default: `uuid_generate_v4()`] // Optional explicit PK
  user_id uuid [not null, note: 'User ID from IAM service']
  course_version_id uuid [not null, note: 'Course Version ID from Course service. THIS IS THE KEY CONTEXT.']
  enrolled_at timestamp [default: `now()`]
  status varchar [default: 'active', note: 'e.g., active, completed, cancelled_by_user, revoked_by_admin']
  completed_at timestamp
  // source_of_enrollment varchar [note: 'e.g., solo_subscription, company_assignment, admin_added']

  indexes {
    (user_id, course_version_id) [pk] // A user is enrolled in a course version once
    (status)
  }
}

Table lesson_progress {
  // id uuid [pk, default: `uuid_generate_v4()`] // Optional explicit PK
  user_id uuid [not null, note: 'User ID from IAM service']
  lesson_id uuid [not null, note: 'Lesson ID from Course service']
  // THE CRUCIAL CONTEXT FIELD:
  enrolled_course_version_id uuid [not null, note: 'FK-like reference to the specific course_version_id from the enrollments table this progress belongs to. Ensures progress is tied to a specific enrollment instance.']
  // This enrolled_course_version_id should match the course_version_id the lesson_id belongs to.

  status varchar [not null, default: 'not_started', note: 'e.g., not_started, in_progress, completed']
  progress_percentage float [default: 0, note: '0-100, if lesson has sub-parts or video progress']
  last_accessed_at timestamp [default: `now()`]
  started_at timestamp
  completed_at timestamp
  // score float [note: 'If the lesson itself is scorable, e.g. interactive exercise']

  // Define a composite primary key or unique constraint
  indexes {
    (user_id, lesson_id, enrolled_course_version_id) [pk, name: 'pk_lesson_progress'] // Ensures progress for a lesson within a specific enrollment
    (user_id, enrolled_course_version_id, status) [name: 'idx_lesson_progress_course_status'] // For querying progress within a course
  }
  // Optionally, if you want to enforce that enrolled_course_version_id exists in enrollments table for the same user:
  // This would be an application-level check or a trigger if not a direct FK due to microservice boundary.
  // For simplicity in DBML, we imply this logical link.
}

Table quiz_attempts {
  id uuid [pk, default: `uuid_generate_v4()`]
  user_id uuid [not null, note: 'User ID from IAM service']
  quiz_id uuid [not null, note: 'Quiz ID from Course service']
  // THE CRUCIAL CONTEXT FIELD:
  enrolled_course_version_id uuid [not null, note: 'FK-like reference to the specific course_version_id from the enrollments table this attempt belongs to.']
  // This enrolled_course_version_id should match the course_version_id the quiz_id belongs to.

  attempt_number int [not null, default: 1]
  score float [note: 'Actual score achieved, e.g., 85.0 for 85%']
  is_passed boolean [note: 'Based on quiz pass_threshold_percentage from Course service']
  started_at timestamp [default: `now()`]
  completed_at timestamp
  answers_snapshot jsonb [note: 'Snapshot of user answers: {question_id: chosen_answer_id(s)}']

  indexes {
    (user_id, quiz_id, enrolled_course_version_id, attempt_number) [unique, name: 'uq_quiz_attempt']
    (user_id, enrolled_course_version_id) [name: 'idx_quiz_attempt_course_perf'] // For overall quiz performance in a course
  }
}

Table certificates { // Issued certificates
  id uuid [pk, default: `uuid_generate_v4()`]
  user_id uuid [not null, note: 'User ID from IAM service']
  // THE CRUCIAL CONTEXT FIELD:
  course_version_id uuid [not null, note: 'Course Version ID from Course service for which this certificate is issued. This should logically link to an enrollment.']
  issued_at timestamp [default: `now()`]
  certificate_uid varchar [unique, not null, note: 'Publicly verifiable unique ID for the certificate']
  // pdf_url text [note: 'URL to the generated PDF, if stored externally by a PDF generation service']
  // metadata jsonb [note: 'e.g., snapshot of course title, user name at time of issuance']

  indexes {
    (user_id, course_version_id) [unique] // Usually one certificate per user per course version
    (certificate_uid)
  }
}
```