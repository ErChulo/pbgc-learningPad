# MathLesson Training Pack — v1.1 Batch 1 Manifest

## Required app baseline

- `mathlesson.v4.7.2.html`

## Why v1.1 exists

This revision removes confusing user-facing schema/version noise from the import workflow by aligning the training files with the corrected app build metadata.

The internal block schema may still be `mathlesson.block.v4.4.0`; that is not a user action item. It is the current internal structured block registry version used by the app.

## Files

- `feature-tour-basic-v1.1.json` — basic feature tour and common authoring blocks.
- `mermaid-lab-v1.1.json` — Mermaid syntax safety and corrected-import checks.
- `export-layout-lab-v1.1.json` — Print/PDF and Reveal layout controls.

## Acceptance checks

For each lesson:

1. Import JSON successfully.
2. Confirm lesson title includes `v1.1`.
3. Confirm no confusing block-schema migration notification appears.
4. Render all sections.
5. Run Prototype Audit.
6. Export Active Lesson JSON and re-import it.
7. Export Student Lesson + Quiz where applicable.
8. Record any action that was hard to find in the UI.
