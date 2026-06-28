# WORK IN PROGRESS — MathLesson

MathLesson is an experimental, local-first HTML application for creating, editing, testing, and exporting interactive mathematics lessons. It is currently under active development and should be treated as a work in progress rather than a production-ready learning management system.

Current packaged build: `mathlesson.v4.9.5.html`.

## Why this project exists

Most mathematical lesson tools split the authoring workflow across several incompatible systems: one place for prose, another for equations, another for diagrams, another for interactive graphs, another for quizzes, and another for export. MathLesson is an attempt to put those pieces into one portable authoring environment that can run locally from a single HTML file.

The project is designed for technical mathematical instruction where a lesson may need explanatory prose, LaTeX, diagrams, computational widgets, data tables, quizzes, and exportable student-facing versions. The immediate development goal is not institutional deployment. The goal is to refine the authoring model and determine what the correct architecture should become.

## What MathLesson does

MathLesson provides a browser-based workspace for:

- writing lesson sections with HTML and LaTeX;
- rendering math with KaTeX;
- rendering diagrams with Mermaid;
- embedding JSXGraph boards;
- embedding Plotly visualizations;
- using Arquero-backed table/data blocks;
- embedding externally rendered Manim videos;
- building quizzes and inline exercises;
- importing and exporting lesson JSON;
- exporting a student-facing HTML lesson with quiz and progress state;
- running diagnostic and print/export checks;
- collecting local UX telemetry during development.

The app is intentionally local-first. It can be opened directly as an HTML file and does not require a server for normal use.

## Where things are in this package

```text
mathlesson-v4.9.5-current-package/
  README.md
  START_HERE.md
  mathlesson.v4.9.5.html
  OpeningManim-1.mp4
  SineCurveUnitCircle-1.mp4
  MovingAngle-1.mp4
  checklists/
    mathlesson-v4.9.5-master-test-checklist.md
    earlier versioned checklists...
  lesson-packs/
    author-onboarding-v1.0/
    student-learning-v1.0/
    stress-test-v2.0/
  prompts/
    prompt-for-JSON-lesson-v1.5.txt
    earlier prompt versions...
  reference-lessons/
  roadmap/
    mathlesson-roadmap-v4.9.5-regex-answer-checking.md
    earlier roadmap versions...
```


## v4.9.5 answer-checking contract

MathLesson supports several answer-checking modes for authored assessment items:

- exact answer matching through `data-answer` on inline exercises or `answer` on quiz items;
- alternate answers through `data-alt-answers` on inline exercises or `altAnswers` on quiz items;
- regular-expression matching through `data-answer-regex` on inline exercises or `answerRegex` on quiz math/free-response items;
- symbolic/numeric checking for math quiz items when the Compute Engine can compare expressions.

Regex checking is intended for capitalization, punctuation, spacing, and numeric-format variants. It is not a substitute for full symbolic equivalence when algebraic expressions may be genuinely rearranged.

## Purpose

The purpose of MathLesson is to explore a high-control authoring environment for mathematical pedagogy. It is intended for lessons where ordinary markdown is not expressive enough and where the author needs direct control over mathematical notation, diagrams, embedded calculations, interactive visuals, assessment items, and export behavior.

The project also acts as a testbed for studying the authoring experience itself. The current builds include local UX telemetry so repeated pain points can be identified from actual interactions instead of guessed from intuition.

## Architecture

The current architecture is a single-file browser application. The HTML file contains the app shell, styles, runtime JavaScript, built-in lessons, editor workspace, export logic, and diagnostic tools.

Major architectural parts:

### 1. Lesson model

A lesson is represented as structured JSON. At minimum, it has:

- metadata such as title, source, and schema version;
- an array of sections;
- optional quiz questions;
- optional references to code blocks, media, tables, plots, or diagrams.

Sections are the main unit of instruction. A section may contain HTML, LaTeX, Mermaid blocks, tables, JSXGraph blocks, Plotly widgets, SVG, or Manim video slots.

### 2. Rendering layer

The rendering layer converts lesson content into visible panels. It initializes KaTeX, Mermaid, Plotly, JSXGraph, Arquero, statistics widgets, and media blocks after content is inserted into the DOM.

### 3. Authoring workspace

The authoring workspace is the full-page Lesson Editor. It supports:

- CodeMirror source editing with highlighting;
- section editing;
- quiz editing;
- code-block editing;
- live preview;
- save/export controls;
- built-in lesson editing through editable saved copies.

### 4. Persistence layer

The app uses `localStorage` for imported lessons, saved copies, progress state, quiz state, notes, and development telemetry. This keeps the app local and portable, but it also means data lives in the browser profile unless explicitly exported.

### 5. Export layer

The export layer supports lesson JSON export and student-facing HTML export. The student export is intended to be a lighter, learner-facing version of the authored lesson with quiz and progress behavior.

### 6. Diagnostics and testing

The app includes diagnostics for export, print layout, renderer stress tests, and local UX telemetry. These tools are development aids. They are not final product features yet.

## Object-oriented analysis

Although the current implementation is a single-file HTML application, the underlying design can be described with object-oriented concepts.

### Core objects

#### `Lesson`

Represents a complete instructional unit.

Responsibilities:

- hold lesson metadata;
- own sections and quizzes;
- provide source material for render/export;
- define the unit of save/import/export.

#### `Section`

Represents one navigable unit inside a lesson.

Responsibilities:

- hold authored content;
- expose a label and stable id;
- contain renderable blocks such as math, diagrams, tables, and widgets.

#### `RenderableBlock`

Represents a content block requiring specialized handling.

Subtypes include:

- `MathBlock` for KaTeX-rendered notation;
- `MermaidBlock` for diagrams;
- `PlotlyBlock` for graphs;
- `JSXGraphBlock` for geometry boards;
- `ArqueroBlock` for data tables;
- `ManimBlock` for externally rendered videos;
- `SVGBlock` for vector graphics.

#### `QuizQuestion`

Represents an assessment item.

Subtypes include:

- multiple choice;
- true/false;
- math/free-response;
- explanatory free response.

Responsibilities:

- store prompt, answer, feedback, and hints;
- provide grading metadata;
- support student review and reset behavior.

#### `EditorWorkspace`

Represents the authoring environment.

Responsibilities:

- open lessons for editing;
- edit sections, quiz questions, and code blocks;
- manage CodeMirror source editors;
- show live preview;
- save and export lessons.

#### `PersistenceStore`

Represents local browser storage.

Responsibilities:

- save imported lessons and edited copies;
- save active lesson state;
- save quiz/progress state;
- save notes and telemetry.

#### `Exporter`

Represents output workflows.

Responsibilities:

- export lesson JSON;
- export student HTML;
- run preflight diagnostics;
- prepare print/export layout.

#### `TelemetryRecorder`

Represents the local-only UX statistics mechanism.

Responsibilities:

- record development interactions;
- separate author, learner, and mixed-development modes;
- export JSON for analysis;
- clear telemetry state between test passes.

## How to use it

### Basic local use

1. Extract the ZIP package.
2. Open `START_HERE.md`.
3. Open `mathlesson.v4.9.5.html` in a browser.
4. Use the sidebar to select a lesson.
5. Use the Lesson Editor to edit the active lesson.
6. Save changes.
7. Export JSON or export a student-facing HTML lesson.

### Editing a built-in lesson

1. Open the app.
2. Select a built-in lesson from the main lesson selector.
3. Open Lesson Editor.
4. The active lesson should appear in the editor selector.
5. Edit a section or quiz item.
6. Save. The built-in lesson becomes a saved editable copy in local storage.
7. Export the edited lesson JSON if you want a durable file.

### Importing lesson JSON

1. Use `Import JSON` from the app or editor.
2. Select one or more `.json` files.
3. Duplicate lessons should be skipped with a visible notice.
4. Imported lessons appear in the lesson selector and editor selector.

### Exporting a student lesson

1. Open the lesson in the editor.
2. Save the lesson.
3. Use `Export Student Lesson + Quiz`.
4. Open the exported HTML file separately to test learner behavior.

### Testing

Use:

`checklists/mathlesson-v4.9.5-master-test-checklist.md`

Testing should be gate-based. Do not repeatedly rerun broad historical tests unless a gate fails.

## Current limitations

- The project is still a single-file application.
- The authoring workspace is not optimized for phone-sized screens.
- Browser print APIs have limited support for mixing portrait and landscape orientation within a single print job.
- Manim content must be rendered externally and then embedded as video.
- PowerPoint export is intentionally deferred.
- UX telemetry is a development aid and should be removed or gated before any public production release.

## Development direction

Near-term work should focus on stabilizing the authoring workspace, improving export reliability, and deciding whether the project is ready for a modular build system such as Vite.

A future architecture may split the current single-file app into modules for lesson schema, renderers, editor components, exporters, diagnostics, and telemetry. That migration should happen only after the current UX defects are sufficiently understood.

## License

No license has been selected yet. Add a license before publishing this repository for public reuse.

### v4.9.5 answer-checking hardening

This version keeps regex answer checking and hardens the answer-checking route. Text and regex answers are checked before symbolic parsing, and MathLive/ComputeEngine parsing is avoided for plain text answers. Use `reference-lessons/answer-checking-diagnostics-v1.0.json` to verify exact, alternate, regex, numeric, and symbolic checks.
