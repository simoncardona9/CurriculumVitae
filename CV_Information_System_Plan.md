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

