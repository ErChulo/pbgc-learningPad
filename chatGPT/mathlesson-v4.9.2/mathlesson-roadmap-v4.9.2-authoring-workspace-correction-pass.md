# MathLesson Roadmap — v4.9.2 Authoring Workspace Correction Pass

## Current build

`mathlesson.v4.9.2.html`

## Status

`v4.9.2` is a focused correction pass after the v4.9.1 authoring-workspace redesign. It is not a feature expansion. It corrects authoring-workspace usability defects discovered during laptop-layout testing and makes built-in lessons part of the editable lesson model.

## Scope completed in v4.9.2

- Preserve CodeMirror syntax highlighting from v4.9.x.
- Add horizontal scrolling to the author preview pane when previewed content exceeds panel width.
- Treat phone-sized authoring as secondary for now; do not over-optimize the full author workspace for iPhone dimensions.
- Make the Lesson Editor a full-page authoring workspace.
- Remove obsolete drag/resize language and hide obsolete modal window controls.
- Make built-in lessons appear in the Lesson Editor selector.
- Allow built-in lessons to be opened as editable copies.
- Allow edited built-in lessons to be saved, downloaded as JSON, and exported as student lessons.
- Update package-level `README.md`, `START_HERE.md`, checklist, and roadmap artifacts.

## Current testing strategy

Testing should be gate-based, not repetitive full regression. The v4.9.2 gate should focus on:

1. Startup stability.
2. Full-page authoring workspace behavior.
3. CodeMirror source editing.
4. Preview horizontal overflow.
5. Built-in lesson editability.
6. Duplicate import notices.
7. Targeted regression of quiz and student-export fixes from v4.8.2/v4.8.3.

## Known non-goals for v4.9.2

- No Vite migration.
- No server-side architecture.
- No PowerPoint export.
- No Manim rendering inside the browser.
- No complete mobile authoring redesign.
- No broad content expansion.

## Next roadmap candidate

If v4.9.2 passes, the next meaningful version should be either:

- `v4.9.3`: final author-workspace polish and bug hardening, or
- `v5.0.0`: project restructuring / repository-grade modularization, potentially including Vite or another build pipeline.

A Vite migration should not start until the single-file authoring workflow is stable enough to avoid migrating known UX defects into the new architecture.
