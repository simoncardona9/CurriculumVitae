# Services Definition

## Purpose

Define backend application services and use cases.

Services coordinate domain rules, validation, persistence, and export generation. They should not contain presentation layout details.

## Technology Decision

Backend services will be implemented in Java with Spring Boot.

Expected Spring responsibilities:

- Dependency injection for services, repositories, strategies, factories, and adapters.
- Bean Validation integration for request and command validation.
- Transaction management when database persistence is introduced.
- Configuration management for export paths, templates, and persistence settings.

Service classes should remain framework-light. They can be Spring beans, but business logic should not depend directly on HTTP, controller DTOs, or database-specific APIs.

## Initial Services

### Profile Service

- Manage person data.
- Manage contact methods.
- Manage professional summaries.
- Enforce visibility rules for sensitive personal information.

### Experience Service

- Manage companies.
- Manage work experience records.
- Manage achievements.
- Associate technologies with work experience.
- Sort experience in reverse chronological order for exports.

### Skill Service

- Manage technologies.
- Manage current skill profile.
- Filter highlighted skills.
- Warn when skills are stale or missing `last_used_year`.

### CV Variant Service

- Create and update CV variants.
- Manage section visibility and ordering.
- Select summary by variant.
- Validate that a variant references existing data.

### Export Service

- Validate source data before export.
- Render Markdown output.
- Render HTML output.
- Store export metadata when persistence exists.

## Transaction Boundaries

- Profile updates should be atomic per person.
- Experience updates should be atomic per work experience and related technologies.
- Variant updates should be atomic for the variant and its section definitions.
- Export generation should not modify source data.

## Acceptance Criteria

- Each service has a narrow responsibility.
- Services validate references before saving or exporting.
- Export services reuse the same visibility and section-ordering rules.
