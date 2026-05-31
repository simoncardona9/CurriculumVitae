# Migration Definition

## Purpose

Define how database schema changes will be introduced once persistence moves beyond YAML.

## Migration Strategy

- Use versioned migrations.
- Keep migrations deterministic and repeatable.
- Keep seed data separate from schema migrations when possible.
- Use local development migrations before production migrations exist.

## Versioning Rules

- Each schema change gets a new migration file.
- Migration names should describe the change.
- Avoid editing already-applied migrations after they are shared.

## Seed Data

- Initial seed data may include the first person profile, technologies, and default CV variant.
- Sensitive personal data should not be committed as seed data unless the repository is private and intentional.

## Rollback Policy

- Prefer forward-fix migrations.
- Define rollback only for simple reversible changes.
- Back up data before destructive schema changes.

## Acceptance Criteria

- A new environment can create the schema from migrations.
- Seed data can populate a useful local development profile.
- Migration history is clear enough to audit.
