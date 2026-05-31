# Components Definition

## Purpose

Define reusable frontend components.

## Initial Components

- `ContactMethodList`: repeatable contact rows with type, value, primary flag, and visibility.
- `VisibilitySelect`: consistent visibility control for public, private, export-only, hidden, and available-on-request values.
- `SummaryEditor`: language-aware professional summary editor.
- `ExperienceTimeline`: ordered work experience list.
- `TechnologyPicker`: searchable normalized technology selector.
- `SkillTable`: editable skill matrix.
- `SectionOrderEditor`: drag or button-based section ordering.
- `VariantPreview`: read-only preview of selected CV variant.
- `ExportButtonGroup`: export actions for Markdown and HTML.

## Component Rules

- Components should not hardcode person-specific data.
- Components should receive data through props or API-backed state.
- Components should expose clear save and cancel behavior for editable records.
- Components should use shared validation display patterns.

## Acceptance Criteria

- Repeated UI patterns are implemented once.
- Visibility controls behave consistently across screens.
- Variant preview can be reused from the variants and exports screens.
