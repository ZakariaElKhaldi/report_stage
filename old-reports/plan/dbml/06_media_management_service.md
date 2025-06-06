# Media Management Service DBML

DBML schema for the Media Management Service. Manages metadata for assets likely stored in Supabase Storage or similar.

```dbml
// Media Management Service DBML
Project media_service {
  database_type: 'PostgreSQL (metadata store for Supabase Storage)'
}

Table media_assets {
  id uuid [pk, default: `uuid_generate_v4()`] // Internal ID for this service
  // folder_id uuid [ref: > media_folders.id, null] // If you implement folders
  uploader_user_id uuid [not null, note: 'User ID from IAM service who uploaded it']
  // For linking to content, these IDs come from Course Service context
  // This service might not know about courses/lessons directly, but stores these for context if provided at upload
  // associated_course_id uuid [null, note: 'Course ID from Course Service, for context']
  // associated_lesson_id uuid [null, note: 'Lesson ID from Course Service, for context']

  storage_provider varchar [default: 'supabase', not null] // e.g., supabase, s3_custom
  storage_bucket_name varchar [not null] // e.g., 'course-videos', 'profile-pictures'
  storage_path varchar [unique, not null, note: 'Full path to the file in the storage provider bucket']

  original_file_name varchar [not null]
  mime_type varchar [not null]
  file_size_bytes bigint [not null]
  // For videos
  duration_seconds int [null]
  width_pixels int [null]
  height_pixels int [null]
  // For images
  // image_width_pixels int [null]
  // image_height_pixels int [null]

  // status varchar [default: 'uploading', note: 'uploading, processing, available, error']
  // processing_details jsonb [note: 'Info about transcoding, etc. if applicable']

  // public_url text [null, note: 'Direct public URL if asset is public, usually not for course content']
  // Instead of public_url, this service would generate signed URLs on demand.

  // tags text[] // Generic tags for the media asset itself
  // description text

  created_at timestamp [default: `now()`]
  updated_at timestamp
  is_deleted boolean [default: false]
  deleted_at timestamp

  indexes {
    (uploader_user_id)
    (mime_type)
    // (associated_course_id) // If you need to query media by course directly here
    // (associated_lesson_id) // If you need to query media by lesson directly here
  }
}

// Optional: Folders for organizing media if Course Creators need this
Table media_folders {
  id uuid [pk, default: `uuid_generate_v4()`]
  creator_user_id uuid [not null, note: 'User ID from IAM service']
  parent_folder_id uuid [ref: > media_folders.id, null]
  name varchar [not null]
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (creator_user_id, parent_folder_id, name) [unique] // Ensure unique folder names within a parent for a user
  }
}
```