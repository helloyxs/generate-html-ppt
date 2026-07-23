---
name: generate-html-ppt
description: When the user asks to create an HTML PPT, presentation slides, or a deck, or convert an existing PowerPoint (.pptx) file, use this skill to generate a modern, responsive HTML presentation based on the predefined template. This includes Chinese requests such as 做PPT、幻灯片、演示文稿、网页版PPT、PPT转换.
---

# HTML PPT Generation Skill

When the user requests an HTML presentation or PPT, follow these instructions to create it.

## Overview

You have three independent presentation styles available, each with its own complete template:

1. **Beautiful.ai-inspired modern premium** (default) — `resources/template-beautiful.html`
2. **Cyberpunk Dark / Tech / Light / Emerald** — `resources/template.html`
3. **Swiss International Style** — `resources/template-swiss.html`

Every slide is laid out on a fixed 1920×1080 stage that JavaScript scales as a whole to fit any window, so the layout is identical on every screen.

Generation follows a staged workflow: confirm a slide-by-slide outline with the user first, build the deck in small batches, then verify the whole file.

## Default Style Decision Tree

- If the user explicitly asks for **Cyberpunk / 赛博朋克 / tech dark**, use `resources/template.html`.
- If the user explicitly asks for **Swiss / 瑞士国际主义 / Swiss Style**, use `resources/template-swiss.html`.
- Otherwise, including when no style is specified, **default to Beautiful.ai-inspired** using `resources/template-beautiful.html`.

## Phase 0: Mode Detection

Determine what the user wants:
- **Mode A: New Presentation** — Create a presentation from scratch. Go to Phase 1.
- **Mode B: PPT Conversion** — Convert a PowerPoint (`.pptx`) file to HTML. Go to Phase 4.
- **Mode C: Cover Generation** — Generate multi-platform social media covers based on a PPT or article. Go to Phase 5.

Do not generate all slides in a single pass, and do not start building before the outline is confirmed.

## Phase 1: Outline & Style Preview

1. **Read the Templates**
   Read the template that matches the chosen/default style:
   - **Beautiful.ai-inspired (default)**: read `resources/template-beautiful.html` and `references/layouts-beautiful.md`. You are locked to the 12 pre-defined layout components (L01-L12) and the CSS classes in the template. Do not invent new layouts or classes.
   - **Cyberpunk Dark**: read `resources/template.html` and `references/layouts.md`.
   - **Swiss International Style**: read `resources/template-swiss.html`, `references/layouts-swiss.md`, and `references/swiss-layout-lock.md`. You are strictly locked to the 22 pre-defined layouts (S01-S22). Every `<section class="slide">` must set `data-layout="Sxx"`.

2. **Requirements Alignment (7-Question Checklist)**
   Before drafting anything, ask the user to clarify their requirements if they haven't provided a complete outline. Use these 7 questions, with Beautiful.ai as the default style:
   1. **Style** — Beautiful.ai-inspired modern premium (default). Want Cyberpunk or Swiss instead?
   2. Who is the audience / what is the scenario?
   3. What is the presentation length?
   4. Are there raw materials/documents?
   5. Are there images/screenshots, and how should they be processed?
   6. Which theme color preset? (For Beautiful style, default is `indigo` and no theme switcher is generated. If another is requested, replace the `:root` variables with those from `resources/themes/theme-beautiful.css`. Cyberpunk: see `resources/themes/`)
   7. Any hard constraints?

3. **Brand Asset Protocol (品牌嗅探)**
   **CRITICAL RULE**: If the user provides or implies a specific company/brand, prioritize extracting its core brand colors (HEX/RGB) and font style via web search or local materials, and inject them as CSS variables (e.g., override the default `--b-accent`) so the deck naturally matches the corporate VI.

4. **Draft the Outline (Narrative Arc)**
   Based on the aligned requirements, draft an outline using a Narrative Arc:
   - Hook (1 slide)
   - Context (1-2 slides)
   - Core (3-5 slides)
   - Shift (1 slide)
   - Takeaway (1-2 slides)
   If the user is unsure about the style, generate two separate preview HTML files (e.g., Beautiful vs. Cyberpunk) and stop for confirmation.

## Phase 1.5: Image Generation & Screenshot Beautification

5. **Prepare Visual Assets**
   Once the outline is confirmed, actively ask the user if they want to generate AI images or beautify screenshots.
   - If they have raw screenshots, read `references/screenshot-framing.md`.
   - If they need AI generated photos, infographics, or evidence charts, read `references/image-prompts.md`.
   - Do NOT proceed to Phase 2 until all visual assets are prepared or the user declines.

## Phase 2: Wireframing (灰度骨架) & Setup

6. **Set Up the File**
   Based on the chosen/default style:
   - **Beautiful.ai-inspired (default)**: copy `resources/template-beautiful.html`. If the user selected a theme other than `indigo`, replace the `:root` CSS variables in the template with the corresponding ones from `resources/themes/theme-beautiful.css`. Replace `{{LANG}}`, `{{TITLE}}`, `{{WATERMARK}}`.
   - **Cyberpunk Dark**: copy `resources/template.html`. Replace `{{THEME}}` with the chosen preset (e.g., `cyberpunk`, `blue`, etc.).
   - **Swiss International Style**: copy `resources/template-swiss.html`.

7. **Generate Wireframe (骨架确认)**
   **CRITICAL: Do NOT generate the full detailed content yet.** Build the HTML structure for the slides according to the outline, but only include slide titles, structural layout classes, and image placeholders. Use placeholder text for detailed paragraphs. Stop and show this wireframe to the user. Do NOT proceed until approved.

## Phase 2.5: Content Filling & Batch Generation

8. **Generate Complete Deck in Batches**
   Once the wireframe is approved, fill in detailed copy and actual content. Insert each batch into the established slide structures, replacing placeholders. Each slide must remain wrapped in `<div class="slide ..." id="s{N}">...</div>` (Beautiful/Cyberpunk) or `<section class="slide ..." id="s{N}">...</section>` (Swiss), with sequential numbering; only the first slide gets the `active` class. Keep style and density consistent.

## Phase 3: Verify and Open

9. **Verify the Result and Open**
   - **Swiss**: run `node scripts/validate-swiss-deck.mjs <file.html>` and fix any errors.
   - **Beautiful.ai-inspired**: run `node scripts/validate-beautiful-deck.mjs <file.html>` and fix any errors. Also open the generated HTML in a browser to verify that all `.anim` elements animate on entry, smart charts render, count-up numbers animate, and no content is clipped.
   - **Cyberpunk**: open the generated HTML in a browser and check each slide for clipped, crowded, or overlapping content.
   - Finally, ensure the completed presentation is opened in the browser for the user to view.

## Reference Documents

Before generating the presentation, consult the relevant references for the chosen style:

- **Beautiful.ai-inspired**: `references/layouts-beautiful.md`, `references/components.md`, `references/checklist.md` (and its Beautiful section)
- **Cyberpunk**: `references/layouts.md`, `references/components.md`, `references/checklist.md`, `references/themes.md`
- **Swiss**: `references/layouts-swiss.md`, `references/swiss-layout-lock.md`, `references/checklist.md`

## Phase 4: PPT Conversion

When converting PowerPoint files:

1. **Extract content** — Run:
   ```bash
   python scripts/extract-pptx.py <input.pptx> <output_dir>
   ```
   *(Requires `python-pptx`: `pip install python-pptx`)*
2. **Confirm with user** — Present extracted slide titles, content summaries, speaker notes, and image counts.
3. **Style Selection** — Proceed to Phase 1 to confirm the visual theme and outline. Default to Beautiful.ai-inspired unless the user requests another style.
4. **Generate HTML** — Map the JSON structure (`extracted-slides.json`) to HTML slides. Use the appropriate layout classes and components, embed images from `assets/`, and insert speaker notes into `<aside class="notes">`.

## Phase 5: Cover Generation

When the user requests platform covers (e.g., WeChat banner 21:9, Xiaohongshu 3:4, etc.):
1. **Extract Core Message**: Analyze the provided article, text, or PPT content.
2. **Consult Reference**: Read `references/covers.md`.
3. **Generate Images**: Produce the actual image assets to the `images/` directory.

## Supporting Files

All paths are relative to the skill root:

- `scripts/extract-pptx.py` — PowerPoint content extraction.
- `scripts/validate-swiss-deck.mjs` — Swiss style validator.
- `scripts/validate-beautiful-deck.mjs` — Beautiful style validator.
- `resources/template.html` — Cyberpunk / Tech base template.
- `resources/template-swiss.html` — Swiss International template.
- `resources/template-beautiful.html` — Beautiful.ai-inspired premium template (default).
- `resources/themes/theme-beautiful.css` — Beautiful style color presets.
