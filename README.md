# Generate HTML PPT

A coding-agent skill for creating stunning, interactive HTML presentations — from scratch or by converting PowerPoint (`.pptx`) files. Packaged for local coding assistants (such as Claude Code, Antigravity, Gemini CLI, etc.), it features a fixed 16:9 stage layout, native multi-theme switching, built-in navigation controls, syntax highlighting, markdown rendering, vector diagrams, and a dedicated dual-window Speaker View.

![image](https://github.com/helloyxs/generate-html-ppt/blob/main/demo/demo_pic.png)
---

## What This Does

**Generate HTML PPT** helps you create gorgeous, web-based presentations without writing complex CSS or JavaScript. It provides a structured workflow for AI agents to craft clean slides and offers readers a premium, responsive presentation experience directly in the browser.


### Key Features

- **Zero Dependencies** — Outputs a single, self-contained HTML file (with inline CSS/JS and CDN fallbacks). No npm, build systems, or framework setup required. Works offline instantly.
- **Interactive Style Switching** — Built-in theme switcher overlay allows readers to toggle between 4 visual styles (`Cyberpunk`, `Light`, `Blue`, `Emerald`) on the fly.
- **Modular Font Presets** — Swaps typography sheets seamlessly (`Modern`, `Serif`, `Tech`, `Rounded`) to fit different target audiences.
- **Fixed 16:9 Stage Scaling** — Authored at a fixed `1920x1080` pixel resolution. The viewport dynamically scales the entire stage via CSS transforms to fit any screen size or aspect ratio without layout reflows.
- **Synced Speaker View** — Press `S` to launch a dual-window presenter mode synced via `BroadcastChannel` featuring an elapsed timer, slide preview panel, and scrollable speaker notes.
- **Rich UI Component Kit** — Responsive grids, styled cards (with color and border modifiers), data tables, code blocks, Marked.js markdown rendering, and Mermaid.js vector diagrams.
- **PowerPoint Conversion** — Includes a Python parser script that extracts slide text, notes, and shapes/images from PowerPoint (`.pptx`) files, making it easy to convert legacy decks to the web.
- **Smooth Micro-Animations** — Supports staggered fade-up entry animations for slide elements and active dashed path connector lines for flowcharts.

---

## Project Structure

The project has a lightweight structure optimized for coding agents:

```text
generate-html-ppt/
├── SKILL.md                  # Workflow map and rules for the AI coding agent
├── README.md                 # Project documentation
├── resources/
│   ├── template.html         # Base HTML deck template with navigation, styling, and script core
│   └── themes/               # CSS theme variables and typography definitions
│       ├── theme-blue.css    # Authoritative tech dark-blue variables
│       ├── theme-light.css   # Minimalist, formal white/gray variables
│       ├── theme-emerald.css  # Organic, fresh green variables
│       ├── font-modern.css   # Default clean sans-serif stack
│       ├── font-serif.css    # Literary display serif stack
│       ├── font-tech.css     # Precise monospace tech stack
│       └── font-rounded.css  # Friendly, rounded display font stack
└── scripts/
    └── extract-pptx.py       # Python script to extract slides, images, and notes from PPTX
```

---

## Installation

### For Claude Code
You can install this skill as a local standalone command by copying the repository files to your local Claude skills directory:

```bash
# Create the skill directories
mkdir -p ~/.claude/skills/generate-html-ppt/resources/themes
mkdir -p ~/.claude/skills/generate-html-ppt/scripts

# Copy files
cp SKILL.md ~/.claude/skills/generate-html-ppt/
cp resources/template.html ~/.claude/skills/generate-html-ppt/resources/
cp resources/themes/*.css ~/.claude/skills/generate-html-ppt/resources/themes/
cp scripts/extract-pptx.py ~/.claude/skills/generate-html-ppt/scripts/
```

Or clone the repository directly:
```bash
git clone https://github.com/your-username/generate-html-ppt.git ~/.claude/skills/generate-html-ppt
```

Once copied/cloned, you can trigger the skill in Claude Code using:
```text
/generate-html-ppt
```

### For Other Coding Agents (Antigravity, Gemini CLI, etc.)
Point the agent to this repository URL or feed it the `SKILL.md` file directly. Coding agents with directory reading and terminal execution privileges will automatically locate `resources/template.html` and begin the staged generation workflow.

---

## Usage

### Create a New Presentation
Run the skill and describe your topic:
```text
/generate-html-ppt "I want to create a pitch deck for a new open-source database project"
```
The agent will:
1. Draft a slide-by-slide outline detailing slide titles, grid layouts, and content components.
2. Select the matching default theme (Cyberpunk, Light, Blue, or Emerald) and font preset.
3. Build the deck in batches of 3–5 slides, writing them sequentially into the template.
4. Verify slide contents to ensure there is no layout overflow or clipped text.

### Convert a PowerPoint File
Point the agent to a local `.pptx` file:
```text
/generate-html-ppt "Convert my Q3_Product_Roadmap.pptx file to a web presentation"
```
The agent will:
1. Run `python scripts/extract-pptx.py Q3_Product_Roadmap.pptx .` to extract slide texts, presenter notes, and image assets (saved to `extracted-slides.json` and `assets/`).
2. Show you the extracted summary for confirmation.
3. Draft a layout plan mapping the extracted content to the HTML PPT grid elements.
4. Generate the final presentation preserving all original images, text, and speaker notes.

---

## Design System & UI Components

The base template includes ready-to-use utility classes to design layouts. Always use them in `1920x1080` design pixels:

* **Grids**: `.grid-2` (2 columns), `.grid-3` (3 columns), and `.grid-4` (4 columns) to organize card modules.
* **Cards**:
  - `.card` — General card wrapper.
  - `.card` + `.b-blue` / `.b-teal` / `.b-red` / `.b-amber` — Adds a colored top border and colors bullet dots inside. Use these side-by-side in grids to group different categories.
  - `.card` + `.fill-blue` / `.fill-teal` / `.fill-red` / `.fill-amber` / `.fill-violet` — High-contrast variant with full border outline and soft wash backgrounds.
  - `.feat-card` + `.feat-blue` / `.feat-teal` / `.feat-red` / `.feat-amber` — Fully stylized feature card blocks.
* **Vector Diagrams**: Wrap your flowchart syntax in `<pre class="mermaid">` to render vector charts.
* **Animations**: Apply `.a` together with staggered entry delays `.a1` through `.a6` to slide children to animate items in sequentially.
* **Architecture Stacks**: Group layered system components using `.stack-col` and `.stack-row` (modified with `.stack-blue` / `.stack-teal` / etc.).

---

## Theme & Font Settings

Themes are selected algorithmically based on formality, mood, and topic:

### Color Themes
| Theme CSS | Mood / Tone | Formality | Best For |
| :--- | :--- | :--- | :--- |
| **Cyberpunk** (Default) | High-impact neon, card glow | Medium-Low | Tech talks, creative pitches, developer decks |
| `theme-blue.css` | Professional, authoritative | Medium | Corporate IT, product launches, SaaS pitches |
| `theme-light.css` | Clean, objective, minimal | High | Financial summaries, legal, formal board decks |
| `theme-emerald.css` | Organic, fresh, expressive | Medium-Low | Sustainability, green tech, design showcases |

### Font Presets
- `font-modern.css` — Clean sans-serif stack.
- `font-serif.css` — Editorial display serif titles over a sans-serif body.
- `font-tech.css` — Monospace headers and labels for engineering topics.
- `font-rounded.css` — Rounded display titles for friendly, consumer, or educational decks.

---

## Navigation Shortcuts
* **Next Slide**: `ArrowRight`, `ArrowDown`, `Space`, `PageDown`, or swipe left.
* **Previous Slide**: `ArrowLeft`, `ArrowUp`, `PageUp`, or swipe right.
* **Jump to Start/End**: `Home` / `End`.
* **Fullscreen**: `F` key.
* **Speaker View**: `S` key (opens synced dual-window view).

---

## Requirements

- **Local Coding Agent** (e.g. Claude Code, Antigravity, , Codex or Gemini CLI).
- **Python 3.x** with `python-pptx` (only for PowerPoint conversion):
  ```bash
  pip install python-pptx
  ```
- **Internet Connection** (optional, to load Highlight.js, Marked.js, and Mermaid.js from CDNs. Fallbacks are included so text and structures degrade gracefully offline).

---

## Philosophy

1. **Zero-Dependency Lifespan**: A single HTML file works offline, runs everywhere, and will still open perfectly in 20 years.
2. **Layout Consistency**: A fixed 16:9 canvas guarantees the layout won't break across different screens, avoiding the "reflow ruin" of responsive designs on projectors or screens.
3. **Interactive Control**: Switch layouts, themes, and typography interactively on the fly rather than wrestling with complex CSS configs.

---

## License

MIT — Feel free to use, modify, and share!

## Credits

Created by [@helloyxs](https://github.com/helloyxs).
