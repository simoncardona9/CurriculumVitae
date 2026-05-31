# CV Information System Plan

## Purpose

Create a system that stores all curriculum vitae information as structured data and can export that information into one or more CV formats.

The current source document is `Currículum Vitae Simon Cardona - Español.docx`. It contains enough information to model a complete professional profile, including personal details, objective, education, certifications, work experience, technical skills, languages, and references.

## Main Goals

- Store CV data once and reuse it across multiple resume versions.
- Keep full historical information, even if only part of it is exported to a specific CV.
- Export the CV to common formats such as PDF, DOCX, Markdown, HTML, and JSON.
- Support different CV variants, for example Spanish, English, backend-focused, cloud-focused, short version, and full version.
- Make it easy to update work experience, skills, technologies, and contact information.
- Separate raw information from presentation/layout so the same data can feed many templates.

## Current CV Information Inventory

### Personal Profile

- Full name: Simón Manuel Cardona Posso
- Nationality: Colombian / legal Uruguayan nationality
- Age: 41
- Place of birth: Colombia
- Date of birth: 07/02/1983
- ID: 4.314.721-7
- Civil status: Married
- Phone: 22007455
- Mobile: +59899816508
- Email: scardona9@gmail.com
- LinkedIn: https://uy.linkedin.com/in/simon-cardona-8545b212

### Professional Objective

Use broad software development and systems engineering experience to contribute to an employer's success while continuing professional growth.

### Education

- 2011 - 2017: Universidad ORT del Uruguay, Systems Engineering degree obtained.
- 2003 - 2010: Facultad de Ingeniería, Universidad de la República, 1st, 2nd, 3rd years.
- 1996 - 2001: Liceo Solymar 1, Scientific orientation - Engineering.
- 1990 - 1995: Escuela No. 229 de Lagomar, Ciudad de la Costa.

### Certifications

- Microsoft .NET Framework 2.0 - Application Development Foundation.
- ORACLE SQL: 1ZO-001 Introduction to Oracle SQL & PL/SQL.

### Work Experience

- 2023 - Now: Dualboot Partners.
  - Development of solutions using Java AWS SDK.
  - Technologies: Java, AWS SDK, Gradle, AWS, Git, PostgreSQL.
  - Methodology: Scrum.
- 2019 - 2023: Intraway.
  - Software creation for telecommunications companies.
  - Technologies: Java, Spring Boot, Kafka, Docker, REST.
  - Methodology: Scrum.
- 2014 - 2019: Globant.
  - Maintenance and development of projects for a Canadian company.
  - Technologies: Java, JavaScript, Node.js, Spring MVC, Hibernate, SQL, Struts, REST, PostgreSQL.
  - Methodology: Agile / Scrum.
- 2011 - 2014: ST Consultores.
  - Development of solutions for the Uruguayan government utility company UTE.
  - Technologies: Java, JavaScript, SQL, CC&B.
- 2007 - 2011: TCS / Tata Consultancy Services.
  - Work on several projects for government, telecommunications, and large supermarket-chain clients.
  - Technologies: Java, J2EE, PL/SQL, SQL, .NET.

### Skills

The current CV stores skills with three fields: skill name, level, and experience.

Examples:

- Java: Expert, 14 years.
- Spring Boot: Expert, 5 years.
- Spring: Expert, 6.5 years.
- Git: Expert, 6 years.
- SQL: Expert, 5.5 years.
- Agile Software Development: Expert, 10 years.
- AWS: Intermediate, 1.5 years.
- Docker: Intermediate, 3 years.
- Python: Beginner, 1 year.
- JavaScript: Intermediate, 3.5 years.

The complete skill list should be migrated into a `skills` table or collection.

### Languages

- English: B2 level according to the Common European Framework of Reference.

### References

Work reference:

- TCS Tata Consultancy Services. Phone: 2518 56 00.

Personal references:

- Arya Baher, Analyst. Mobile: +598 99 977 880.
- Ignacio Larrañaga, Engineer. Mobile: +598 99 636 955.
- Luis Enrique Morales, Engineer. Mobile: +598 94 360 550.
- Emilio Pombo, Engineer. Mobile: +598 99 727 386.

## Suggested Domain Model

### Core Entities

#### Person

Stores the owner of the CV.

Suggested fields:

- `id`
- `full_name`
- `preferred_name`
- `headline`
- `summary`
- `date_of_birth`
- `place_of_birth`
- `nationalities`
- `civil_status`
- `identity_document`
- `created_at`
- `updated_at`

#### Contact Method

Stores contact channels separately so they can be shown, hidden, or reordered per export.

Suggested fields:

- `id`
- `person_id`
- `type`: email, mobile, phone, linkedin, github, portfolio, location
- `label`
- `value`
- `url`
- `is_primary`
- `visibility`: public, private, export_only, hidden

#### Professional Summary

Stores multiple profile summaries or objectives for different CV variants.

Suggested fields:

- `id`
- `person_id`
- `title`
- `content`
- `language`
- `target_role`
- `is_default`

#### Education

Suggested fields:

- `id`
- `person_id`
- `institution`
- `degree_or_program`
- `field`
- `start_date`
- `end_date`
- `is_completed`
- `description`
- `location`
- `sort_order`

#### Certification

Suggested fields:

- `id`
- `person_id`
- `name`
- `issuer`
- `credential_id`
- `issue_date`
- `expiration_date`
- `url`
- `description`

#### Company

Suggested fields:

- `id`
- `name`
- `industry`
- `website`
- `location`

#### Work Experience

Suggested fields:

- `id`
- `person_id`
- `company_id`
- `job_title`
- `start_date`
- `end_date`
- `is_current`
- `summary`
- `methodology`
- `sort_order`

#### Work Achievement

Use this instead of storing only paragraphs. It allows the system to build stronger CV bullet points later.

Suggested fields:

- `id`
- `work_experience_id`
- `description`
- `impact`
- `metrics`
- `sort_order`
- `visibility`

#### Technology

Normalized list of technologies, frameworks, languages, tools, platforms, and methodologies.

Suggested fields:

- `id`
- `name`
- `category`: language, framework, database, cloud, tool, methodology, platform
- `description`

#### Work Experience Technology

Many-to-many relationship between jobs and technologies.

Suggested fields:

- `work_experience_id`
- `technology_id`
- `usage_context`
- `proficiency_at_time`

#### Skill

Represents the person's current skill profile.

Suggested fields:

- `id`
- `person_id`
- `technology_id`
- `level`: beginner, intermediate, advanced, expert
- `years_experience`
- `last_used_year`
- `is_highlighted`
- `notes`

#### Language

Suggested fields:

- `id`
- `person_id`
- `language`
- `level`
- `framework`: CEFR, native, custom
- `notes`

#### Reference

Suggested fields:

- `id`
- `person_id`
- `name`
- `relationship`
- `role`
- `company`
- `phone`
- `email`
- `type`: work, personal, academic
- `visibility`: hidden_by_default, available_on_request, export_allowed
- `notes`

#### CV Variant

Defines a specific resume version.

Suggested fields:

- `id`
- `person_id`
- `name`
- `language`
- `target_role`
- `target_country`
- `template_id`
- `summary_id`
- `created_at`
- `updated_at`

#### CV Variant Section

Controls which sections appear in each CV and in what order.

Suggested fields:

- `id`
- `cv_variant_id`
- `section_type`: profile, experience, education, skills, certifications, languages, references
- `title_override`
- `sort_order`
- `is_visible`

#### Export

Keeps history of generated files.

Suggested fields:

- `id`
- `cv_variant_id`
- `format`: pdf, docx, html, markdown, json
- `file_path`
- `generated_at`
- `template_version`
- `status`

## Possible Database Options

### Recommended Start: PostgreSQL

Best option if this will become a serious application with filters, variants, exports, and future integrations.

Pros:

- Strong relational model for CV data.
- Good support for structured data and JSON fields.
- Easy to query skills, jobs, technologies, dates, and variants.
- Works well with most backend frameworks.

Suggested use:

- Store normalized entities in relational tables.
- Use JSONB only for flexible template settings, not as the main data model.

### Simple Local Start: SQLite

Best option for an initial local prototype.

Pros:

- No server required.
- Easy to version and back up.
- Good enough for one-person CV management.

Limitations:

- Less ideal if the app later becomes multi-user or cloud-hosted.
- Some advanced search and concurrency features are weaker than PostgreSQL.

### Document Database: MongoDB

Useful if each CV profile is stored as a large document.

Pros:

- Natural fit for nested resume data.
- Flexible schema while the model is still changing.

Limitations:

- Harder to query consistently across skills, technologies, companies, and variants.
- Less strict data integrity unless carefully designed.

### File-Based Option: YAML or JSON

Good for a developer-focused first version.

Pros:

- Very simple.
- Easy to edit manually.
- Works well with static site generators or command-line exporters.

Limitations:

- Harder to build a rich UI.
- No built-in validation unless added separately.
- Not ideal for search, relationships, or export history.

## Recommended Architecture

### Phase 1

- Create a structured source file, for example `cv-data.json` or `cv-data.yaml`.
- Build one exporter that generates Markdown or HTML.
- Keep the system simple while the domain model is validated.

### Phase 2

- Move data into SQLite or PostgreSQL.
- Add a backend API.
- Add a web UI for editing all sections.
- Add PDF and DOCX exports.

### Phase 3

- Add CV variants by target role and language.
- Add template selection.
- Add version history and export history.
- Add import from DOCX or Markdown.

## Suggested Backend

Good technology options:

- Java + Spring Boot, matching existing Java/Spring experience.
- Node.js + TypeScript + NestJS, good for fast CRUD APIs and export services.
- Python + FastAPI, good for document generation and scripting.

Recommended fit:

- Backend: Java + Spring Boot.
- Database: PostgreSQL for long-term use, SQLite for first prototype.
- Export engine: server-side templates using HTML/CSS first, then PDF generation.

## Suggested UI

The UI should feel like a structured professional profile editor, not like a document editor.

### Main Navigation

- Profile
- Experience
- Education
- Skills
- Certifications
- Languages
- References
- CV Variants
- Exports

### Profile Screen

Purpose:

- Edit personal information, contact methods, headline, and professional summaries.

Important features:

- Contact methods as repeatable rows.
- Visibility controls for sensitive data such as ID, birth date, civil status, and references.
- Multiple summaries by language and target role.

### Experience Screen

Purpose:

- Manage jobs, projects, responsibilities, achievements, methodologies, and technologies.

Important features:

- Timeline list sorted by date.
- Separate fields for company, role, dates, description, achievements, and technologies.
- Technology picker using normalized technology records.
- Achievement bullets with optional metrics.

### Skills Screen

Purpose:

- Manage the skills matrix.

Important features:

- Table with skill, category, level, years of experience, last used, and highlight flag.
- Filters by category.
- Warnings for stale skills, for example a skill not used for several years.
- Ability to select which skills appear in each CV variant.

### CV Variants Screen

Purpose:

- Create different CV versions from the same stored data.

Important features:

- Variant name, language, target role, and template.
- Section visibility and order.
- Select which work experiences, skills, certifications, and references are included.
- Preview panel.

### Export Screen

Purpose:

- Generate and download files.

Important features:

- Export to PDF, DOCX, HTML, Markdown, and JSON.
- Store export history.
- Show template version and generation date.
- Option to regenerate previous exports.

## Export Strategy

### Markdown Export

Good first export target because it is easy to inspect and version.

### HTML Export

Good base for browser preview and PDF generation.

### PDF Export

Generate from HTML/CSS after the layout is stable.

### DOCX Export

Useful for recruiters and editable CV delivery. It can be generated from templates, but should come after the data model is stable.

### JSON Export

Useful for backups, integrations, and import/export between systems.

## Data Quality Suggestions

- Store dates as dates or year/month pairs, not only free text.
- Store current jobs with `is_current = true` instead of using only "Now".
- Normalize technologies so "Git" and "GIT" do not become separate skills.
- Keep sensitive personal information private by default.
- Store references but exclude them from default exports unless explicitly selected.
- Add language support from the beginning because the current CV is in Spanish and may need English variants.
- Add a `visibility` field to items that may be excluded from specific CVs.
- Add `sort_order` fields for sections where manual ordering matters.

## First Implementation Backlog

1. Create a structured CV data file from the current DOCX.
2. Define validation rules for required fields.
3. Build a Markdown exporter.
4. Add one HTML template.
5. Add SQLite or PostgreSQL persistence.
6. Build CRUD screens for profile, experience, education, skills, and CV variants.
7. Add PDF export.
8. Add DOCX export.

## Open Decisions

- Should the first version be a local desktop-style tool, a web application, or a command-line exporter?
- Should the main language of stored labels be English, Spanish, or multilingual from the start?
- Should PostgreSQL be used immediately, or should the first version start with SQLite or JSON/YAML?
- Should sensitive personal fields be included in normal exports?
- Should references be exported by default or only marked as available upon request?

## Proposed MVP Decisions

These decisions keep the first version small enough to build quickly while preserving a clean path toward a full web application later.

- First version type: command-line exporter plus structured data file.
- Primary source format: YAML, because it is easier to edit manually than JSON.
- First export formats: Markdown and HTML.
- Initial storage: file-based YAML only.
- Future storage: SQLite first, then PostgreSQL if the system becomes multi-user or cloud-hosted.
- Main stored data language: multilingual from the start for user-facing text, with English field names in the data model.
- Default export language: Spanish.
- Sensitive fields: stored, but hidden from public exports by default.
- References: stored, but excluded by default and replaced with "available upon request" when needed.
- CV variants: supported in the data file from the beginning, even if the first exporter only uses one default variant.

## MVP Scope

### Included

- A single structured `cv-data.yaml` file with all CV information.
- Validation rules that detect missing required fields and invalid references between records.
- Markdown exporter for readable, versionable CV output.
- HTML exporter for browser preview and later PDF generation.
- One default Spanish CV variant.
- Support for hiding sensitive personal data in exports.
- Support for selecting highlighted skills and ordered sections.

### Not Included Yet

- Database persistence.
- Web UI.
- Login or multi-user support.
- PDF generation.
- DOCX generation.
- Automatic import from the current DOCX.
- Template marketplace or advanced template editor.

## Functional Requirements

### Data Management

- The system must store the complete professional profile in one source file.
- The system must allow a person to have multiple contact methods.
- The system must support multiple professional summaries by language and target role.
- The system must support multiple CV variants from the same source data.
- The system must support experience, education, certifications, skills, languages, and references.
- The system must normalize technologies so work experience and skills can reuse the same technology records.
- The system must keep information that is not currently exported.

### Validation

- The system must validate the source file before exporting.
- Required person fields must be present: `id`, `full_name`, and at least one public contact method.
- Each work experience must have a company, start date, title, and summary.
- Current jobs must use `is_current: true` and should not require an end date.
- Non-current jobs should have an end date.
- Each skill must reference an existing technology.
- Each work experience technology must reference an existing technology.
- Each CV variant section must use a supported section type.
- Export should fail with a clear error message when required data is invalid.
- Validation warnings should not block export unless the data would make the generated CV incorrect.

### Exporting

- The exporter must accept a variant id.
- The exporter must apply section order from the selected variant.
- The exporter must include only visible sections.
- The exporter must include only data allowed by visibility rules.
- The exporter must support language-specific labels.
- The exporter must generate deterministic output so repeated exports with unchanged data produce the same content.
- The exporter must write generated files into an `exports/` directory.
- The exporter must not overwrite existing files unless explicitly requested or unless using a deterministic latest-output filename.

## Suggested Project Structure

```text
CurriculumVitae/
  cv-data.yaml
  CV_Information_System_Plan.md
  README.md
  src/
    cv_exporter/
      __init__.py
      cli.py
      load.py
      validate.py
      render_markdown.py
      render_html.py
      models.py
  templates/
    markdown/
      default_es.md.j2
    html/
      default_es.html.j2
      default.css
  exports/
    .gitkeep
  tests/
    test_validation.py
    test_markdown_export.py
```

The structure above assumes Python for the first command-line prototype. If the first implementation uses Java + Spring Boot instead, the same boundaries should remain: data loading, validation, rendering, templates, and tests.

## Initial YAML Data Shape

The first data file should be human-editable and stable. A simplified starting shape:

```yaml
person:
  id: simon-cardona
  full_name: "Simon Manuel Cardona Posso"
  preferred_name: "Simon Cardona"
  headline:
    es: "Ingeniero de Sistemas / Desarrollador Backend"
    en: "Systems Engineer / Backend Developer"
  date_of_birth: "1983-02-07"
  place_of_birth: "Colombia"
  nationalities:
    - "Colombian"
    - "Uruguayan legal nationality"
  civil_status: "Married"
  identity_document:
    value: "4.314.721-7"
    visibility: private

contacts:
  - id: email-main
    person_id: simon-cardona
    type: email
    value: "scardona9@gmail.com"
    is_primary: true
    visibility: public
  - id: linkedin-main
    person_id: simon-cardona
    type: linkedin
    value: "Simon Cardona"
    url: "https://uy.linkedin.com/in/simon-cardona-8545b212"
    visibility: public

summaries:
  - id: summary-es-backend
    person_id: simon-cardona
    language: es
    target_role: backend
    is_default: true
    content: "Resumen profesional pendiente de redaccion final."

technologies:
  - id: java
    name: Java
    category: language
  - id: spring-boot
    name: Spring Boot
    category: framework
  - id: aws
    name: AWS
    category: cloud

companies:
  - id: dualboot-partners
    name: Dualboot Partners
  - id: intraway
    name: Intraway

experience:
  - id: dualboot-2023-current
    person_id: simon-cardona
    company_id: dualboot-partners
    job_title: "Software Developer"
    start_date: "2023-01"
    end_date:
    is_current: true
    summary:
      es: "Desarrollo de soluciones utilizando Java AWS SDK."
      en: "Development of solutions using Java AWS SDK."
    methodology: Scrum
    technologies:
      - technology_id: java
      - technology_id: aws

skills:
  - id: skill-java
    person_id: simon-cardona
    technology_id: java
    level: expert
    years_experience: 14
    is_highlighted: true

cv_variants:
  - id: default-es
    person_id: simon-cardona
    name: "CV Espanol"
    language: es
    target_role: backend
    summary_id: summary-es-backend
    sections:
      - section_type: profile
        sort_order: 10
        is_visible: true
      - section_type: experience
        sort_order: 20
        is_visible: true
      - section_type: skills
        sort_order: 30
        is_visible: true
      - section_type: education
        sort_order: 40
        is_visible: true
```

## Visibility Rules

Visibility should be evaluated consistently across all exporters.

- `public`: can appear in normal exports.
- `private`: stored only; never exported unless explicitly forced.
- `export_only`: can appear in selected exports but should not appear in public previews.
- `hidden`: disabled data that remains in the source file for history.
- `available_on_request`: references or sensitive details can be represented by a generic sentence instead of full data.

Default behavior:

- Email, LinkedIn, and mobile can be public.
- Identity document, date of birth, civil status, full address, and references are private by default.
- References should not be printed with names and phone numbers unless the chosen CV variant explicitly enables them.

## Markdown Export Specification

The first Markdown exporter should produce a clean professional CV, not a dump of every stored field.

Recommended section order for `default-es`:

1. Header with name, headline, and public contact methods.
2. Professional profile.
3. Work experience.
4. Technical skills.
5. Education.
6. Certifications.
7. Languages.
8. References statement.

Formatting rules:

- Use one `#` heading for the person's name.
- Use `##` headings for major sections.
- Use reverse chronological order for work experience.
- Use concise bullet points for achievements and responsibilities.
- Show current jobs as `YYYY - Present` in English variants and `YYYY - Actualidad` in Spanish variants.
- Hide empty sections.
- Hide internal ids.

## HTML Export Specification

The first HTML exporter should use the same content decisions as Markdown.

Requirements:

- Generate one self-contained HTML file or one HTML file plus a CSS file.
- Use semantic HTML sections.
- Keep print styles in mind from the beginning.
- Use CSS variables for colors, spacing, and typography.
- Avoid hardcoding data in templates.
- Keep the layout readable in desktop browser preview and printable to PDF later.

## Command-Line Interface

Suggested commands:

```text
cv-export validate --data cv-data.yaml
cv-export markdown --data cv-data.yaml --variant default-es --output exports/cv-default-es.md
cv-export html --data cv-data.yaml --variant default-es --output exports/cv-default-es.html
```

Expected behavior:

- `validate` prints errors and warnings.
- Export commands run validation first.
- Export commands stop on validation errors.
- Export commands print the generated file path.
- A `--force` option can overwrite existing output files.

## Non-Functional Requirements

- The source data must remain readable in a normal text editor.
- The exporter must be deterministic.
- The code should separate data loading, validation, and rendering.
- Templates should not contain business rules beyond simple display logic.
- Tests should cover validation and at least one complete export.
- The first prototype should run locally without requiring a database server.

## Acceptance Criteria For Phase 1

- `cv-data.yaml` contains the current CV information in structured form.
- Running validation reports no blocking errors.
- Running the Markdown export creates a readable Spanish CV.
- Running the HTML export creates a browser-readable Spanish CV.
- Sensitive fields are not included in default exports.
- References are not exposed by default.
- Work experience appears in reverse chronological order.
- Skills can be filtered to highlighted skills for short CV variants.
- The generated output can be regenerated from source data without manual edits.

## Development Specification Documents

The project should keep detailed development specifications in separate Markdown files instead of growing a single large plan forever. The main plan remains the roadmap and index; each area owns its implementation definition documents.

Recommended folder organization:

```text
CurriculumVitae/
  backend/
    definition/
      domain-definition.md
      services-definition.md
      dao-definition.md
      controllers-definition.md
      security-definition.md
      patterns-definition.md
      logging-exceptions-definition.md
  frontend/
    definition/
      frontend-definition.md
      screens-definition.md
      components-definition.md
  database/
    definition/
      database-definition.md
      schema-definition.md
      migration-definition.md
```

### Backend Definition Documents

- `backend/definition/domain-definition.md`: domain entities, aggregate boundaries, value objects, validation invariants, and core business rules.
- `backend/definition/services-definition.md`: application services, use cases, service responsibilities, transaction boundaries, and orchestration rules.
- `backend/definition/dao-definition.md`: repository and DAO interfaces, persistence responsibilities, query expectations, and data access constraints.
- `backend/definition/controllers-definition.md`: REST controllers, endpoints, request/response DTOs, status codes, and API validation behavior.
- `backend/definition/security-definition.md`: authentication, authorization, sensitive data handling, API protection, audit behavior, and export privacy rules.
- `backend/definition/patterns-definition.md`: backend design patterns used by the project, their purpose, where they apply, and rules to avoid overengineering.
- `backend/definition/logging-exceptions-definition.md`: logging, redaction, traceability, custom exception hierarchy, and API error mapping.

### Frontend Definition Documents

- `frontend/definition/frontend-definition.md`: frontend architecture, routing approach, state management, form strategy, validation behavior, and integration boundaries.
- `frontend/definition/screens-definition.md`: screen-by-screen requirements for profile, experience, education, skills, variants, and exports.
- `frontend/definition/components-definition.md`: reusable component definitions for forms, tables, pickers, section ordering, preview panels, and export controls.

### Database Definition Documents

- `database/definition/database-definition.md`: database choice, environment assumptions, naming conventions, audit fields, and persistence principles.
- `database/definition/schema-definition.md`: tables, columns, keys, constraints, indexes, and relationships.
- `database/definition/migration-definition.md`: migration strategy, versioning rules, seed data, rollback policy, and local development setup.

Documentation rules:

- Each definition file should describe decisions, responsibilities, data contracts, and acceptance criteria for that area.
- Files should link back to the main plan when they depend on roadmap-level decisions.
- Implementation work should update the relevant definition file when behavior changes.
- Keep examples short and concrete; avoid duplicating the same entity or endpoint definition across multiple files.

## Next Backlog Breakdown

1. Create `cv-data.yaml` with the current CV content.
2. Create a minimal exporter package and CLI.
3. Implement YAML loading.
4. Implement validation.
5. Implement Markdown rendering.
6. Implement HTML rendering.
7. Add tests for invalid data and successful export.
8. Generate the first Spanish Markdown CV.
9. Generate the first Spanish HTML CV.
10. Review the output and refine profile summaries and work bullets.
