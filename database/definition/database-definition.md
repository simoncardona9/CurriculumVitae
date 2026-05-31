# Database Definition

## Purpose

Define database-level decisions and persistence principles.

The first MVP starts with YAML, but the database model should be ready for a later SQLite or PostgreSQL implementation.

## Recommended Path

1. YAML file for the first command-line exporter.
2. SQLite for a local editable prototype.
3. PostgreSQL for a long-term web application.

## Naming Conventions

- Use snake_case for table and column names.
- Use singular table names only if the selected framework standardizes on them; otherwise prefer plural names.
- Use stable ids for records that may be referenced by variants.
- Use `created_at` and `updated_at` where persistence supports audit fields.

## Persistence Principles

- Preserve historical data unless explicitly removed.
- Normalize technologies and companies.
- Keep visibility fields close to sensitive or exportable data.
- Avoid storing rendered CV output as the source of truth.

## Acceptance Criteria

- The database design can represent the full YAML source data.
- The design supports multiple CV variants from one profile.
- The design supports deterministic exports.
