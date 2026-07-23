# Generate HTML PPT

An AI Coding-Agent Skill for creating stunning, interactive HTML presentations — from scratch or by converting PowerPoint (`.pptx`) files. Packaged for local coding assistants (such as Claude Code, Antigravity, Gemini CLI, etc.), it features a fixed 16:9 stage layout, dual visual styles (Cyberpunk and Swiss International), native multi-theme switching, built-in navigation controls, syntax highlighting, markdown rendering, vector/chart diagrams (Mermaid & ECharts), and a dedicated dual-window Speaker View.

![image](https://github.com/helloyxs/generate-html-ppt/blob/main/demo/demo_pic.png)
---

## What This Does

**Generate HTML PPT** helps you create gorgeous, web-based presentations without writing complex CSS or JavaScript. It provides a structured workflow for AI agents to craft clean slides and offers readers a premium, responsive presentation experience directly in the browser.

### Key Features

- **Dual Visual Systems** — Choose between high-impact *Cyberpunk* and highly rigorous *Swiss International Style*, covering everything from geeky tech talks to commercial design showcases.
- **Zero Dependencies** — Outputs a single, self-contained HTML file (with inline CSS/JS and CDN fallbacks). No npm, build systems, or framework setup required. Works offline instantly.
- **Interactive Style Switching** — The Cyberpunk style features a built-in theme switcher overlay that allows readers to toggle between 4 visual themes and 4 typography presets on the fly.
- **Fixed 16:9 Stage Scaling** — Authored at a fixed `1920x1080` pixel resolution. The viewport dynamically scales the entire stage via CSS transforms to fit any screen size or aspect ratio without layout reflows (True WYSIWYG).
- **Synced Speaker View** — Press `S` to launch a dual-window presenter mode synced via `BroadcastChannel` featuring an elapsed timer, slide preview panel, and scrollable speaker notes.
- **PowerPoint Conversion** — Includes a Python parser script that extracts slide text, notes, and shapes/images from `.pptx` files, making it easy to convert legacy decks to the web.
- **AI Visuals & Cover Generation** — Built-in screenshot beautification guidelines and AI image prompts allow agents to generate aesthetic assets and social media covers (e.g., for LinkedIn or newsletters).
- **Rich UI Component Kit** — Responsive grids, styled cards, code blocks, ECharts data visualization, and Mermaid.js vector diagrams.

---

## Project Structure

The project features a comprehensive directory structure optimized for AI agent workflows:

```text
generate-html-ppt/
├── SKILL.md                  # Workflow map and rules for the AI coding agent
├── README.md                 # English & Chinese Documentation
├── README_en.md
├── references/               # Detailed agent handbooks (layouts, components, themes, prompts)
├── resources/
│   ├── template.html         # Base template for the Cyberpunk style
│   ├── template-swiss.html   # Base template for the Swiss International Style
│   ├── screenshot-backgrounds/ # High-res backgrounds for wrapping/beautifying software screenshots
│   └── themes/               # CSS theme variables and typography definitions (for Cyberpunk)
└── scripts/
    ├── extract-pptx.py       # Python script to extract slides, images, and notes from PPTX
    └── validate-swiss-deck.mjs # Strict Node.js validation script for Swiss layout compliance
```

---

## Installation

### 1-Click Install via AI Assistant (Recommended)
You can directly ask your AI coding assistant (e.g., Claude Code, Codex) to install this skill for you. Just send the following prompt to your AI:

```text
Please clone and install https://github.com/helloyxs/generate-html-ppt.git as your local skill, and also install the required python-pptx dependency.
```

### Manual Install for Claude Code
Clone or copy the repository files to your local agent's skills directory:

```bash
# 1. Clone the repository
git clone https://github.com/helloyxs/generate-html-ppt.git ~/.claude/skills/generate-html-ppt

# 2. Install Python dependency (only for PPTX conversion)
pip install python-pptx

# 3. Node.js is required for running the Swiss layout validation script (validate-swiss-deck.mjs)
```

### Manual Install for Codex
Clone or copy the repository files to your Codex skills directory:

```bash
# 1. Clone the repository
git clone https://github.com/helloyxs/generate-html-ppt.git ~/.codex/skills/generate-html-ppt

# 2. Install Python dependency (only for PPTX conversion)
pip install python-pptx
```

During your chat, just provide the `SKILL.md` file, or tell the agent to "Load and follow `~/.claude/skills/generate-html-ppt/SKILL.md`" (replace with your Codex path if applicable).

---

## Usage Guidelines

### 1. Create a New Presentation (Mode A)
Run the command or describe your topic to the AI:
```text
/generate-html-ppt "I want to create a pitch deck for a new open-source database project"
```
The agent will follow a strict workflow:
1. **Style Selection & Outline**: The AI asks a 7-question checklist to align on audience, length, and style (*Cyberpunk* vs *Swiss*).
2. **Visual Assets**: Beautifies user-provided screenshots using the `screenshot-framing` guide or generates AI illustrations using `image-prompts`.
3. **Batch Generation**: Builds the slides in batches of 3–5 to prevent code generation errors.
4. **Validation**: If using the Swiss style, the AI automatically runs `validate-swiss-deck.mjs` to fix any layout anomalies before opening the HTML in a browser.

### 2. Convert a PowerPoint File (Mode B)
Provide a local `.pptx` file path:
```text
/generate-html-ppt "Convert my Q3_Product_Roadmap.pptx file to a web presentation"
```
The agent will run `python scripts/extract-pptx.py` to extract slide texts, speaker notes, and image assets, then map the content into the chosen HTML template.

### 3. Generate Social Media Covers (Mode C)
You can ask the AI to design cover images:
```text
"Design a 21:9 and a 3:4 cover image for this presentation for my social media"
```
The AI will extract the core message and use its image generation capabilities based on the `covers.md` guideline to output tailored images.

---

## Design System & UI Components

The HTML PPT uses two distinct layout systems based on your chosen template.

### A. Cyberpunk Style (`template.html`)
Highly flexible grid system with abundant components:
* **Grids**: `.grid-2`, `.grid-3`, and `.grid-4` to organize card modules freely.
* **Cards**: Stylized blocks wrapped with modifiers like `.b-blue`, `.fill-teal`, `.feat-red` to create neon glows and structured boxes.
* **Animations**: Apply `.a` together with staggered entry delays `.a1` through `.a6` to animate items sequentially.

### B. Swiss International Style (`template-swiss.html`)
Highly locked and rigorous design system:
* **22 Locked Layouts (`S01-S22`)**: Strict compliance with Bauhaus and Swiss grid principles. Every page `<section data-layout="Sxx">` requires specific child tags (e.g., `.main-content`, `.visual-area`). The AI is instructed never to invent custom CSS or layouts.
* **Minimalism & Negative Space**: Prioritizes pure typography and white space without relying on bounding card containers.

### Common Components
* **Vector Diagrams (Mermaid.js)**: Wrap flowchart/sequence diagram syntax in `<pre class="mermaid">` to render vector graphics natively.
* **Data Visualizations (ECharts)**: Declare JSON configuration in `<div class="echarts" data-option='...'>` to render interactive statistical charts (bars, lines, pies).

---

## Theme & Font Settings

The AI selects themes based on formality and topic (Note: Themes apply to the Cyberpunk template; the Swiss template has its own locked aesthetic):

### Color Themes (Cyberpunk)
| Theme CSS | Mood / Tone | Formality | Best For |
| :--- | :--- | :--- | :--- |
| **Cyberpunk** (Default) | High-impact neon, card glow | Medium-Low | Tech talks, creative pitches, developer decks |
| `theme-blue.css` | Professional, authoritative | Medium | Corporate IT, product launches, SaaS pitches |
| `theme-light.css` | Clean, objective, minimal | High | Financial summaries, legal, formal board decks |
| `theme-emerald.css` | Organic, fresh, expressive | Medium-Low | Sustainability, green tech, design showcases |

### Font Presets (Cyberpunk)
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
* **Direct Links**: URL hashes update automatically (e.g., `#s3`) for direct bookmarking and sharing.

---

## Philosophy

1. **Zero-Dependency Lifespan**: A single HTML file works offline, runs everywhere, and will still open perfectly in 20 years.
2. **Layout Consistency**: A fixed 16:9 canvas guarantees the layout won't break across different screens, avoiding the "reflow ruin" of responsive designs on projectors.
3. **AI-Native Workflow**: Turns CSS tweaking into logical instructions for an AI. With layout validation scripts and rigid schemas, it eliminates AI hallucination in UI generation.

---

## License

MIT — Feel free to use, modify, and share!

## Credits

Inspired by [@zarazhangrui](https://github.com/zarazhangrui)'s [frontend-slides](https://github.com/zarazhangrui/frontend-slides) and [@op7418](https://github.com/op7418)'s [guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill).
