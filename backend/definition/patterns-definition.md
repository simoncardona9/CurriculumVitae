# Backend Patterns Definition

## Purpose

Define the backend design patterns that the project will use intentionally.

Patterns should solve concrete problems in the CV information system. They should not be added just because they are familiar.

## Recommended Patterns

### Repository / DAO

Use for persistence boundaries.

Applies to:

- Loading and saving YAML in version 1.
- Replacing YAML with database persistence in later versions.
- Keeping services independent from concrete storage details.

Examples:

- `PersonRepository`
- `WorkExperienceRepository`
- `TechnologyRepository`
- `CvVariantRepository`

Rule:

- Services depend on repository interfaces, not YAML parsing, SQL, JPA, or database-specific APIs.

### Strategy

Use when the system has multiple interchangeable algorithms.

Applies to:

- Export formats: Markdown, HTML, PDF, DOCX, JSON.
- Template selection.
- Visibility evaluation if rules become different by target country, language, or export type.

Examples:

- `ExportStrategy`
- `MarkdownExportStrategy`
- `HtmlExportStrategy`
- `VisibilityStrategy`

Rule:

- Use Strategy when behavior really varies. Do not create one strategy interface for a behavior that has only one stable implementation and no expected variation.

### Facade

Use to provide one simple entry point over several services.

Applies to:

- Export generation, because exporting may need profile data, variant configuration, visibility filtering, template rendering, and export history.
- CV preview generation, because the frontend should not coordinate every backend detail directly.

Examples:

- `CvExportFacade`
- `CvPreviewFacade`

Rule:

- A facade coordinates use cases. It should not become a place where all business rules accumulate.

### Specification

Use for composable business rules that answer yes/no questions.

Applies to:

- Whether a field or item is exportable.
- Whether a skill belongs in a CV variant.
- Whether a section should be rendered.
- Whether a reference can be exposed.
- Whether a work experience or project matches a target role.

Examples:

- `IsPublicVisibilitySpecification`
- `BelongsToVariantSpecification`
- `CanExportReferenceSpecification`
- `MatchesTargetRoleSpecification`

Rule:

- Use Specification for domain decisions and filtering logic that must be reused or combined. Keep simple one-off validations as normal methods.

### Factory

Use when object creation depends on type, format, or configuration.

Applies to:

- Selecting export strategies.
- Selecting templates.
- Creating validators for a format or variant.

Examples:

- `ExportStrategyFactory`
- `TemplateRendererFactory`

Rule:

- Factories should create or select collaborators. They should not execute business workflows.

### Builder

Use when creating records, DTOs, or immutable domain objects with many fields, optional fields, or nested collections.

Applies to:

- Java records used as domain data carriers.
- Request and response DTOs with many optional fields.
- Export view models.
- Test fixtures for domain objects.
- Nested structures such as `WorkExperience` with projects, achievements, and technologies.

Examples:

- `PersonRecord.builder()`
- `WorkExperienceRecord.builder()`
- `ProjectRecord.builder()`
- `CvVariantRecord.builder()`
- `ExportViewModel.builder()`

Rule:

- Prefer direct constructors only for very small records with two or three obvious required fields.
- Use builders for records with optional values, nullable values, collections, or many same-type fields such as multiple `String` values.
- Builders should validate required fields before creating the object.
- Builders should make defensive copies of collections when building immutable records.
- Tests should use builders or fixture factories instead of long positional constructors.

### Adapter / Wrapper

Use to isolate third-party libraries or infrastructure details.

Applies to:

- YAML parsing.
- Template rendering.
- Future PDF generation.
- Future DOCX generation.
- File storage.
- Database-specific persistence details.

Examples:

- `YamlCvDataSource`
- `TemplateRendererAdapter`
- `PdfRendererAdapter`
- `FileStorageAdapter`

Rule:

- Use adapters when the code touches external tools, libraries, file formats, or infrastructure APIs. This keeps the domain and services easier to test.

### DTO / Mapper

Use to separate API contracts from domain objects.

Applies to:

- Controller request bodies.
- Controller responses.
- Export view models.
- Frontend-specific API payloads.

Examples:

- `ProfileResponse`
- `WorkExperienceRequest`
- `CvVariantResponse`
- `ExportViewModel`
- `WorkExperienceMapper`

Rule:

- Controllers should not expose persistence entities directly.

### Cache

Use for repeated lookup data.

Applies to:

- Technologies by id.
- Technologies by normalized name.
- Possibly companies by normalized name later.

Examples:

- `TechnologyCache`

Rule:

- Cache only data with clear invalidation rules. Do not cache user-edited data without defining refresh behavior.

## Patterns To Avoid Initially

### Abstract Factory

Avoid until there are families of related objects that must be created together.

### Observer / Event Bus

Avoid in the MVP. Use direct service calls first. Consider domain events later for audit logging, export history, or asynchronous generation.

### Command Pattern

Avoid for normal CRUD in the MVP. Consider it later if undo, queued jobs, or audit replay becomes necessary.

### Generic Wrapper Everywhere

Avoid wrapping every class. Use adapters only at external boundaries.

### Inheritance-Heavy Template Method

Avoid deep inheritance for exporters. Prefer Strategy plus composition.

## Initial Pattern Map

```text
Controllers
  -> DTO / Mapper
  -> Services

Services
  -> Repository / DAO
  -> Specification
  -> Facade for larger workflows

Export
  -> Facade
  -> Strategy
  -> Factory
  -> Builder for export view models
  -> Adapter for template/file/PDF/DOCX tools

Persistence
  -> Repository / DAO
  -> Adapter for YAML or database implementation
  -> Cache for technology lookup
```

## Acceptance Criteria

- Each backend pattern has a clear reason to exist.
- Services remain independent from persistence technology.
- Export format variation is handled through Strategy.
- Export workflow coordination is handled through a Facade.
- Reusable export and filtering rules are modeled with Specification.
- Records and complex DTOs are created through Builder when construction is non-trivial.
- Third-party libraries and file/database APIs are isolated behind adapters.
- Patterns do not create unnecessary layers for simple one-use behavior.
