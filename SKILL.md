---
name: generate-html-ppt
description: When the user asks to create an HTML PPT, presentation slides, or a deck, or convert an existing PowerPoint (.pptx) file, use this skill to generate a modern, responsive HTML presentation based on the predefined template. This includes Chinese requests such as 做PPT、幻灯片、演示文稿、网页版PPT、PPT转换.
---

# HTML PPT Generation Skill

When the user requests an HTML presentation or PPT, follow these instructions to create it.

## Overview

You have a ready-to-use HTML presentation template located at `resources/template.html` (relative to this skill). Every slide is laid out on a fixed 1920x1080 stage that JavaScript scales as a whole to fit any window, so the layout is identical on every screen. The template includes modern CSS (grids, cards, images, code and table styles, typography, animations, dark-themed cover) and JavaScript for stage scaling and navigation (keyboard, swipe, buttons, fullscreen, URL-hash restore).

Generation follows a staged workflow: confirm a slide-by-slide outline with the user first, build the deck in small batches, then verify the whole file. Optional font presets in `resources/themes/` restyle the deck's typography without touching the template.

## Instructions

Detect which mode the user wants, and then follow the corresponding phases.

### Phase 0: Mode Detection

Determine what the user wants:
- **Mode A: New Presentation** — Create a presentation from scratch. Go to Phase 1.
- **Mode B: PPT Conversion** — Convert a PowerPoint (`.pptx`) file to HTML. Go to Phase 4.

Do not generate all slides in a single pass, and do not start building before the outline is confirmed.

### Phase 1: Outline & Style Preview

1. **Read the Template**
   First, read the contents of `resources/template.html` (relative to this SKILL.md file) to familiarize yourself with the HTML structure, CSS variables, and layout classes available. Commented-out example slides (a cover slide and a standard content slide) next to the `<!-- SLIDES_GO_HERE -->` marker show the expected structure.

2. **Draft the Outline & Style Preview**
   Based on the user's provided content or topic, draft one line per slide: slide number, title, the layout and components you plan to use (e.g. cover, `.grid-3` cards, `.code-block` + list), and the key points it carries. Check the plan against the Content Density limits while drafting.
   Explain to the user that the generated presentation will natively support 4 visual styles (Cyberpunk, Light, Blue, Emerald) switchable via an interactive theme-switcher UI on the page. Ask the user which style they would prefer as the **default theme** (default is Cyberpunk).

### Phase 2: Build in Batches

3. **Set Up the File**
   Copy the template into the workspace (e.g., `presentation.html` or whatever the user specifies) and replace the placeholders:
   - Replace `{{LANG}}` with the content language code (e.g. `zh-CN` for Chinese, `en` for English).
   - Replace `{{TITLE}}` with the presentation title.
   - Replace `{{WATERMARK}}` with the appropriate watermark text (e.g., company name), or delete the entire watermark `<div>` if not needed.
   - If the user prefers a different default theme than Cyberpunk (e.g., Light, Blue, Emerald), configure the default theme fallback string in the `<script>` block (change `|| 'cyberpunk'` to `'light'`, `'blue'`, or `'emerald'`).

4. **Generate 3-5 Slides per Batch**
   Build the slides following the confirmed outline, inserting each batch in place of the `<!-- SLIDES_GO_HERE -->` marker and re-adding the marker after the inserted slides until the deck is complete. Each slide must be wrapped in a `<div class="slide" id="s{N}">...</div>` with sequential numbering; only the first slide gets the `active` class (e.g., `<div class="slide active" id="s1">...</div>`). Use the components from the UI Components section below, and keep style, color modifiers, and density consistent across batches. Report progress briefly after each batch and continue with the next one; pause for the user only if the outline itself needs to change.

### Phase 3: Verify

5. **Verify the Result**
   Open the generated HTML in a browser and step through every slide (arrow keys). The template clips overflow silently (`overflow: hidden`), so a slide that looked fine in your draft may lose its bottom rows. Check each slide for clipped, crowded, or overlapping content; if you find any, shorten the text or split the slide, then re-check. Because the fixed stage never reflows, the layout is identical at every window size, so checking at a single viewport (e.g. 1280x720) is enough; smaller screens only scale the whole stage down, they do not rearrange anything. Also confirm cross-batch consistency: sequential slide ids, working hash navigation (e.g. `deck.html#s3`), no leftover `{{...}}` placeholders, and one consistent visual style across all batches.

## UI Components

Use the CSS classes provided in the template to create a visually appealing presentation:
- **Typography**: `.slide-label`, `.slide-title`, `.slide-subtitle`, `.em` (blue), `.em-teal`, `.em-red`, `.em-amber`.
- **Layouts**: `.grid-2`, `.grid-3`, `.grid-4` for multiple columns; `.stack-col` for a vertical stack of `.stack-row` layers.
- **Cards**: `.card` (general card), `.feat-card` (feature card with full colored border), `.plat-card` (compact platform card).
- **Color variants**: colored components take a `-blue` / `-teal` / `-red` / `-amber` / `-violet` (or `.b-*` / `.fill-*`) modifier class; always pair them:
  - `.card` + `.b-blue` / `.b-teal` / `.b-red` / `.b-amber` (adds a thick colored top border and colors all list bullet dots inside the card). **IMPORTANT:** Always use these color modifiers on `.card` elements side-by-side in a grid to visually distinguish different key points or topics (e.g., `<div class="card b-blue">`, `<div class="card b-teal">`).
  - `.card` + `.fill-blue` / `.fill-teal` / `.fill-red` / `.fill-amber` / `.fill-violet` (adds a full 2px colored border and a soft wash background). Use this when you want a strongly highlighted, fully colored box.
  - `.feat-card` + `.feat-blue` / `.feat-teal` / `.feat-red` / `.feat-amber` (sets full border and heading color).
  - `.badge` (inside `.card`): automatically inherits the color of the parent card if the card uses a `.b-*` or `.fill-*` modifier. **TIP:** Place the `<span class="badge">` directly inside the `<h3>` (e.g., `<h3><span class="badge">1</span> Title</h3>`) to align the badge and the text perfectly on the same line.
  - `.tag` (standalone label) + `.tag-blue` / `.tag-teal` / `.tag-red` / `.tag-amber`.
  - `.stack-row` + `.stack-blue` / `.stack-teal` / `.stack-red` / `.stack-amber` / `.stack-navy` (for layered architecture diagrams).
- **Images**: `.slide-img` for a single image (contain-fit, max 562px tall); wrap multiple `<img>` in `.img-grid` for an auto-fitting image grid. Always include descriptive `alt` text.
- **Code & Tables**: `.code-block` on a `<pre><code>` for automatic syntax highlighting (via Highlight.js); `.data-table` on a `<table>` (with `<th>` header cells) for data tables; inline `<code>` inside cards, lists, and paragraphs is styled automatically.
- **Diagrams & Markdown**: Wrap inline diagrams in `<pre class="mermaid">` (rendered by Mermaid.js); wrap markdown text blocks in elements with `class="markdown"` (rendered by Marked.js).
- **Speaker Notes**: Add `<aside class="notes">Speaker notes content here</aside>` inside any slide. Notes are invisible in the presentation view but displayed in Speaker View.
- **Cover Slide**: Use `.cover-bg`, `.cover-grid`, `.cover-content`, `.cover-logo` (accent word inside `<span>`), `.cover-title` (gradient text via `<span class="h">`), `.cover-divider`, `.cover-sub` (highlight via `<span class="warm">`), and `.cover-stats` with `.cover-stat` items (`.n` = number, `.l` = label) for key metrics.
- **Footers**: Use `.bottom-strip` for key takeaways at the bottom of a slide; inside it, `.em` renders teal and `.wm` renders amber.
- **Animations**: Add `.a` and `.a1` through `.a6` to elements for staggered fade-up animations when the slide becomes active. For inline SVG diagrams, `.flow-anim` / `.flow-anim-slow` animate dashed connector lines and `.pulse-dot` makes a dot pulse.

## Themes & Fonts

The template's visual style is driven by CSS variables for colors (e.g., `--cover-bg`) and fonts (e.g., `--font-body`). These styles are configured via small CSS snippets in the `resources/themes/` directory. To apply a style, paste the contents of the chosen `.css` file into the `<style>` block right after the template's `:root { ... }` rule.

**Metadata-driven Theme Selection:** Use the following metadata tags (`mood`, `tone`, `formality`, `best_for`) to intelligently select the appropriate styles for the Safe and Bold preview options.

**Color Themes (`theme-*.css`):**
- `theme-blue.css` — [mood: tech, authoritative | tone: modern | formality: medium | best_for: tech talks, corporate IT, product launches] Deep blue tech look (the template's default).
- `theme-light.css` — [mood: clean, institutional | tone: objective, restrained | formality: high | best_for: formal reports, finance, legal, board decks] Bright, clean white/gray look.
- `theme-emerald.css` — [mood: organic, vibrant | tone: fresh, expressive | formality: medium-low | best_for: sustainability, creative pitches, bold brand statements] Rich, organic deep green.

**Font Presets (`font-*.css`):**
- `font-modern.css` — [mood: neutral | formality: medium | best_for: standard corporate presentations] The default look (PingFang/YaHei sans).
- `font-serif.css` — [mood: literary, authoritative | formality: high | best_for: formal reports, finance, policy, and executive decks] Serif display titles over a sans body.
- `font-tech.css` — [mood: geeky, precise | formality: low | best_for: engineering, web3, developer topics] Monospace titles and labels.
- `font-rounded.css` — [mood: playful, warm | formality: low | best_for: friendly, education, consumer topics] Rounded display titles.

Choose the theme and preset algorithmically based on these metadata tags during Phase 1. When the tone is ambiguous, use `blue` + `modern`. Presets use system fonts only — never add webfont links or `@font-face` with external URLs, since the deck must work as a single offline HTML file.

## Content Density

Slides clip overflowing content (`overflow: hidden`), so anything that does not fit disappears silently. Keep every slide within these limits:

- One `.slide-title` and at most one `.slide-subtitle` per slide.
- At most 4 cards in `.grid-4`, 3 in `.grid-3`, or 4 (two rows of two) in `.grid-2`.
- Per `.card`: one `<h3>` plus either one paragraph (about 2 lines) or a list of at most 4 items.
- A `.bottom-strip` also takes vertical space, so shorten card text when you use one.
- Media counts too: treat a `.code-block`, `.data-table`, or `.img-grid` as roughly one card row when budgeting space.
- If content exceeds these limits, split it across two slides. Do not shrink text below the template's base font sizes to force a fit.
- As a last resort for dense reference slides, add the `scroll` class to the slide (`<div class="slide scroll" ...>`) to enable vertical scrolling. Prefer splitting slides instead.

## Fixed 16:9 Stage

Every slide is authored at a fixed 1920x1080 size. JavaScript scales the whole stage with a single `transform` to fit the window, letterboxing on phones or unusual aspect ratios. Content never reflows and there are no responsive breakpoints, so a slide looks identical on every screen, just scaled. All slide-internal sizes are fixed pixels at the 1920x1080 design size: do not add media queries, `vw`/`vh` units, or `clamp()` to slide content, and do not rearrange layouts for small screens.

To prevent visible gaps or seams between the 16:9 stage and the surrounding viewport letterbox margins (especially on wide screens), the template script dynamically matches the viewport background color to the active slide's computed background. When customizing slide backgrounds (e.g., for dark mode), apply the background color directly to the `.slide` elements (or via a theme class on the slides) so that JavaScript's `window.getComputedStyle(slide).backgroundColor` can read and propagate it to the viewport.

## Navigation & Export

The template script scales the fixed 16:9 stage to the window and provides arrow-key/space/PageUp/PageDown/swipe navigation, clickable dots and arrows, `Home`/`End` to jump to the first/last slide, `F` for fullscreen, and `S` (or the top-left speaker button) for **Speaker View** (opens a synced window with timer, current/next slide titles, and speaker notes). The current slide is synced to the URL hash (e.g. `deck.html#s3`), so a refresh keeps the position and slides can be deep-linked. Print styles are built in: printing to PDF renders every slide on its own 16:9 page without navigation chrome.

## Constraints
- Follow the workflow: confirm the outline before building, and never dump the entire deck in one response.
- Ensure the JavaScript at the bottom of the template remains intact so navigation, fullscreen, and URL-hash restore keep working.
- Maintain a single HTML file output unless the user specifically asks for external CSS/JS.
- Respect the content density limits; overflowing content is clipped, not scrolled.

## Phase 4: PPT Conversion

When converting PowerPoint files:

1. **Extract content** — Run the Python script:
   ```bash
   python scripts/extract-pptx.py <input.pptx> <output_dir>
   ```
   *(Note: The script requires `python-pptx`, which can be installed via `pip install python-pptx`)*
2. **Confirm with user** — Present extracted slide titles, content summaries, speaker notes, and image counts to the user.
3. **Style Selection** — Proceed to Phase 1 (Outline & Style Preview) to confirm the visual theme and slide outline.
4. **Generate HTML** — Map the JSON structure (`extracted-slides.json`) to HTML slides. For each slide:
   - Use the appropriate layout classes and components (`.grid-2`, `.grid-3`, `.card`, etc.) to format the extracted text.
   - Embed extracted images from the `assets/` folder.
   - Insert speaker notes into `<aside class="notes">`.

## Supporting Files

- [extract-pptx.py](file:///Users/yan/ai_std_code/generate-html-ppt/scripts/extract-pptx.py) — Python script for PowerPoint content extraction.
- [template.html](file:///Users/yan/ai_std_code/generate-html-ppt/resources/template.html) — The base HTML presentation template.

