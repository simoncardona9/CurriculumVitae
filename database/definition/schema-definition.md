# Schema Definition

## Purpose

Define the future relational schema.

## Initial Tables

- `persons`
- `contact_methods`
- `professional_summaries`
- `education`
- `certifications`
- `companies`
- `work_experiences`
- `work_achievements`
- `technologies`
- `work_experience_technologies`
- `skills`
- `languages`
- `references`
- `cv_variants`
- `cv_variant_sections`
- `exports`

## Key Relationships

- `contact_methods.person_id` references `persons.id`.
- `professional_summaries.person_id` references `persons.id`.
- `work_experiences.person_id` references `persons.id`.
- `work_experiences.company_id` references `companies.id`.
- `work_achievements.work_experience_id` references `work_experiences.id`.
- `skills.person_id` references `persons.id`.
- `skills.technology_id` references `technologies.id`.
- `cv_variants.person_id` references `persons.id`.
- `cv_variants.summary_id` references `professional_summaries.id`.
- `cv_variant_sections.cv_variant_id` references `cv_variants.id`.
- `exports.cv_variant_id` references `cv_variants.id`.

## Constraints

- Technology normalized names should be unique.
- Contact method type and visibility should use constrained values.
- CV variant section type should use constrained values.
- `sort_order` should be present where manual ordering matters.

## Acceptance Criteria

- Schema supports all Phase 1 export needs.
- Schema can be implemented in SQLite or PostgreSQL.
- Constraints protect data needed for correct exports.
