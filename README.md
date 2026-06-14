# pbgc-learningPad
### Notes tool to enhance the actuarial-mathematical learning.

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
