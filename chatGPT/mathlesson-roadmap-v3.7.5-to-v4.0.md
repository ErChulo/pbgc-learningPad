# MathLesson Roadmap v3.7.5 → v4.0.0

**Roadmap version:** `roadmap-v3.7.5.1`  
**Application baseline:** `mathlesson.v3.7.5.html`  
**Status date:** 2026-06-18  
**Direction:** single-file mathematical lesson authoring workbench  
**Permanent decision:** TikZJax is removed and must not return as a runtime dependency.

---

## 0. Current State — `v3.7.5`

`v3.7.5` is the current tested baseline.

Implemented and tested:

- TikZJax removed.
- Clean Code Blocks editor.
- Supported editable blocks:
  - Mermaid
  - Plotly / Explore widgets
  - JSXGraph
  - Arquero
  - Manim video blocks
  - SVG blocks
- Collapsible / hidden sidebar.
- Wide-content mode.
- Persistent layout settings.
- Draggable editor modal.
- Resizable editor modal.
- Expanded/fullscreen editor mode.
- Larger editing areas.
- Mobile-safe editor behavior.
- User-created lessons can be saved, exported, imported, loaded, and deleted.

Known structural issue discovered in `v3.7.5`:

- Exported lessons may contain empty legacy properties:

```json
{
  "plots": {},
  "tables": {}
}
```

- These are legacy lesson-level registries.
- New editor-inserted Plotly, Arquero, Mermaid, JSXGraph, Manim, and SVG blocks live inside `sections[i].content`.
- This is not data loss, but it is confusing and should be corrected in the next roadmap step.

---

# 1. `v3.7.6` — Export Model Cleanup and Built-In Lesson Full Export

## Objective

Make exported JSON pedagogically clear and structurally complete.

## Changes

- Add export support for built-in lessons.
- Built-in lessons must export the complete lesson bundle:
  - title
  - audio metadata
  - sections
  - quiz
  - section HTML
  - inline code blocks
  - Plotly widgets
  - Arquero blocks
  - Mermaid blocks
  - JSXGraph blocks
  - SVG blocks
  - Manim video blocks
  - legacy `plots` registry if used
  - legacy `tables` registry if used
  - helper functions/config where applicable
- Add a clear export schema version:

```json
{
  "schemaVersion": "mathlesson.lesson.v1",
  "exportedFrom": "mathlesson.v3.7.6"
}
```

- Add a `blockInventory` section to exported JSON.
- The inventory should list discovered blocks even if they are stored inside `sections[i].content`.

Example:

```json
{
  "blockInventory": [
    {
      "id": "plot-sine-family",
      "type": "plotly",
      "sectionIndex": 1,
      "sectionLabel": "§2 Sine Curves",
      "storage": "sections[1].content"
    }
  ]
}
```

- Keep legacy `plots` and `tables` only when actually populated.
- Empty `plots: {}` and `tables: {}` should be omitted from user-facing exports, or moved into a clearly named legacy section.

## Tests

- Export a built-in lesson and re-import it.
- Export a user lesson with inline Plotly blocks.
- Export a user lesson with inline Arquero blocks.
- Confirm no apparent data loss.
- Confirm block inventory correctly detects all blocks.
- Confirm old legacy `plots` / `tables` lessons still work.

---

# 2. `v3.7.7` — Rich Quiz Stem Rendering

## Objective

Quiz questions should support the same mathematical and graphical tooling as lesson sections.

## Changes

Allow quiz stems / preambles to render:

- LaTeX / KaTeX
- Mermaid
- Plotly / Explore widgets
- JSXGraph
- Arquero
- Manim video blocks
- SVG
- tables
- callouts
- code blocks

## Required behavior

Each quiz question should become a small renderable panel.

After a quiz card is rendered, the app should run the same renderer pipeline used for lesson sections:

```text
renderKatex
initExploreWidgets
initMermaid
initArqueroBlocks
initJSXGraphs
Manim/video initialization
SVG sizing
```

## Tests

- Multiple-choice question with LaTeX only.
- Multiple-choice question with Mermaid preamble.
- True/false question with Plotly preamble.
- Math-answer question with JSXGraph preamble.
- Free-response question with Arquero table preamble.
- Quiz reset and submit still work.
- Quiz state persistence still works.

---

# 3. `v3.7.8` — Authoring Guide Rewrite

## Objective

Turn the Authoring Guide into a real didactic manual for new users.

## Changes

Revise the guide so it teaches:

- what MathLesson is
- expected prerequisites
- lesson JSON structure
- section structure
- quiz structure
- inline block structure
- Code Blocks editor usage
- how Plotly blocks work
- how Arquero blocks work
- how JSXGraph blocks work
- how Mermaid blocks work
- how Manim video blocks work
- how SVG blocks work
- how export/import works
- difference between legacy registries and inline section blocks
- recommended authoring workflow

## Tests

- New user can inspect built-in lesson JSON and understand structure.
- Guide explains why exported `blockInventory` exists.
- Guide explains where code actually lives.
- Guide contains working minimal examples for every block type.

---

# 4. `v3.7.9` — Theme System Feasibility and Implementation

## Objective

Add dark/light mode only if it can be applied coherently to all major renderers.

## Decision rule

Implement dark/light toggle only if compatible theming can be forced into:

- app chrome
- content panels
- KaTeX surfaces
- Plotly layouts
- Mermaid diagrams
- JSXGraph boards
- Arquero tables
- SVG wrappers where possible
- code blocks
- editor modal
- quiz cards

If this cannot be made coherent, discard the feature rather than ship a partial theme toggle.

## Changes if feasible

- Add global theme toggle.
- Store theme preference in localStorage.
- Re-render or restyle affected widgets after theme changes.
- Add theme-aware Plotly layout defaults.
- Add theme-aware JSXGraph board defaults.
- Add theme-aware Mermaid initialization.

## Tests

- Toggle theme in lesson mode.
- Toggle theme in editor mode.
- Toggle theme with Plotly visible.
- Toggle theme with JSXGraph visible.
- Toggle theme with Mermaid visible.
- Reload page and confirm persistence.
- Confirm print/export styling is not damaged.

---

# 5. `v3.8.0` — Calculator Foundation

## Objective

Add mathematical calculation as a first-class authoring tool.

## Candidate libraries

- Nerdamer: symbolic algebra, differentiation, solving, LaTeX output.
- math.js: numeric computation, units, matrices, complex numbers, expression parsing.

## Changes

Add a Calculator panel or editor tab supporting:

- evaluate
- simplify
- expand
- factor
- differentiate
- integrate where supported
- solve where supported
- substitute values
- convert output to LaTeX
- insert result into lesson as:
  - inline math
  - display math
  - worked step
  - callout

## Tests

- Numeric expression.
- Symbolic simplification.
- Derivative.
- Basic equation solving.
- LaTeX output insertion.
- Calculator does not break lessons without calculator usage.

---

# 6. `v3.8.5` — Statistics and Probability Foundation

## Objective

Support applied-math lessons, probability lessons, and actuarial examples.

## Candidate functionality

- descriptive statistics
- distributions
- PDF/CDF/quantile calculators
- random simulation
- regression demonstrations
- Monte Carlo blocks
- survival curves
- actuarial life-table blocks

## Candidate libraries

- simple-statistics
- jStat
- math.js
- custom actuarial helpers

## Block candidates

```text
probability-distribution
simulation
regression
life-table
survival-curve
annuity-factor
present-value-cashflow
multiple-decrement-table
```

## Tests

- Distribution calculator.
- Monte Carlo simulation block.
- Survival curve plotted with Plotly.
- Arquero table feeding a statistic.
- Export/re-import with statistical blocks.

---

# 7. `v3.9.0` — Reveal.js Presentation Export

## Objective

Allow lessons to be presented as slide decks.

## Changes

- Add `Export → Reveal.js Deck`.
- Convert sections to slides.
- Support explicit slide breaks:

```html
<hr class="slide-break">
```

or:

```html
<!-- slide -->
```

- Preserve:
  - KaTeX
  - Mermaid
  - Plotly
  - JSXGraph where feasible
  - Arquero tables
  - Manim videos
  - SVG
  - callouts

## Tests

- Export simple lesson.
- Export lesson with Plotly.
- Export lesson with JSXGraph.
- Export lesson with Mermaid.
- Export lesson with Manim video.
- Confirm slides are not visually overloaded.

---

# 8. `v3.9.2` — Print/PDF Pagination Quality

## Objective

Prevent ugly splits in printed/PDF output.

## Changes

Add print/export layout protection for:

- tables
- paragraphs
- callouts
- equations
- Plotly figures
- JSXGraph boards
- Mermaid diagrams
- Manim video placeholders
- quiz cards
- whole lesson sections where feasible

Add author controls:

```html
<hr class="page-break">
<div class="avoid-break">...</div>
<div class="allow-break">...</div>
```

Add export diagnostics:

```text
Table in §3 may exceed one page.
Plot in §5 may be clipped.
Section §7 has no safe page break.
```

## Tests

- Print preview with long table.
- Print preview with large plot.
- Print preview with long paragraph.
- Print preview with mixed blocks.
- Confirm no severe clipping.

---

# 9. `v3.9.5` — PowerPoint Export

## Objective

Generate `.pptx` decks for users who need PowerPoint.

## Candidate library

- PptxGenJS

## Expected limitations

Interactive elements will usually need to be flattened.

| Element | PowerPoint strategy |
|---|---|
| Text | native text boxes |
| LaTeX | image or approximate Office equation |
| Plotly | snapshot image |
| JSXGraph | snapshot image |
| Mermaid | SVG/image |
| Arquero table | native table or image |
| Manim | linked/embedded video or poster frame |

## Tests

- Export text-only deck.
- Export deck with equations.
- Export deck with Plotly snapshot.
- Export deck with Mermaid snapshot.
- Export deck with Manim video/poster.
- Confirm slide overflow warnings.

---

# 10. `v3.9.8` — Tool Adapter Registry

## Objective

Stop hardcoding every tool into scattered renderer functions.

## Changes

Introduce a registry pattern:

```json
{
  "id": "plotly",
  "name": "Plotly",
  "category": "visualization",
  "loadMode": "core",
  "status": "available",
  "blockSelector": ".explore-widget[data-explore]",
  "renderFunction": "initExploreWidgets",
  "exportBehavior": "interactive-or-snapshot"
}
```

The registry should govern:

- diagnostics
- prerequisites
- rendering
- export behavior
- block inventory
- editor support
- quiz support

## Tests

- Diagnostics generated from registry.
- Block inventory generated from registry.
- Export behavior references registry.
- Adding a new tool requires minimal code changes.

---

# 11. `v4.0.0` — Structured Mathematical Workbench

## Objective

Move from raw HTML-first lessons to a structured block-based lesson model.

## Canonical block types

```text
text
math
callout
definition
theorem
proof
example
exercise
quiz
table
dataset
plotly
jsxgraph
mermaid
arquero
calculator
statistics
simulation
manim-video
svg
code
slide-break
page-break
```

## Target structure

```json
{
  "schemaVersion": "mathlesson.lesson.v2",
  "title": "Example Lesson",
  "blocks": [
    {
      "id": "plot-survival-curve",
      "type": "plotly",
      "title": "Survival Curve",
      "source": "...",
      "renderStatus": "ok",
      "exportPolicy": "fit-to-page",
      "breakPolicy": "avoid",
      "dependencies": ["plotly"]
    }
  ]
}
```

## Requirements

- Raw HTML remains available as advanced mode.
- Structured blocks become the canonical model.
- Export logic uses block metadata.
- Quiz stems use the same block model as lesson sections.
- Presentation and PDF exports use layout metadata.
- Tool adapters define render/export behavior.

## Tests

- Convert old `sections[i].content` lesson to structured blocks.
- Load old lessons without breaking.
- Export structured lesson to JSON.
- Export structured lesson to Reveal.js.
- Export structured lesson to PDF.
- Export structured lesson to PowerPoint.

---

# Version-Control Policy for Roadmap Files

From this point forward, roadmap files should be versioned explicitly.

Filename pattern:

```text
mathlesson-roadmap-v<baseline>-to-v4.0.md
```

Examples:

```text
mathlesson-roadmap-v3.7.5-to-v4.0.md
mathlesson-roadmap-v3.7.6-to-v4.0.md
```

Each roadmap should include:

- roadmap version
- application baseline
- status date
- completed versions
- next target version
- changed assumptions
- permanent decisions

---

# Immediate Next Step

The next implementation step is:

```text
v3.7.6 — Export Model Cleanup and Built-In Lesson Full Export
```

Primary test focus:

- built-in lesson export
- full-bundle JSON export
- block inventory
- removal or clarification of empty legacy `plots` / `tables`
- import/export round trip
