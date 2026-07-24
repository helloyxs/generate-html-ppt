# Generate HTML PPT (网页版 PPT 生成工具)

一个用于创建精美、交互式 HTML 演示文稿（PPT）的 AI Coding-Agent Skill（编程助手技能），支持从零开始生成或转换现有的 PowerPoint (`.pptx`) 文件。本技能专为本地编程助手（如 Claude Code, Antigravity, Gemini CLI 等）打包设计，具备固定 16:9 舞台布局、三大视觉风格（赛博朋克、瑞士国际主义、Beautiful.ai 商业风格）、原生多主题切换、内置导航控制、代码高亮、Markdown 渲染、Mermaid/ECharts 图表支持、以及独立的双窗口演讲者视图（Speaker View）。

---
![image](https://github.com/helloyxs/generate-html-ppt/blob/main/demo/beautiful-demo.gif)

## 功能特性

**Generate HTML PPT** 能够帮助您在无需编写复杂 CSS 或 JavaScript 的情况下创建精美的网页版演示文稿。它为 AI 助手提供了一套结构化的生成工作流，并为演讲和阅读者在浏览器中提供极具质感、响应式的演示体验。

### 核心亮点

- **三大视觉流派选择** — 支持充满张力的 Cyberpunk（赛博朋克）、高度严谨的 Swiss International Style（瑞士国际主义）以及优雅现代的 Beautiful Style（Beautiful.ai 商业风格）三大系统，覆盖极客技术到高端商业路演的全场景。
- **零依赖 (Zero Dependencies)** — 输出单个、完全自包含的 HTML 文件（包含内联 CSS/JS 以及 CDN 备用链接）。无需安装 npm、构建系统或搭建框架，支持离线即开即用。
- **交互式主题切换** — Cyberpunk 风格支持在生成的幻灯片内置主题切换悬浮菜单，读者可在播放时一键实时切换 4 种视觉风格和字体预设。
- **固定 16:9 舞台缩放** — 基于 `1920x1080` 像素分辨率设计。视口通过 CSS transforms 自动对整个舞台进行等比缩放，完美自适应任何屏幕尺寸，绝不发生布局折行（“所见即所得”）。
- **双屏同步演讲者视图** — 按 `S` 键可启动通过 `BroadcastChannel` 实时同步的独立演讲者控制台，包含演讲计时器、下一页幻灯片预览以及可滚动的演讲备注（备注支持 Markdown）。
- **PowerPoint 转换** — 内置 Python 解析脚本，可提取 PPTX 文件中的文本、演讲备注以及图片资产，将传统 PPT 轻松升级为网页版演示文稿。
- **AI 视觉与配图生成** — 内置截图美化预设和 AI 配图提示词系统，支持生成适配各大社媒平台（如小红书、微信公众号）的封面图。
- **丰富的 UI 组件库** — 包含响应式网格、高质感卡片、代码高亮、ECharts 数据可视化以及 Mermaid.js 渲染的矢量流向图。

---

## 项目结构

项目结构更加完善，包含完整的 AI 工作流指南和静态资源：

```text
generate-html-ppt/
├── SKILL.md                  # AI 编程助手的工作流地图与生成规则
├── README.md                 # 中英文文档
├── README_en.md
├── references/               # AI 助手的详细工作手册 (排版, 组件, 配图提示词等)
├── resources/
│   ├── template.html         # 赛博朋克风格 (Cyberpunk) 的主模版
│   ├── template-swiss.html   # 瑞士国际主义风格 (Swiss) 的主模版
│   ├── template-beautiful.html # Beautiful.ai 商业风格的主模版
│   └── screenshot-backgrounds/ # 用于自动包裹、美化软件截图的高清底图
└── scripts/
    ├── extract-pptx.py       # 用于从 PPTX 中提取文字、图片和讲稿的 Python 脚本
    └── validate-swiss-deck.mjs # 严格校验瑞士风格排版合规性的 Node.js 脚本
```

---

## 安装说明

### 使用 AI 助手一键安装（推荐）
您可以直接要求您的 AI 编程助手（如 Claude Code, Codex 等）为您安装此技能，只需向 AI 发送以下指令：

```text
请帮我将 https://github.com/helloyxs/generate-html-ppt.git 克隆并安装为你的本地技能，同时安装所需的 python-pptx 依赖。
```

### 在 Claude Code 中手动安装
建议将本 Skill 复制到本地智能助手的技能目录（或项目目录）下作为扩展使用：

```bash
# 1. 克隆本仓库到本地技能目录
git clone https://github.com/helloyxs/generate-html-ppt.git ~/.claude/skills/generate-html-ppt

# 2. 安装 Python 依赖（用于解析 .pptx，仅在使用该功能时需要）
pip install python-pptx

# 3. 安装 Node.js 依赖（可选，如果使用瑞士风格模板，系统会自带执行环境校验脚本）
```

### 在 Codex 中手动安装
将本 Skill 复制到 Codex 的技能目录下：

```bash
# 1. 克隆本仓库到 Codex 技能目录
git clone https://github.com/helloyxs/generate-html-ppt.git ~/.codex/skills/generate-html-ppt

# 2. 安装 Python 依赖（用于解析 .pptx，仅在使用该功能时需要）
pip install python-pptx
```

在交互中，只需给 AI 传入 `SKILL.md`，或者告诉 AI 助手“加载并遵循 `~/.claude/skills/generate-html-ppt/SKILL.md`”（Codex 用户请替换为对应的技能路径）。

---

## 使用指南

### 1. 从零开始生成演示文稿 (Mode A)
运行命令或给 AI 发送需求：
```text
/generate-html-ppt "我想为一个新的开源数据库项目制作一份路演 PPT"
```
AI 助手将遵循结构化工作流：
1. **风格选择与大纲对齐**：通过 7 个检查问题，与您确认观众、时长，并在 **Cyberpunk (赛博朋克)**、**Swiss (瑞士国际)** 和 **Beautiful (商业风格)** 三套模板中选择其一。
2. **准备视觉资产**：自动使用 `screenshot-framing` 规范美化您的截图，或根据 `image-prompts` 生成 AI 插画。
3. **分批生成幻灯片**：每 3~5 页为一批分段生成，确保代码稳定。
4. **验证与交付**：若选择瑞士风格，AI 会自动运行 `validate-swiss-deck.mjs` 脚本校验排版，修复完成后自动在浏览器中打开 PPT。

### 2. 转换现有的 PPTX 文件 (Mode B)
向 AI 助手提供本地 PowerPoint 文件路径：
```text
/generate-html-ppt "把我的 Q3_Product_Roadmap.pptx 文件转换为网页演示文稿"
```
AI 将自动执行 `python scripts/extract-pptx.py`，提取其中的文本与图片，将其内容映射到网页版模板中，同时保留所有的演讲备注（Speaker Notes）。

### 3. 生成自媒体封面 (Mode C)
如果需要将 PPT 内容分发到不同社交平台，可指令：
```text
"根据刚刚生成的幻灯片，帮我设计一张微信公众号(21:9)和小红书(3:4)的封面图"
```
AI 将提取核心信息并使用内置的设计参数调用文生图模型输出封面。

---

## 设计系统与 UI 组件

根据所选模板，HTML PPT 采用截然不同的排版逻辑。

### A. 赛博朋克风格 (Cyberpunk - `template.html`)
灵活度极高的网格系统，组件丰富：
* **网格布局 (Grids)**：`.grid-2`（双栏）、`.grid-3`、`.grid-4`，用于自由排布内容。
* **卡片组件 (Cards)**：带有 `.b-blue`, `.fill-teal`, `.feat-red` 等多种色彩边框和水洗背景的卡片，实现赛博霓虹发光效果。
* **微动效支持**：给元素添加 `.a` 以及 `.a1` 至 `.a6` 等级，即可实现瀑布式延迟渐显入场动画。

### B. 瑞士国际主义风格 (Swiss Style - `template-swiss.html`)
高度锁定的严谨设计系统：
* **22 种专属排版版式 (`S01-S22`)**：遵循包豪斯和瑞士国际主义网格，不允许随意嵌套 class，每一页的 `<section data-layout="Sxx">` 的子元素都有严格的标签层级（如 `.main-content`, `.visual-area` 等）。
* **极简与大留白**：强调信息的纯粹与清晰，依赖大字号和负空间，不需要卡片容器。

### C. Beautiful.ai 商业风格 (Beautiful Style - `template-beautiful.html`)
优雅现代的高端路演设计系统：
* **12 种精品排版 (`L01-L12`)**：提供从开场封面、数据大字报到图片网格等 12 种高质感版式。
* **光晕主题交替**：支持 `light`、`dark` 以及带有渐变光晕的 `hero` 主题交错使用，打造富有节奏感的演示体验。
* **丰富的智能动画与图表**：支持弹簧物理引擎的入场动画（`.anim` 等）、数字滚动增加效果（`.count-up`）以及基于 data 属性一键配置的智能图表（`.smart-chart`）。

### 共通组件
* **矢量图表 (Mermaid.js)**：直接在 `<pre class="mermaid">` 中编写文本语法渲染关系图、架构图或甘特图。
* **数据可视化 (ECharts)**：直接在 `<div class="echarts" data-option='...'>` 中声明 JSON 配置，即可渲染可交互的统计分析图表。

---

## 主题与排版预设

AI 助手将根据您的 PPT 调性匹配主题（仅限赛博朋克模板生效，瑞士风格主题固定）：

### 颜色主题
| 主题 CSS | 氛围基调 | 正式度 | 推荐应用场景 |
| :--- | :--- | :--- | :--- |
| **Cyberpunk** (默认) | 高对比度、赛博霓虹卡片发光 | 中 - 低 | 技术分享、创意项目路演、极客演讲 |
| `theme-blue.css` | 专业、严谨、沉稳蓝色调 | 中 | 企业 IT 介绍、新产品发布、SaaS 路演 |
| `theme-light.css` | 极简、客观、干净灰白色调 | 高 | 财务季度汇报、法律提案、董事会报告 |
| `theme-emerald.css` | 有机、新鲜、环保绿色调 | 中 - 低 | 可持续发展倡议、创意品牌宣讲、设计展 |

### 字体预设
- `font-modern.css` — 现代无衬线中英文字体栈，适合标准商业场景。
- `font-serif.css` — 经典衬线标题搭配无衬线正文，适合高正式度的人文报告。
- `font-tech.css` — 等宽字体预设，为工程或开发者主题带来极客感。
- `font-rounded.css` — 亲和温暖的圆体字预设，适合大众消费或轻松沙龙场景。

---

## 幻灯片快捷键与播放控制
* **下一页**：`右方向键`、`下方向键`、`空格键`、`PageDown`，或在移动端向左轻扫。
* **上一页**：`左方向键`、`上方向键`、`PageUp`，或在移动端向右轻扫。
* **跳转首页/末页**：`Home` / `End` 键。
* **全屏切换**：按 `F` 键。
* **演讲者视图**：按 `S` 键，自动弹出双屏同步的提词器与控制窗口。
* **页面锚点定位**：幻灯片状态与 URL hash 实时同步（如 `#s3`）。

---

## 项目哲学

1. **零依赖生命周期 (Zero-Dependency)**：单个 HTML 文件不仅能离线使用，在任何设备随时运行，且在 20 年后依然能够被完美打开。
2. **等比缩放的排版稳定性 (Layout Consistency)**：固定 16:9 舞台确保幻灯片绝对不会因为屏幕比例不同而发生“错位”或“坍塌”。
3. **AI Native 原生工作流**：将琐碎的代码调试变为对 AI 的逻辑指导。借助校验脚本与锁定的布局系统，大幅降低大模型的幻觉概率。

---

## 开源协议

MIT — 欢迎自由地使用、定制、修改并分享！

## 致谢

本项目灵感源于 [@zarazhangrui](https://github.com/zarazhangrui) 的 [frontend-slides](https://github.com/zarazhangrui/frontend-slides) 以及 [@op7418](https://github.com/op7418) 的 [guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill) 开源项目。