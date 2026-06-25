# MathLesson Roadmap — v4.7.5 to Guided Onboarding and UX Redesign

## Current confirmed baseline

- Current application baseline: `mathlesson.v4.7.5.html`
- Prior usability baseline: `mathlesson.v4.7.4.html`
- Current stress-test pack: `mathlesson-stress-test-pack-v1.0.zip`
- Current stress-test files:
  - `stress-long-formulas-matrices.json`
  - `stress-big-mermaid-diagrams.json`
  - `stress-wide-arquero-tables.json`
  - `stress-layout-overflow-print-reveal.json`
  - `stress-integrated-heavy-lesson.json`

## v4.7.5 update: Prompt Library integration

`v4.7.5` promotes the external-material lesson-generation prompt to a first-class authoring asset inside the application.

### Added artifact

- Prompt artifact: `prompt-for-JSON-lesson-v1.0.txt`
- Prompt version: `v1.0`
- Status: initial app-integrated prompt library version
- Purpose: generate MathLesson-compatible JSON lessons from externally supplied source material such as PDFs, OCR text, notes, articles, booklets, and pasted source text.

### App integration

The Authoring Guide now includes:

- a **Prompt Library: JSON Lesson Generator Prompt v1.0** section;
- a prompt-version registry table;
- usage instructions for pairing the prompt with external material;
- the full prompt text embedded in the guide;
- a **Copy Prompt v1.0** control;
- an explicit rule that prompt revisions require version increments.

### Version-control rule for prompts

Do not silently replace `v1.0`. Any substantive change to the prompt must create a new version.

Examples:

- `v1.1`: small changes to wording, baseline version, or quality checks;
- `v1.2`: improved renderer policies or quiz rules;
- `v2.0`: major schema change, major app architecture change, or major lesson-generation policy change.

## Why this matters

The prompt is now part of the lesson-production workflow, not just an external note. It should be maintained as carefully as the app roadmap because it controls how future lessons are generated from source material.

## Reclassification of earlier lab files

The earlier Batch 1 lab files remain useful, but they should be treated as stress-test / diagnostic material, not beginner onboarding lessons.

- `feature-tour-basic-v1.1.json`: stress and feature coverage
- `mermaid-lab-v1.1.json`: Mermaid safety and import behavior
- `export-layout-lab-v1.1.json`: export layout and pagination controls

They are not final training lessons because they are still too diagnostic in tone.

## Next workstream 1: Stress testing

Use the Stress Test Pack to identify bugs, layout failures, confusing workflows, and renderer limitations.

Recommended sequence:

1. `stress-long-formulas-matrices.json`
2. `stress-big-mermaid-diagrams.json`
3. `stress-wide-arquero-tables.json`
4. `stress-layout-overflow-print-reveal.json`
5. `stress-integrated-heavy-lesson.json`

For each file:

- import through the top-bar **Import JSON** button;
- verify immediate load;
- run Prototype Audit;
- check the browser console;
- test JSON export and re-import;
- test Student Lesson export;
- test Reveal export when relevant;
- test Print/PDF preview when relevant;
- record issues in the Stress Test Runner log.

## Next workstream 2: Guided Onboarding Pack

After stress testing, create a new guided onboarding pack. These files should not be technical labs. They should be beginner walkthroughs that tell the author exactly what to do.

Required style:

- step-by-step language;
- explicit button names;
- explicit navigation paths;
- short tasks;
- low cognitive load;
- clear “what you should see” checkpoints;
- explicit “write down what was hard to find” friction prompts.

Planned guided onboarding files:

1. `onboarding-01-import-and-navigate.json`
2. `onboarding-02-edit-sections-and-preview.json`
3. `onboarding-03-add-math-and-callouts.json`
4. `onboarding-04-add-diagrams-and-plots.json`
5. `onboarding-05-add-quiz-and-exercises.json`
6. `onboarding-06-export-json-and-student-lesson.json`
7. `onboarding-07-run-audit-and-fix-layout.json`
8. `onboarding-08-generate-lesson-from-external-material.json`

The eighth onboarding file should explicitly introduce `prompt-for-JSON-lesson-v1.0.txt` and show how it is used with an external source.

## Next workstream 3: UX/UI redesign specification

The UX/UI redesign should still come after stress testing and guided onboarding. The redesign must be based on observed workflow friction, not abstract preferences.

Expected redesign targets:

- task-first home/dashboard;
- global Lesson menu;
- always-visible import/export affordances;
- command palette;
- unified authoring workspace;
- clearer separation of author tasks versus advanced diagnostics;
- better placement of Prompt Library and generated-lesson workflow;
- reduced panel hunting.

## Next workstream 4: Vite migration

Vite migration remains deferred until after:

1. stress testing;
2. guided onboarding lessons;
3. UX/UI redesign specification.

The current single-file prototype remains the behavioral specification.

The future Vite build should preserve a single-file HTML output option.

## Exclusions

- Do not reintroduce TikZJax.
- Do not add PowerPoint export in this phase.
- Do not start Vite migration before the onboarding and UX/UI workstreams are complete.

## Acceptance criteria for the next phase

The next phase is complete when:

- `mathlesson.v4.7.5.html` imports stress-test files without confusing import notices;
- the Prompt Library is visible under Authoring Guide;
- `prompt-for-JSON-lesson-v1.0.txt` is preserved as a versioned artifact;
- the Stress Test Runner log captures any observed bugs/friction;
- the Guided Onboarding Pack is defined and ready for production;
- UX/UI redesign requirements are grounded in real user workflow observations.
