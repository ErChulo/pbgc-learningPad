# MathLesson Roadmap v4.9.6 — Inline Exercise UI Hotfix

## Purpose

Fix the regression where compact inline exercise widgets rendered as empty red bars instead of usable answer controls.

## Completed in v4.9.6

- Bare `<div class="exercise-widget" ...></div>` blocks now auto-expand into visible controls.
- `data-type="text"` widgets receive standard text inputs.
- math/default widgets receive MathLive math fields.
- Missing prompt, input row, Check button, result area, and hint area are created at render time.
- Existing full exercise-widget markup remains supported.
- The same auto-expansion logic is included in student exports.
- Diagnostic reference lesson updated to `answer-checking-diagnostics-v1.1.json`.

## Next candidates

- Add an author-side linter warning for malformed exercise widgets.
- Add visible diagnostic badges for answer-checking mode: exact, alt, regex, numeric, symbolic.
- Improve lesson QA tools before the Vite + React migration.
