# Frontend Definition

## Purpose

Define the frontend architecture and interaction model for the CV information system.

The UI should behave like a structured professional profile editor, not a document editor.

## Initial Architecture

- Route-based application with main navigation sections.
- Forms for structured profile editing.
- Tables for repeatable records such as skills, contacts, education, certifications, and references.
- Timeline-oriented experience management.
- Preview panel for CV variants.
- Export screen for generated outputs.

## Main Navigation

- Profile.
- Experience.
- Education.
- Skills.
- Certifications.
- Languages.
- References.
- CV Variants.
- Exports.

## State Management

- Keep form state local to each screen until saved.
- Keep selected CV variant as shared application state.
- Fetch reference data such as technologies and companies through dedicated API calls.

## Validation Behavior

- Validate required fields before submit.
- Show server validation errors next to the relevant field when possible.
- Warn before exposing sensitive fields in an export.

## Acceptance Criteria

- Users can edit structured data without touching raw YAML or JSON.
- The UI makes visibility settings clear for sensitive information.
- The preview reflects the selected variant and section order.
