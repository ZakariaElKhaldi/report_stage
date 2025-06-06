# Course Catalog & Content Service DBML

DBML schema for the Course Catalog & Content Service.

```dbml
// Course Catalog & Content Service DBML
Project course_service {
  database_type: 'PostgreSQL'
}

Table courses {
  id uuid [pk, default: `uuid_generate_v4()`]
  // author_id uuid [note: 'user_id of the primary course creator, from IAM service'] // Stored, not FK
  primary_author_user_id uuid [not null, note: 'User ID from IAM service']
  // visibility varchar //  More complex, handled by publication status in course_versions
  created_at timestamp [default: `now()`]
  updated_at timestamp
  is_deleted boolean [default: false]
  deleted_at timestamp
  // created_by_user_id uuid [note: 'User ID from IAM service']
  // updated_by_user_id uuid [note: 'User ID from IAM service']
}

Table course_versions {
  id uuid [pk, default: `uuid_generate_v4()`]
  course_id uuid [ref: > courses.id, not null]
  version_number int [not null, default: 1]
  publication_status varchar [not null, default: 'draft', note: 'e.g., draft, pending_review, published, archived']
  published_at timestamp
  changelog text [note: 'Summary of changes in this version']
  // certificate_rule_id uuid [note: 'ID from Certification Service if rules are complex & separate'] // Or simple rules here
  min_quiz_score_for_cert float [note: 'If simple rule: min average score across all quizzes for certificate']
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (course_id, version_number) [unique]
    (publication_status)
  }
}

Table course_version_translations {
  id uuid [pk, default: `uuid_generate_v4()`]
  course_version_id uuid [ref: > course_versions.id, not null]
  language_code varchar(10) [not null, note: 'e.g., en, fr-CA'] // Using BCP 47 codes
  title varchar [not null]
  description text
  learning_objectives text // Comma-separated or JSON array
  target_audience text

  indexes {
    (course_version_id, language_code) [unique]
  }
}

Table modules {
  id uuid [pk, default: `uuid_generate_v4()`]
  course_version_id uuid [ref: > course_versions.id, not null]
  display_order int [not null, default: 0]
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table module_translations {
  id uuid [pk, default: `uuid_generate_v4()`]
  module_id uuid [ref: > modules.id, not null]
  language_code varchar(10) [not null]
  title varchar [not null]
  description text

  indexes {
    (module_id, language_code) [unique]
  }
}

Table lessons {
  id uuid [pk, default: `uuid_generate_v4()`]
  module_id uuid [ref: > modules.id, not null]
  lesson_type varchar [not null, default: 'video', note: 'e.g., video, text_image, quiz_ref']
  display_order int [not null, default: 0]
  estimated_duration_minutes int
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table lesson_translations {
  id uuid [pk, default: `uuid_generate_v4()`]
  lesson_id uuid [ref: > lessons.id, not null]
  language_code varchar(10) [not null]
  title varchar [not null]
  content_text text [note: 'For text-based lessons']
  // For video lessons, video_media_id will be in lesson_media (Media Service context)
  // For image lessons, image_media_id will be in lesson_media

  indexes {
    (lesson_id, language_code) [unique]
  }
}

Table quizzes { // Defines the structure of a quiz
  id uuid [pk, default: `uuid_generate_v4()`]
  // module_id uuid [ref: > modules.id, note: 'If quiz is part of a module']
  // course_version_id uuid [ref: > course_versions.id, note: 'If quiz is standalone for a course version']
  // One of the above should be not null, or quiz is linked via lesson_type='quiz_ref'
  associated_lesson_id uuid [ref: > lessons.id, null, unique, note: 'If this quiz IS the lesson content']
  pass_threshold_percentage float [note: 'e.g., 70 for 70% to pass this specific quiz']
  time_limit_minutes int
  max_attempts int [null, note: 'Null for unlimited attempts']
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table quiz_translations {
  id uuid [pk, default: `uuid_generate_v4()`]
  quiz_id uuid [ref: > quizzes.id, not null]
  language_code varchar(10) [not null]
  title varchar [not null]
  description text
  instructions text

  indexes {
    (quiz_id, language_code) [unique]
  }
}

Table questions { // Questions for a quiz
  id uuid [pk, default: `uuid_generate_v4()`]
  quiz_id uuid [ref: > quizzes.id, not null]
  question_type varchar [not null, note: 'e.g., single_choice, multiple_choice, true_false, short_answer']
  display_order int [not null, default: 0]
  points int [default: 1, not null] // Points for this question
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table question_translations {
  id uuid [pk, default: `uuid_generate_v4()`]
  question_id uuid [ref: > questions.id, not null]
  language_code varchar(10) [not null]
  question_text text [not null]
  hint text

  indexes {
    (question_id, language_code) [unique]
  }
}

Table answers { // Possible answers for a question
  id uuid [pk, default: `uuid_generate_v4()`]
  question_id uuid [ref: > questions.id, not null]
  is_correct boolean [not null]
  display_order int [not null, default: 0]
  feedback_if_selected text [note: 'Feedback specific to choosing this answer']
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table answer_translations {
  id uuid [pk, default: `uuid_generate_v4()`]
  answer_id uuid [ref: > answers.id, not null]
  language_code varchar(10) [not null]
  answer_text text [not null]

  indexes {
    (answer_id, language_code) [unique]
  }
}

// Content Organization
Table categories {
  id uuid [pk, default: `uuid_generate_v4()`]
  parent_category_id uuid [ref: > categories.id, null]
  // name and description moved to translations
  is_active boolean [default: true]
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table category_translations {
  category_id uuid [ref: > categories.id, not null]
  language_code varchar(10) [not null]
  name varchar [not null]
  description text

  indexes {
    (category_id, language_code) [pk]
  }
}

Table course_categories { // Many-to-many: courses to categories
  course_id uuid [ref: > courses.id, not null] // Could also be course_version_id if categories are version specific
  category_id uuid [ref: > categories.id, not null]

  indexes {
    (course_id, category_id) [pk]
  }
}

Table tags {
  id uuid [pk, default: `uuid_generate_v4()`]
  // name moved to translations
  is_active boolean [default: true]
  created_at timestamp [default: `now()`]
  updated_at timestamp
}

Table tag_translations {
  tag_id uuid [ref: > tags.id, not null]
  language_code varchar(10) [not null]
  name varchar [not null, unique] // Tag names usually unique per language

  indexes {
    (tag_id, language_code) [pk]
    (language_code, name) [unique] // Ensure tag name is unique within a language
  }
}

Table course_tags { // Many-to-many: courses to tags
  course_id uuid [ref: > courses.id, not null] // Could also be course_version_id
  tag_id uuid [ref: > tags.id, not null]

  indexes {
    (course_id, tag_id) [pk]
  }
}
```