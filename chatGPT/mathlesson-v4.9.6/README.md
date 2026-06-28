# MathLesson v4.9.6

Current app file: `mathlesson.v4.9.6.html`.

This version is the current stable baseline after the inline exercise UI hotfix.

## Purpose

v4.9.6 fixes compact inline exercise widgets that rendered as non-editable red bars. Bare `<div class="exercise-widget" ...></div>` blocks now expand into visible prompt, input, Check button, feedback area, and hint area.

## Regex answer checking

Inline exercise widgets support `data-answer-regex` and `data-answer-regex-flags`.

Quiz free-response/math items support `answerRegex` and `answerRegexFlags`.

Use regex for capitalization, punctuation, spacing, and simple answer-format variants. Use symbolic checking for true mathematical equivalence.

## Diagnostic lesson

Use `reference-lessons/answer-checking-diagnostics-v1.1.json` to test exact text, alternate text, regex text, numeric, and symbolic answer modes.

## Manual validation

Open `mathlesson.v4.9.6.html`, import the diagnostics lesson, and verify that compact exercise blocks show usable input boxes and Check buttons. Confirm that `Boundary` and `boundary.` are accepted by the regex exercise, unrelated text is rejected, and MathLive text-format console errors do not appear during text/regex checks.

## GitHub upload note

The extracted text contents of the package are uploaded here. Binary MP4 files are omitted from this connector upload. See `OMITTED_BINARY_FILES.md`.
