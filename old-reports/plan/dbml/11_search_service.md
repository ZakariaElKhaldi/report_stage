# Search Service DBML

Minimal relational DBML for the Search Service. The primary data store is the search engine index itself (e.g., Elasticsearch, MeiliSearch).

```dbml
// Search Service DBML (Minimal relational store, main data in Search Engine)
Project search_service {
  database_type: 'PostgreSQL (for operational data, not the search index itself)'
}

Table indexed_document_sources { // Tracks what kind of documents are being indexed
  source_key varchar [pk, note: 'e.g., courses, users, consultants']
  description text
  last_indexed_at timestamp [null, note: 'When this source was last fully re-indexed']
  // indexing_status varchar [default: 'idle', note: 'e.g., idle, indexing, error']
  // error_message text [null]
}

Table search_synonyms {
  id uuid [pk, default: `uuid_generate_v4()`]
  term varchar [not null]
  synonyms text[] [not null, note: 'Array of synonyms for the term']
  language_code varchar(10) [not null, default: 'en']
  is_two_way boolean [default: true, note: 'If true, synonyms also map back to term']
  created_at timestamp [default: `now()`]
  updated_at timestamp

  indexes {
    (term, language_code) [unique]
  }
}
```