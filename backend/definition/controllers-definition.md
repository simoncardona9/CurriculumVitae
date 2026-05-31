# Controllers Definition

## Purpose

Define the future REST API surface for the CV information system.

Controllers expose application use cases through HTTP. They should validate request shape, call services, and return consistent responses.

## Initial Controller Areas

### Profile Controller

- `GET /api/profile`
- `PUT /api/profile`
- `GET /api/profile/contacts`
- `POST /api/profile/contacts`
- `PUT /api/profile/contacts/{id}`
- `DELETE /api/profile/contacts/{id}`

### Experience Controller

- `GET /api/experience`
- `POST /api/experience`
- `GET /api/experience/{id}`
- `PUT /api/experience/{id}`
- `DELETE /api/experience/{id}`

### Skills Controller

- `GET /api/skills`
- `POST /api/skills`
- `PUT /api/skills/{id}`
- `DELETE /api/skills/{id}`
- `GET /api/technologies`
- `POST /api/technologies`

### CV Variants Controller

- `GET /api/cv-variants`
- `POST /api/cv-variants`
- `GET /api/cv-variants/{id}`
- `PUT /api/cv-variants/{id}`
- `PUT /api/cv-variants/{id}/sections`

### Export Controller

- `POST /api/exports/markdown`
- `POST /api/exports/html`
- `GET /api/exports`
- `GET /api/exports/{id}`

## Response Rules

- Use `200 OK` for successful reads and updates.
- Use `201 Created` for created resources.
- Use `204 No Content` for successful deletes.
- Use `400 Bad Request` for invalid request data.
- Use `404 Not Found` for missing resources.
- Use `409 Conflict` for duplicate normalized records.

## Acceptance Criteria

- Endpoints map cleanly to service responsibilities.
- Request and response DTOs avoid exposing internal persistence details.
- Validation errors are returned in a consistent structure.
