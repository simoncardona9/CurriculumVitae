# Backend Security Definition

## Purpose

Define backend security requirements for the CV information system.

The system stores sensitive personal information, contact data, references, and export history. Security rules must protect that data by default and make exposure intentional.

## Initial Security Scope

- Authentication for future web/API access.
- Authorization for profile and export operations.
- Sensitive field visibility.
- API validation and input handling.
- Export privacy controls.
- Audit behavior for sensitive operations.
- Local development security defaults.

## Authentication

The first command-line MVP does not require user authentication.

When the backend API is introduced:

- All write endpoints should require authentication.
- Export generation should require authentication.
- Read endpoints should require authentication unless a public sharing feature is explicitly designed.
- Password-based authentication should not be added unless the project needs first-party accounts.
- If external authentication is used, prefer standards such as OAuth2 or OpenID Connect.

## Authorization

Initial assumption: one CV owner manages one profile.

Future multi-user behavior:

- A user can only access profiles they own or have been granted permission to manage.
- Admin permissions, if added, must be explicit.
- Export history should be visible only to users allowed to view the related CV variant.
- References and identity fields require stricter access than normal profile data.

## Sensitive Data

Sensitive fields include:

- Identity document.
- Date of birth.
- Civil status.
- Phone and mobile numbers.
- Personal references.
- Work references.
- Full address, if added later.
- Export files containing private information.

Rules:

- Sensitive fields are private by default.
- Public exports must exclude private fields.
- References must not be exported with names and phone numbers unless explicitly enabled by the selected CV variant.
- Logs must not print sensitive field values.
- Validation messages should identify invalid fields without echoing private values.

## API Protection

- Validate all request bodies.
- Reject unknown enum values for visibility, level, section type, and export format.
- Use server-side validation even when the frontend also validates.
- Return consistent error responses.
- Avoid exposing stack traces in API responses.
- Apply request size limits to import/export endpoints.
- Use HTTPS in any deployed environment.

## Export Security

- Export commands and endpoints must apply visibility rules before rendering.
- Exported files should be written to a controlled `exports/` location.
- Generated filenames should avoid unsafe user input.
- Existing files should not be overwritten unless the caller explicitly uses a force option.
- Private export variants should be clearly marked in metadata or filename conventions when persistence exists.

## Audit Behavior

When database persistence exists, audit the following events:

- Profile updates.
- Sensitive field updates.
- Reference updates.
- CV variant changes that expose private data.
- Export generation.
- Export deletion.

Audit records should include:

- Event type.
- User id, when authentication exists.
- Related entity id.
- Timestamp.
- Result status.

Audit records should not store sensitive values directly.

## Local Development Defaults

- Do not commit generated exports that contain sensitive personal data.
- Do not commit real secrets.
- Use environment variables or local ignored config files for secrets.
- Use sample data when tests need personal information.

## Acceptance Criteria

- Backend services can determine whether a field is exportable.
- Default exports do not expose identity document, birth date, civil status, or references.
- API endpoints validate input before calling service logic.
- Error responses do not leak sensitive data or stack traces.
- Future authentication and authorization rules are documented before API implementation begins.
