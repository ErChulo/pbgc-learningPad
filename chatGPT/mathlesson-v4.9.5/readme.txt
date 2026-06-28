## Download link

[Download `mathlesson-v4.9.5-current-package.zip`](sandbox:/mnt/data/mathlesson-v4.9.5-current-package.zip)

## Changes made

* Built `mathlesson.v4.9.5.html` from `v4.9.4`.
* Hardened answer-checking order:

  * regex,
  * alternate answers,
  * exact / normalized text,
  * numeric,
  * symbolic.
* Removed unsupported MathLive `getValue("text")` calls.
* Prevented likely plain-text answers from being routed into MathLive / ComputeEngine symbolic parsing.
* Preserved inline exercise regex checking.
* Preserved quiz `answerRegex` / `answerRegexFlags` checking.
* Added `reference-lessons/answer-checking-diagnostics-v1.0.json`.
* Updated `README.md`, `START_HERE.md`, checklist, roadmap, and prompt guidance.
* JavaScript syntax check passed.
* ZIP integrity check passed.

## Manual testing checklist

* Open `mathlesson.v4.9.5.html`.
* Import `reference-lessons/answer-checking-diagnostics-v1.0.json`.
* Test exact text, alternate text, regex text, numeric, and symbolic answers.
* Confirm `Boundary` and `boundary.` are accepted by regex.
* Confirm unrelated text is rejected.
* Confirm the console no longer shows `MathLive 0.110.0: Unexpected format "text"` during text/regex checks.
* Ignore local `file:` URL warnings.

