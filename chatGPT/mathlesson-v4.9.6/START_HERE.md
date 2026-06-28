# MathLesson v4.9.6 — Start Here

Use `mathlesson.v4.9.6.html` as the current stable single-file HTML build.

This version is a narrow hotfix over v4.9.5. It fixes compact inline exercise widgets that rendered as non-editable red bars instead of usable answer controls.

## What changed in v4.9.6

- Bare inline exercise widgets now auto-expand into visible controls.
- A compact block such as:

```html
<div class="exercise-widget"
     data-type="text"
     data-prompt="Enter boundary."
     data-answer="boundary"
     data-answer-regex="^boundary\\.?$"
     data-answer-regex-flags="i"></div>
```

now renders as a prompt, answer input, Check button, result area, and optional hint area.

- `data-type="text"` creates a normal text input.
- Math/default widgets create a MathLive math field.
- Student exports include the same auto-expansion behavior.

## Recommended validation

1. Open `mathlesson.v4.9.6.html`.
2. Import `reference-lessons/answer-checking-diagnostics-v1.1.json`.
3. Confirm compact exercise blocks render as actual input boxes and Check buttons.
4. Confirm `Boundary` and `boundary.` are accepted by regex.
5. Confirm unrelated text is rejected.
6. Confirm the console does not show `MathLive 0.110.0: Unexpected format "text"` during text/regex checks.

## Current baseline

Treat v4.9.6 as the stable authoring baseline unless a new blocker appears.

Recommended next work is content-level authoring, especially patching the Differential Forms lesson JSON and exporting/testing a student lesson.
