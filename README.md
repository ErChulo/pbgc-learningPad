# pbgc-learningPad
### Notes tool to enhance the actuarial-mathematical learning.

## v3.8

Continuing — sidebar collapse, maximize, example lessons, exampleBoard, downloadLessonJSON, then authoring guide rewrite.

TikZJax fully purged. Now Patch 8: Sidebar collapse button + CSS:

Now add the expand button to the DOM (next to hamburger in topbar):

Patch 9: Editor maximize button + CSS

Now add CSS for the maximize button and the maximized state:

Patch 10: exampleBoard in JSXGRAPH_BOARDS

Patch 11: downloadLessonJSON function + example lessons list in sidebar

First find where user lessons / lesson select lives:

Add CSS for the example lesson items:

Patch 12: downloadLessonJSON function — find where LESSONS are defined to understand the data structure:

The LESSONS object has JS functions (plots, tables) — it can’t be trivially serialized to JSON. The download should serialize only the serializable parts (title, sections labels, quiz). Let me find the boot() function and the editor’s export approach to match the expected JSON format:

Good — the JSON format needs {title, sections:[{id,label,content}], quiz:[...], audio:{...}}. For built-in lessons the sections have contentId (template refs) not inline content. I’ll serialize the template content into the JSON for download:

Patch 13: Wire all DOMContentLoaded listeners (collapse, expand, maximize, download buttons):

Patch 14: Rewrite the Authoring Guide for beginners

Now I have the full guide panel. Replace it wholesale with the beginner-friendly rewrite:

Now verify everything looks clean and write the CHANGELOG:

Zero tikz references. Now sanity-check the other patches:

All patches confirmed. Write the CHANGELOG and deploy:

v3.8 deployed. Here’s what was applied:

TikZJax — fully gone: CDN links, CSS, initTikZ()/watchForSVG() engine, insert button, skeleton, guide callout, demo section, KaTeX ignore list entries. Zero occurrences remain.

New features:

Sidebar collapse (◀ button in logo bar → hidden sidebar; ▶ fixed button to restore)

Editor maximize (⤢/⤡ toggle → full-screen modal)

Examples section in sidebar with ⬇ per-lesson download buttons wired to downloadLessonJSON()

exampleBoard JSXGraph board (draggable A–B segment + live midpoint M)

Demo diagrams section now shows two interactive explore widgets (sine sweep, Normal PDF with μ/σ) replacing the TikZ blocks

Authoring Guide — rewritten from scratch, numbered 1–13, beginner-first: quick start walkthrough, copy-paste code blocks for every widget type, tables for callouts/quiz types/localStorage keys.

## 3.7.1

Two root causes before touching anything:

TikZJax: Our .tikz-source div approach is wrong. TikZJax’s MutationObserver watches for <script type="text/tikz"> appearing in the live DOM. We should put the script tags directly in the templates — they’re inert inside <template> blocks, then become live when cloned. Our engine then just needs to add a loading indicator and watch for SVG output.

KaTeX: Two issues. First, renderMathInElement is running before Mermaid finishes (Mermaid is async) — if Mermaid node labels contain math they won’t render. Second, Mermaid/JSXGraph SVG output contains text elements that KaTeX tries to parse as math when called again, corrupting them. Fix: add ignoredTags including svg, and call KaTeX a second time after a short delay to catch any async content.

(second update)

TikZJax: The engine was creating <script> elements dynamically and hoping TikZJax’s MutationObserver would catch them mid-WASM-load — unreliable. Now TikZ code lives directly in <script type="text/tikz"> inside <template> tags. They’re inert there, become live when the template is cloned into the DOM, and TikZJax catches them reliably. initTikZ() now only manages the loading spinner and a watchForSVG() helper with a 30-second timeout.

KaTeX: Two fixes — added ignoredTags: ['svg', 'script', 'pre', 'code', ...] and ignoredClasses: ['mermaid', 'jxgbox', ...] so KaTeX never touches Mermaid SVG or JSXGraph boards. Added a second KaTeX pass 600ms after the first to catch content added by async engines like Mermaid. Both passes wrapped in try/catch so one bad element never kills the rest.

## 3.7

v3.7 — three new libraries, fully integrated with edge cases handled.

Mermaid.js — write diagrams as text in <pre class="mermaid"> blocks. Dark-themed. Error messages appear inline if syntax is wrong. Demo §5 has a Bayesian flowchart and a PBGC case lifecycle sequence diagram.

TikZJax — standard LaTeX TikZ syntax, rendered via WebAssembly. Content goes in .tikz-source divs (not <script> tags — those break inside <template> elements). Loading spinner shows while WASM runs, 30-second timeout with a clear error if it stalls. First load fetches ~6MB binary, cached by browser after that. Demo §5 has the unit circle and a normal PDF.

JSXGraph — interactive geometry with draggable objects. Board functions are registered by name in JSXGRAPH_BOARDS and referenced via data-init attribute. All boards are properly destroyed on lesson switch to prevent memory leaks. Two built-in boards: circle theorem (inscribed angle stays 90° over a diameter) and derivative explorer (drag a point, watch the tangent line and live f’(x) value).

Insert toolbar has three new buttons: ⬡ Mermaid, ∇ TikZ, ⊹ JSXGraph — each inserts a working skeleton.

## 3.6

section completion marking and lesson table of contents

## 3.5

full mobile layout.
On phone/tablet (≤768px):

☰ button in the top bar opens the sidebar as a slide-in panel
Tap the dark backdrop or any nav item to close it
Editor modal slides up from the bottom (sheet style)
Everything reflows to single column — no horizontal overflow
Tables scroll horizontally if they're wide

On desktop — nothing changed, hamburger is hidden, layout is identical to v3.4.

## v3.4

Every lesson section now has a ▶ My Notes panel at the bottom:

Click to expand — textarea autofocuses
Types automatically save 400ms after you stop — no save button needed
Gold dot on the toggle tells you at a glance which sections have notes
Auto-opens when you return to a section that has saved notes
Clear button wipes the note with a confirmation
Prints with your lesson — notes appear inline below each section with a yellow background

Notes are keyed per lesson + per section, so switching lessons never mixes them up.

## v3.3
Three patches:

    - Preview: call initExploreWidgets after renderKatex in editorUpdatePreview
    - 3D performance: throttle draw3d with requestAnimationFrame and a pending flag
    - MathLive UX: better hint + keyboard shortcut label + auto-focus on field click

Three fixes shipped:

#1 — Explore widgets in preview: Insert a 2D plot or exercise widget in the editor, and the preview pane renders it live — sliders work, Check buttons work, everything. No more seeing raw HTML.

#2 — 3D/2D performance: Both widgets now use a requestAnimationFrame throttle flag. No matter how fast you drag a slider, only one Plotly redraw fires per frame. The 3D surface at high frequencies is now smooth.

#3 — MathLive discoverability: The placeholder now says Type LaTeX e.g. \frac{1}{2} instead of a vague message. The hint bar shows four concrete examples (\frac{a}{b}, \sqrt{x}, x^2, \cos(x)) as styled code chips so users immediately know the syntax.

## v3.2 — Insert toolbar in the editor's Sections tab.

How to use it:

> Open ✏️ Lesson Editor → select or create a section

- The Insert: toolbar appears above the textarea
- Click any callout type or ∫ Display Math → snippet inserted at cursor immediately
- Click ⊕ 2D Plot, ⊕ 3D Surface, or ✎ Exercise Widget → config panel opens:

- Fill in title, function, ranges
- Add/remove/tweak parameters with the parameter builder
- Click Insert ↓ → HTML injected, panel closes, preview updates

The textarea remains editable for fine-tuning after insertion. Every insert goes at the current cursor position so you can place components anywhere in the section.
