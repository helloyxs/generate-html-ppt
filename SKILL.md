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
- **Mode C: Cover Generation** — Generate multi-platform social media covers based on a PPT or article. Go to Phase 5.

Do not generate all slides in a single pass, and do not start building before the outline is confirmed.

### Phase 1: Outline & Style Preview

1. **Read the Templates**
   The skill uses distinct templates for different visual styles. Do not generate all styles in one presentation; each style is a separate PPT.
   - For Cyberpunk Dark, read `resources/template.html` (relative to this SKILL.md file) to familiarize yourself with the HTML structure, CSS variables, and layout classes.
   - For Swiss International Style (瑞士国际主义), this is a completely independent template. You must directly use the local `resources/template-swiss.html` as a complete, standalone template rendering, not depending on `template.html`. **CRITICAL: You MUST read `references/swiss-layout-lock.md` and `references/layouts-swiss.md` before generating any Swiss style slides.** You are strictly locked to using only the 22 pre-defined layout components (S01-S22). You MUST set `data-layout="Sxx"` on every `<section class="slide">` and you MUST NOT invent new layouts or use CSS classes that don't exist in the layout definitions.

2. **Requirements Alignment (7-Question Checklist)**
   Before drafting anything, you MUST ask the user to clarify their requirements if they haven't provided a complete outline. Use these 7 questions:
   - 1. Style A (Cyberpunk) or Style B (Swiss)?
   - 2. Who is the audience / What is the scenario?
   - 3. What is the presentation length?
   - 4. Are there raw materials/documents?
   - 5. Are there images/screenshots, and how should they be processed?
   - 6. Which theme color preset?
   - 7. Any hard constraints?
   
2.5 **Brand Asset Protocol (品牌嗅探)**
   **CRITICAL RULE**: 如果用户提供或暗示了特定的公司/品牌，必须优先通过 Web Search 或读取本地资料，提取其核心品牌色（HEX/RGB）和字体风格，并将其注入为 CSS Variables（例如覆盖默认的 `var(--theme-primary)`），使 PPT 天然契合用户的企业 VI。
3. **Draft the Outline (Narrative Arc)**
   Based on the aligned requirements, draft the outline using a Narrative Arc:
   - Hook (1 slide)
   - Context (1-2 slides)
   - Core (3-5 slides)
   - Shift (1 slide)
   - Takeaway (1-2 slides)
   Generate **two separate preview HTML files** if the user is unsure about the style. Stop and let the user confirm the outline and visual direction.

### Phase 1.5: Image Generation & Screenshot Beautification

4. **Prepare Visual Assets**
   Once the outline is confirmed, actively ask the user if they want to generate AI images or beautify their screenshots.
   - If they have raw screenshots, read `references/screenshot-framing.md` to learn how to adapt them using the built-in assets in `resources/screenshot-backgrounds/`.
   - If they need AI generated photos, infographics, or evidence charts, read `references/image-prompts.md` for standardized prompts.
   - Do NOT proceed to Phase 2 until all visual assets are prepared or the user declines.

### Phase 2: Wireframing (灰度骨架) & Setup

5. **Set Up the File**
   Depending on the user's chosen style:
   - If **Cyberpunk Dark**, copy `resources/template.html` into the workspace.
   - If **Swiss International Style**, completely use the local independent template `resources/template-swiss.html`.
   Replace the placeholders (like `{{LANG}}`, `{{TITLE}}`, `{{WATERMARK}}`) in the chosen template.

6. **Generate Wireframe (骨架确认)**
   **CRITICAL: Do NOT generate the full detailed content yet.** First, generate a **Wireframe** of the presentation.
   - Build the HTML structure for the slides according to the outline, but **only** include slide titles, structural layout classes (e.g., `.grid-2`, `.card`), and image placeholders.
   - Use placeholder text (e.g., "Content goes here" or `lorem ipsum`) for detailed paragraphs.
   - Stop and show this wireframe HTML to the user (e.g., open it in the browser). Ask: "Is this layout rhythm and information hierarchy correct?"
   - Do NOT proceed to content filling until the user approves the wireframe.

### Phase 2.5: Content Filling & Batch Generation

7. **Generate Complete Deck in Batches**
   Once the wireframe is approved, fill in the detailed copy and actual content. Insert each batch into the established slide structures (replacing the placeholders). Each slide must remain wrapped in a `<div class="slide" id="s{N}">...</div>` with sequential numbering; only the first slide gets the `active` class (e.g., `<div class="slide active" id="s1">...</div>`). Keep style and density consistent. Report progress briefly after each batch and continue until complete.

### Phase 3: Verify and Open

5. **Verify the Result and Open**
   - **If using Swiss International Style:** You MUST run the validation script before proceeding. Run:
     `node scripts/validate-swiss-deck.mjs <your_generated_file.html>`
     If the script outputs any Errors, you MUST fix the HTML according to the error messages until the script passes without errors.
   - Open the generated HTML in a browser to check. The template clips overflow silently (`overflow: hidden`). Check each slide for clipped, crowded, or overlapping content. Check cross-batch consistency.
   - Finally, ensure the completed presentation is opened in the browser for the user to view.

## Reference Documents

Before generating the presentation or making style choices, you MUST consult the following reference documents located in the `references/` directory:

- **Layouts & Skeletons**: 
  - Read `references/layouts.md` for the 10 layout skeletons and theme rhythm rules when using Cyberpunk/Style A.
  - Read `references/layouts-swiss.md` and `references/swiss-layout-lock.md` for the 22 locked layouts when using Swiss International Style.
- **Components**: Read `references/components.md` for a full list of available UI components, cards, diagrams (Mermaid, ECharts), and animations.
- **Themes & Fonts**: Read `references/themes.md` for styling, fonts, and theme configurations.
- **Quality Control**: Read `references/checklist.md` for content density limits, 16:9 stage constraints, and the P0-P3 quality checks. You MUST ensure your generated presentation passes this checklist.


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

## Phase 5: Cover Generation

When the user requests to generate platform covers (e.g., WeChat banner 21:9, Xiaohongshu 3:4, etc.):
1. **Extract Core Message**: Analyze the provided article, text, or PPT content to extract a highly condensed core message and an eye-catching short title (under 10 words).
2. **Consult Reference**: You MUST read `references/covers.md` for platform-specific design guidelines and exact prompt templates.
3. **Generate Images**: Use Codex or the available Image Generation tools to produce the covers. Do not output HTML code for covers, output the actual image assets to the `images/` directory.

## Supporting Files

- [extract-pptx.py](file:///Users/yan/ai_std_code/generate-html-ppt/scripts/extract-pptx.py) — Python script for PowerPoint content extraction.
- [template.html](file:///Users/yan/ai_std_code/generate-html-ppt/resources/template.html) — The base HTML presentation template.
