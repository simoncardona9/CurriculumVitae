# Logging And Exceptions Definition

## Purpose

Define how the backend logs system behavior and how custom exceptions are created, propagated, and exposed.

Logs should help diagnose problems without revealing sensitive CV information. Exceptions should describe system failures clearly without leaking private data or implementation details to API clients.

## Logging Goals

- Help debug validation, persistence, export, and API errors.
- Show enough context to trace a request or export workflow.
- Avoid exposing sensitive personal information.
- Keep log messages consistent and searchable.
- Separate expected business failures from unexpected system failures.

## Sensitive Data Rules

Logs must not include raw values for:

- Identity document.
- Date of birth.
- Civil status.
- Full phone numbers.
- Email addresses.
- Reference names.
- Reference phone numbers.
- Full address.
- Generated export file contents.
- Any field marked `private`, `hidden`, or `available_on_request`.

Allowed log context:

- Entity type.
- Entity id.
- CV variant id.
- Export id.
- Section type.
- Technology id or normalized non-sensitive technology name.
- Error code.
- Request correlation id.
- User id when authentication exists.

If a sensitive value is needed for debugging, log only a masked form.

Examples:

```text
email=sc***@gmail.com
mobile=+598*****6508
identity_document=***hidden***
reference_phone=***hidden***
```

## Log Levels

### ERROR

Use for failures that prevent completing a requested operation.

Examples:

- Export generation failed.
- Data file cannot be loaded.
- Database operation failed.
- Unexpected exception reached a global handler.

### WARN

Use for recoverable problems or suspicious input.

Examples:

- Validation warnings.
- Stale skills detected.
- Export skipped a hidden section.
- A requested optional reference was not exportable because of visibility rules.

### INFO

Use for important lifecycle events.

Examples:

- Application started.
- Validation completed.
- Export generated.
- CV variant updated.

### DEBUG

Use for developer diagnostics.

Examples:

- Selected export strategy.
- Template selected.
- Number of sections rendered.
- Number of skills filtered.

DEBUG logs must still follow sensitive data rules.

### TRACE

Avoid in the first implementation unless there is a concrete need.

## Structured Logging

Prefer structured key-value logging over long free-text messages.

Recommended fields:

- `event`
- `correlation_id`
- `user_id`
- `person_id`
- `cv_variant_id`
- `export_id`
- `entity_type`
- `entity_id`
- `error_code`
- `status`
- `duration_ms`

Example:

```text
event=cv_export_generated correlation_id=abc123 cv_variant_id=default-es format=markdown status=success duration_ms=42
```

## Correlation Id

When the API exists, each request should have a correlation id.

Rules:

- Accept an incoming correlation id header if present.
- Generate one if missing.
- Include it in logs.
- Include it in API error responses.
- Pass it through service and export operations.

Suggested header:

```text
X-Correlation-Id
```

## Exception Design Goals

- Use custom exceptions for expected domain, validation, persistence, and export failures.
- Keep exception names specific enough to guide handling.
- Include safe metadata such as ids and error codes.
- Do not include sensitive field values in exception messages.
- Map exceptions consistently to API responses.

## Exception Hierarchy

Suggested base hierarchy:

```text
CvSystemException
  DomainException
    InvalidVisibilityException
    InvalidCvVariantException
    ExportNotAllowedException
  ValidationException
    MissingRequiredFieldException
    InvalidReferenceException
    DuplicateTechnologyException
  PersistenceException
    CvDataLoadException
    CvDataSaveException
    RepositoryOperationException
  ExportException
    ExportGenerationException
    TemplateRenderException
    UnsupportedExportFormatException
  SecurityException
    UnauthorizedOperationException
    ForbiddenDataAccessException
```

## Exception Fields

Custom exceptions should carry safe diagnostic information.

Suggested fields:

- `errorCode`
- `message`
- `entityType`
- `entityId`
- `fieldName`
- `correlationId`
- `cause`

Rules:

- `message` must be safe for logs.
- `fieldName` can identify the field, but not the private value.
- `entityId` is allowed when it is not itself sensitive.
- `cause` should be preserved for server logs, but not exposed directly to API clients.

## Error Codes

Use stable error codes so frontend and tests do not depend on message text.

Initial examples:

```text
CV_VALIDATION_MISSING_REQUIRED_FIELD
CV_VALIDATION_INVALID_REFERENCE
CV_TECHNOLOGY_DUPLICATE_NORMALIZED_NAME
CV_VARIANT_NOT_FOUND
CV_VARIANT_INVALID_SECTION
CV_EXPORT_UNSUPPORTED_FORMAT
CV_EXPORT_GENERATION_FAILED
CV_EXPORT_FORBIDDEN_PRIVATE_DATA
CV_PERSISTENCE_LOAD_FAILED
CV_PERSISTENCE_SAVE_FAILED
CV_SECURITY_UNAUTHORIZED
CV_SECURITY_FORBIDDEN
```

## API Error Mapping

When REST controllers exist, map exceptions to HTTP responses consistently.

```text
ValidationException -> 400 Bad Request
InvalidCvVariantException -> 400 Bad Request
DuplicateTechnologyException -> 409 Conflict
UnauthorizedOperationException -> 401 Unauthorized
ForbiddenDataAccessException -> 403 Forbidden
Entity not found exception -> 404 Not Found
UnsupportedExportFormatException -> 400 Bad Request
ExportGenerationException -> 500 Internal Server Error
PersistenceException -> 500 Internal Server Error
Unexpected exception -> 500 Internal Server Error
```

API error response shape:

```json
{
  "errorCode": "CV_VALIDATION_INVALID_REFERENCE",
  "message": "The request contains an invalid reference.",
  "field": "technology_id",
  "entityType": "Skill",
  "entityId": "skill-java",
  "correlationId": "abc123"
}
```

The response must not include stack traces, sensitive values, SQL, file contents, or template contents.

## Global Exception Handling

When using Spring Boot, define one global exception handler.

Responsibilities:

- Convert custom exceptions to API error responses.
- Add correlation id to error responses.
- Log server-side details safely.
- Avoid duplicate logging of the same exception at multiple layers.
- Return generic messages for unexpected exceptions.

Expected component:

```text
GlobalExceptionHandler
```

## Logging Responsibilities By Layer

### Controllers

- Log request start and completion at DEBUG only if needed.
- Do not log request bodies by default.
- Delegate exception mapping to the global exception handler.

### Services

- Log important business events at INFO.
- Throw custom exceptions for expected failures.
- Avoid logging and rethrowing the same exception unless additional safe context is added.

### Repositories / DAO

- Log persistence failures with entity type and safe ids.
- Wrap infrastructure exceptions in persistence exceptions.
- Do not log raw YAML contents or SQL parameter values that may contain sensitive data.

### Exporters

- Log selected variant, format, template, and output path.
- Do not log rendered CV content.
- Log section counts and skipped sections only with safe metadata.

## Testing Requirements

- Unit tests should verify custom exception error codes.
- Controller tests should verify exception-to-HTTP mapping.
- Logging tests should verify that sensitive values are not logged for representative failures.
- Export tests should verify that private fields do not appear in logs or output.

## Acceptance Criteria

- Logs help identify what operation failed and where.
- Logs never expose private CV values.
- Custom exceptions use stable error codes.
- API errors expose safe messages and correlation ids.
- Infrastructure errors are wrapped before reaching services or controllers.
- Unexpected exceptions produce generic client responses and detailed safe server logs.
