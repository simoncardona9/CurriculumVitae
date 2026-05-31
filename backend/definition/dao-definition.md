# DAO Definition

## Purpose

Define repository and data access responsibilities for the backend.

The first MVP may use YAML files, but these DAO boundaries should guide the future SQLite or PostgreSQL implementation.

## Persistence Strategy

The first version of the system will use YAML as the persistence source.

This decision keeps the MVP simple, easy to inspect, and easy to version while the domain model and export rules are still being validated.

Later versions should move to a database-backed implementation, but backend services must not be attached to the specific storage solution used. The application should depend on repository and cache contracts, not on YAML parsing, SQL queries, JPA, SQLite, PostgreSQL, or any other concrete persistence detail.

Required approach:

- Version 1 uses YAML repositories that load and save structured files.
- Future versions can replace YAML repositories with database repositories.
- Service code should keep using the same repository interfaces.
- Database choice should be isolated to infrastructure configuration and DAO implementation classes.
- Exporters and validators should receive already-loaded domain data or use repository contracts.
- Tests should be able to use in-memory or fixture-backed repositories without changing service behavior.

The intended evolution is:

```text
Phase 1: YAML files
Phase 2: Local database implementation, likely SQLite
Phase 3: Production-ready relational database, likely PostgreSQL
```

The exact database can change later without changing domain rules, service contracts, or controller behavior.

## Initial Repository Interfaces

- `PersonRepository`
- `ContactMethodRepository`
- `ProfessionalSummaryRepository`
- `EducationRepository`
- `CertificationRepository`
- `CompanyRepository`
- `WorkExperienceRepository`
- `TechnologyRepository`
- `TechnologyCache`
- `SkillRepository`
- `LanguageRepository`
- `ReferenceRepository`
- `CvVariantRepository`
- `ExportRepository`

## Query Expectations

- Load a complete CV profile by person id.
- Load a CV variant by id with sections ordered by `sort_order`.
- Load work experience by person id in reverse chronological order.
- Load skills by person id with optional category and highlight filters.
- Load technologies by id and by normalized name.
- Load technologies through a cache when resolving repeated skill, work experience, and project references.
- Load export history by variant id.

## Technology Cache Definition

Technology records are reused by skills, work experience, and projects. The DAO layer should provide a small cache abstraction so repeated lookups do not duplicate normalization logic or repeatedly scan YAML/database records.

### Responsibilities

- Cache technologies by `id`.
- Cache technologies by normalized name.
- Provide lookup methods used by validators, services, and exporters.
- Keep normalization rules in one place.
- Avoid creating duplicate records for values such as `Git`, `GIT`, and `git`.

### Suggested Interface

```text
TechnologyCache
  getById(id)
  getByNormalizedName(name)
  getAll()
  refresh()
  evict(id)
  clear()
```

### Normalization Rules

- Trim leading and trailing whitespace.
- Compare names case-insensitively.
- Collapse repeated internal whitespace.
- Preserve original display name from the `Technology` record.
- Do not infer aliases unless an explicit alias list is added later.

### Cache Lifecycle

For the YAML MVP:

- Build the cache after loading `cv-data.yaml`.
- Treat the cache as read-only during export.
- Rebuild the cache when the YAML file is reloaded.

For the database-backed version:

- Populate the cache lazily or at application startup.
- Evict or refresh cached entries when a technology is created, updated, merged, or deleted.
- Clear the cache during test setup to prevent cross-test contamination.

### Invalidation Rules

- Creating a technology adds or refreshes the cached `id` and normalized name.
- Updating a technology evicts the old normalized name and stores the new normalized name.
- Deleting a technology evicts it by `id` and normalized name.
- Merging technologies evicts all replaced records and refreshes the surviving record.

## Data Access Rules

- Repositories should not render CV content.
- Repositories should not decide export visibility.
- Repositories should preserve historical records unless explicitly deleted.
- Normalized names should prevent duplicate technology records such as `Git` and `GIT`.
- Technology normalization and cache lookup should be centralized in `TechnologyCache`.
- Services and exporters should not build their own technology maps unless they use `TechnologyCache`.

## Acceptance Criteria

- DAO interfaces support the first export workflow.
- DAO interfaces can be implemented using YAML first and database persistence later.
- Query responsibilities are explicit and testable.
- Technology lookups use a shared cache abstraction.
- Cache invalidation behavior is defined before database persistence is implemented.
