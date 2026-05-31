# Screens Definition

## Purpose

Define screen-level requirements for the frontend.

## Profile Screen

- Edit name, headline, personal information, and professional summaries.
- Manage contact methods as repeatable rows.
- Configure visibility for sensitive fields.

## Experience Screen

- Show work experience as a reverse chronological timeline.
- Edit company, role, dates, summary, achievements, methodology, and technologies.
- Mark current job with an explicit current flag.

## Skills Screen

- Show skills in a filterable table.
- Edit level, years of experience, last used year, and highlight flag.
- Filter by technology category.

## CV Variants Screen

- Manage variant name, language, target role, template, and summary.
- Reorder sections.
- Toggle section visibility.
- Select highlighted skills and included experiences.

## Export Screen

- Generate Markdown and HTML exports for the selected variant.
- Show export history when persistence is available.
- Provide links to generated files.

## Acceptance Criteria

- Each major data area has an editable screen.
- Screens support both full CV and short variant workflows.
- Empty states explain what action is available without replacing the main workflow.
