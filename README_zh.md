# Generate HTML PPT (网页版 PPT 生成工具)

一个用于创建精美、交互式 HTML 演示文稿（PPT）的 AI Coding-Agent Skill（编程助手技能），支持从零开始生成或转换现有的 PowerPoint (`.pptx`) 文件。本技能专为本地编程助手（如 Claude Code, Antigravity, Gemini CLI 等）打包设计，具备固定 16:9 舞台布局、原生多主题切换、内置导航控制、代码高亮、Markdown 渲染、Mermaid 矢量图表以及独立的双窗口演讲者视图（Speaker View）。

---

## 功能特性

**Generate HTML PPT** 能够帮助您在无需编写复杂 CSS 或 JavaScript 的情况下创建精美的网页版演示文稿。它为 AI 助手提供了一套结构化的生成工作流，并为演讲和阅读者在浏览器中提供极具质感、响应式的演示体验。

### 核心亮点

- **零依赖 (Zero Dependencies)** — 输出单个、完全自包含的 HTML 文件（包含内联 CSS/JS 以及 CDN 备用链接）。无需安装 npm、构建系统或搭建框架，支持离线即开即用。
- **交互式主题切换** — 生成的幻灯片内置主题切换悬浮菜单，读者可在播放时一键实时切换 4 种视觉风格（`Cyberpunk` 赛步朋克、`Light` 极简浅色、`Blue` 科技深蓝、`Emerald` 有机翡翠绿）。
- **模块化字体预设** — 支持无缝切换不同的排版字型（`Modern` 现代、`Serif` 衬线/人文、`Tech` 极客等宽、`Rounded` 圆体）以契合不同受众。
- **固定 16:9 舞台缩放** — 基于 `1920x1080` 像素分辨率设计。视口通过 CSS transforms 自动对整个舞台进行等比缩放，完美自适应任何屏幕尺寸或纵横比，绝不发生布局折行或错位（“所见即所得”）。
- **双屏同步演讲者视图** — 按 `S` 键可启动通过 `BroadcastChannel` 实时同步的独立演讲者控制台，包含演讲计时器、下一页幻灯片预览以及可滚动的演讲备注（备注支持 Markdown）。
- **丰富的 UI 组件库** — 包含响应式网格布局、带颜色和边框修饰符的卡片组件（Cards）、数据图表、代码块高亮、Marked.js 渲染的 Markdown 文本以及 Mermaid.js 渲染的矢量流向图。
- **PowerPoint 转换** — 内置 Python 解析脚本，可提取 PPTX 文件中的文本、幻灯片备注以及图片资产，方便将传统 PPT 轻松升级为网页版演示文稿。
- **微交互与动画** — 支持元素按序延迟淡入（Staggered Fade-up）入场动画，并支持 SVG dashed 连接线流动等精美微动画。

---

## 项目结构

项目采用极简的轻量化目录结构，非常便于 AI 助手读取和调用：

```text
generate-html-ppt/
├── SKILL.md                  # AI 编程助手的工作流地图与生成规则
├── README.md                 # 英文官方文档
├── README_zh.md              # 中文官方文档
├── resources/
│   ├── template.html         # 包含导航、基础样式和 JS 交互核心的 HTML PPT 模版
│   └── themes/               # CSS 主题变量及字体排版定义
│       ├── theme-blue.css    # 专业、权威的科技蓝主题变量
│       ├── theme-light.css   # 极简、正式的白灰色系主题变量
│       ├── theme-emerald.css  # 有机、新鲜的翡翠绿主题变量
│       ├── font-modern.css   # 默认清爽的无衬线字体预设
│       ├── font-serif.css    # 文雅人文的衬线展示字预设
│       ├── font-tech.css     # 适合技术主题的精准等宽字预设
│       └── font-rounded.css  # 亲和力强的圆体展示字预设
└── scripts/
    └── extract-pptx.py       # 用于从 PPTX 中提取文字、图片和讲稿的 Python 脚本
```

---

## 安装说明

### 在 Claude Code 中安装
您可以将本 Skill 复制到本地 Claude 技能目录下，作为本地独立的 Claude Code 命令行扩展使用：

```bash
# 创建 Skill 目录
mkdir -p ~/.claude/skills/generate-html-ppt/resources/themes
mkdir -p ~/.claude/skills/generate-html-ppt/scripts

# 复制文件到对应路径
cp SKILL.md ~/.claude/skills/generate-html-ppt/
cp resources/template.html ~/.claude/skills/generate-html-ppt/resources/
cp resources/themes/*.css ~/.claude/skills/generate-html-ppt/resources/themes/
cp scripts/extract-pptx.py ~/.claude/skills/generate-html-ppt/scripts/
```

或者直接克隆本仓库：
```bash
git clone https://github.com/your-username/generate-html-ppt.git ~/.claude/skills/generate-html-ppt
```

安装完成后，可在 Claude Code 中输入以下指令触发：
```text
/generate-html-ppt
```

### 在其他 Coding Agents（如 Antigravity, Gemini CLI 等）中调用
直接向您的编程助手提供本仓库的 URL，或直接将 `SKILL.md` 文件的内容发送给它。具备文件读取和终端执行权限的编程助手会自动检索并读取 `resources/template.html` 模版，并开始分阶段的幻灯片生成流程。

---

## 使用指南

### 1. 从零开始生成演示文稿
运行 Skill 并向 AI 描述您的 PPT 主题：
```text
/generate-html-ppt "我想为一个新的开源数据库项目制作一份路演 PPT"
```
AI 助手将执行以下步骤：
1. 规划每一页幻灯片的纲要（Outline），详细列出标题、排版网格以及包含的组件。
2. 根据演示主题基调选择匹配的默认主题（Cyberpunk, Light, Blue 或 Emerald）和字体预设。
3. 以每批 3~5 页的节奏分批编写幻灯片并插入模版中。
4. 校验生成后的 HTML，确保文字在 16:9 固定舞台上没有发生任何溢出或裁剪。

### 2. 转换现有的 PPTX 文件
向 AI 助手提供本地 PowerPoint 文件路径：
```text
/generate-html-ppt "把我的 Q3_Product_Roadmap.pptx 文件转换为网页演示文稿"
```
AI 助手将执行以下步骤：
1. 运行 `python scripts/extract-pptx.py Q3_Product_Roadmap.pptx .` 提取幻灯片中的文本、演讲备注与图片（数据将保存至 `extracted-slides.json`，图片导出至 `assets/` 目录）。
2. 向您展示提取出的幻灯片内容摘要以作确认。
3. 规划幻灯片布局，将提取出的文字和图片映射到模版网格中。
4. 自动生成包含所有原始图片、文字和演讲备注的 HTML 演示文稿。

---

## 设计系统与 UI 组件

基础模版内置了开箱即用的实用类，所有排版元素均基于 `1920x1080` 设计像素进行布局：

* **网格布局 (Grids)**：`.grid-2`（双栏）、`.grid-3`（三栏）、`.grid-4`（四栏），用于快速排布卡片。
* **卡片组件 (Cards)**：
  - `.card` — 普通卡片容器。
  - `.card` + `.b-blue` / `.b-teal` / `.b-red` / `.b-amber` — 顶部带有对应色块的卡片，内部列表的项目符号自动变色。非常适合并排在网格中以区分不同要点。
  - `.card` + `.fill-blue` / `.fill-teal` / `.fill-red` / `.fill-amber` / `.fill-violet` — 带有完整高对比边框和淡色背景水洗效果的卡片。适合强调某个核心板块。
  - `.feat-card` + `.feat-blue` / `.feat-teal` / `.feat-red` / `.feat-amber` — 完整高质感特色卡片。
* **矢量图表 (Mermaid)**：直接在 `<pre class="mermaid">` 中编写 Mermaid 语法，即可渲染矢量关系图或流程图。
* **动效支持 (Animations)**：给幻灯片元素添加 `.a` 基础类，并配合 `.a1` 至 `.a6` 等级，即可实现内容按顺序延迟渐显入场。
* **架构层级**：使用 `.stack-col` 与 `.stack-row`（配合 `.stack-blue` / `.stack-teal` 等）快速搭建垂直层级架构图。

---

## 主题与排版预设

AI 助手将根据您的 PPT 调性、正式程度和演示场景，算法化匹配最适合的主题与字体：

### 颜色主题
| 主题 CSS | 氛围基调 | 正式度 | 推荐应用场景 |
| :--- | :--- | :--- | :--- |
| **Cyberpunk** (默认) | 高对比度、赛博霓虹卡片发光 | 中 - 低 | 技术分享、创意项目路演、极客演讲 |
| `theme-blue.css` | 专业、严谨、沉稳蓝色调 | 中 | 企业 IT 介绍、新产品发布、SaaS 路演 |
| `theme-light.css` | 极简、客观、干净灰白色调 | 高 | 财务季度汇报、法律提案、董事会报告 |
| `theme-emerald.css` | 有机、新鲜、环保绿色调 | 中 - 低 | 可持续发展倡议、创意品牌宣讲、设计展 |

### 字体预设
- `font-modern.css` — 现代无衬线中英文字体栈，适合标准商业场景。
- `font-serif.css` — 经典衬线标题搭配无衬线正文，适合高正式度、富有人文气息的报告。
- `font-tech.css` — 等宽字体预设，为工程或开发者主题带来精准极客感。
- `font-rounded.css` — 亲和温暖的圆体字预设，适合大众消费、少儿教育或轻松沙龙场景。

---

## 幻灯片快捷键与播放控制
* **下一页**：`右方向键`、`下方向键`、`空格键`、`PageDown`，或在移动端向左轻扫。
* **上一页**：`左方向键`、`上方向键`、`PageUp`，或在移动端向右轻扫。
* **跳转首页/末页**：`Home` / `End` 键。
* **全屏切换**：按 `F` 键。
* **演讲者视图**：按 `S` 键（或点击左上角麦克风/演讲者图标），自动弹出双屏同步控制窗口。
* **页面锚点定位**：幻灯片状态实时与 URL hash 同步（如 `deck.html#s3`），刷新页面或分享链接将直接定位至目标页。

---

## 内容密度指南
因为 16:9 舞台在视口溢出时会进行静默裁剪，为了保证最佳显示效果，请让内容遵循以下配额上限：
1. **标题字数**：每页幻灯片最多使用一个 `.slide-title` 和一个 `.slide-subtitle`。
2. **卡片数量**：在网格中，三栏最多放 3 张卡片，四栏最多放 4 张卡片，双栏最多放两行 4 张卡片。
3. **卡片字数**：每张卡片包含一个 `<h3>` 标题，下方最多跟一个 2 行的正文段落，或最多 4 条的无序列表。
4. **媒体占位**：`.code-block`（代码块）、`.data-table`（表格）或 `.img-grid`（图片组）在空间预算上大致相当于一整行卡片。
5. **滚动逃生舱**：如果因特殊原因（如展示大型数据表）导致幻灯片内容必须超出视口，可以给 slide 容器加上 `scroll` 类（`<div class="slide scroll">`）来启用垂直滚动条。

---

## 系统要求

- **本地 AI 编程助手**（如 Claude Code, Antigravity, Codex 或 Gemini CLI）。
- **Python 3.x** 及其库 `python-pptx`（仅在使用 PPTX 转换功能时需要）：
  ```bash
  pip install python-pptx
  ```
- **网络连接**（可选。本模版从 CDN 异步载入 Highlight.js、Marked.js 和 Mermaid.js。若处于无网环境，幻灯片依然可以流畅播放，仅代码高亮和矢量图表会进行静默降级）。

---

## 项目哲学

1. **零依赖生命周期 (Zero-Dependency)**：单个 HTML 文件不仅能离线使用，可以在任何设备随时运行，且在 20 年后依然能够被完美打开。
2. **等比缩放的排版稳定性 (Layout Consistency)**：固定 16:9 舞台确保幻灯片绝对不会因为屏幕比例不同而发生“错页”或“排版坍塌”，避免了响应式布局在投影仪上的灾难。
3. **实时交互式控制**：在运行的页面中实时一键更改布局、主题或排版，免除在命令行中反复调试 CSS 参数的痛苦。

---

## 开源协议

MIT — 欢迎自由地使用、定制、修改并分享！

## 致谢

本项目灵感源于 [@zarazhangrui](https://github.com/zarazhangrui) 创建的 [frontend-slides](https://github.com/zarazhangrui/frontend-slides) 开源项目。
