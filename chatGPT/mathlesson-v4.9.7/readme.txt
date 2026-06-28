## Download link

[Download `mathlesson-v4.9.7-current-package.zip`](sandbox:/mnt/data/mathlesson-v4.9.7-current-package.zip)

## Changes made

* Built `mathlesson.v4.9.7.html` from `v4.9.6`.
* Added iPhone / small-screen stabilization.
* Added native mobile text/LaTeX inputs for math assessment fields on small screens.
* Preserved MathLive behavior on larger screens.
* Added mobile keyboard dismissal behavior.
* Added iOS viewport and safe-area CSS.
* Improved quiz button reachability on phones.
* Improved UX Stats panel scrolling/closing on phones.
* Improved Lesson Editor scrolling and action-button reachability on phones.
* Propagated mobile answer-input behavior into student exports.
* Updated `README.md`, `START_HERE.md`, checklist, and roadmap.
* JavaScript syntax check passed.
* ZIP integrity check passed.

## Manual testing checklist

* Open `mathlesson.v4.9.7.html` on desktop.
* Import `reference-lessons/answer-checking-diagnostics-v1.1.json`.
* Confirm exact, alternate, regex, numeric, and symbolic answer checks still work.
* Deploy `mathlesson.v4.9.7.html` to Netlify.
* Open the Netlify URL on iPhone in portrait mode.
* Confirm inline exercises can be completed without rotating to landscape.
* Confirm quiz math questions can be completed without rotating to landscape.
* Confirm keyboard/input panel can be dismissed after answering.
* Confirm UX Stats opens, scrolls, exports/copies, and closes.
* Confirm Lesson Editor opens, scrolls, saves, previews, and closes.
* Export a student lesson and test the exported lesson on iPhone.

