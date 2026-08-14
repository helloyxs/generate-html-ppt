 # Beautiful.ai 风格布局库

 本文档配合 `resources/template-beautiful.html` 使用，提供 12 套可直接复制的版式骨架。

> **图片处理范式升级**：12 套版式里所有图片区都已切换到 [`image-treatments.md`](image-treatments.md) 的 8 套范式（T1-T8）。**默认走 T2 Float / T6 Quiet Frame**；其它范式（截图用 T3、设备用 T5、营销首图用 T8、章节背景用 T7）按场景选。所有图必须写 `data-treatment="t-XXX"` 自检属性，**禁止**给 figcaption 写"原始截图/产品截图/Screenshot"。

| Layout | 图片范式 | 适用 |
|---|---|---|
| L01 开场封面 | T7 Backdrop 或 T1 Bleed | 满屏背景 + 标题 |
| L02 章节幕封 | T7 Backdrop | 模糊背景大字标题 |
| L03 三列卡片 | (无图)或 T8 Edge Card | 卡片自带卡片感,营销可用 T8 |
| L04 数据大字报 | (无图)或 T7 Backdrop | 纯数据时可走 T7 烘托 |
| L05 左文右图 | T2 Float | 单图,纯留白;不加图注 |
| L06 图片网格 | T6 Quiet Frame | 多图分组,单图无 figcap |
| L07 流水线 | (无图) | 流程图为主 |
| L08 智能图表 | (无图) | 图表为主 |
| L09 对比页 | T6 Quiet Frame(双图) | 视觉等价,只靠位置/标题区分 |
| L10 大引用 | (无图) | 文字为主 |
| L11 四列小卡 | (无图) | 卡片为主 |
| L12 收尾页 | T7 Backdrop | 收尾背景图 |



 ## 设计原则

 1. **12 列网格**：所有内容在 1920×1080 的 12 列网格上排布。
 2. **大量留白**：页面四周留 80px/120px 边距，内容之间用 `--space-6` 以上间距。
 3. **主题节奏**：light / dark / hero light 交替使用，每 3-4 页至少一个 hero。
 4. **动画编排**：所有需要入场动画的元素加 `.anim` 和 `.d1`~`.d8`；方向动画加 `.anim-left` / `.anim-right` / `.anim-scale`。
 5. **智能图表**：用 `.smart-chart` + `data-chart` 自动渲染，支持 bar / line / donut / pie / radar。
 6. **数字动画**：大数字加 `.count-up` + `data-value`，进入页面时自动 count-up。
 7. **纵向布局自适应策略（杜绝巨幅中空）**：
    - **无中间扩展内容（如仅有标题+2列卡片）**：使用 **方案一** `.frame.vstack`，设置 `justify-content: flex-start; gap: 40px;`（或 `justify-content: center;`），使内容紧凑靠拢，留白集中在底部。
    - **含流程图/数据指标（3段式结构）**：使用 **方案二** `.frame.between`，配合 `justify-content: space-between`，并在中间插入 `.pipeline`（步骤流程）或 `.stat-card`（指标行）填补空白。

 ## 基础结构

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>栏目名</div><div>页码</div></div>
   <div class="frame">...</div>
   <div class="foot"><div>左下角</div><div>右下角</div></div>
 </div>
 ```

 可用主题类：
 - `.light` — 浅底黑字
 - `.dark` — 深底白字
 - `.hero.light` — 浅底 + 渐变光晕
 - `.hero.dark` — 深底 + 渐变光晕

 ## L01 开场封面 (Hero Cover)

 ```html
 <div class="slide hero light active" id="s1">
   <div class="cover-orb one anim-float"></div>
   <div class="cover-orb two anim-float" style="animation-delay:-2s"></div>
   <div class="cover-frame">
     <div class="cover-logo anim d1">BRAND<span>NAME</span></div>
     <div class="cover-title anim d2"><span class="gradient-text">主标题</span></div>
     <div class="cover-sub anim d3">一句话副标题</div>
     <div class="cover-divider anim d3"></div>
     <div class="cover-stats anim d4">
       <div class="cover-stat"><div class="n">12</div><div class="l">指标</div></div>
       <div class="cover-stat"><div class="n">8</div><div class="l">指标</div></div>
       <div class="cover-stat"><div class="n">∞</div><div class="l">指标</div></div>
     </div>
   </div>
 </div>
 ```

 ## L02 章节幕封 (Act Divider)

 ```html
 <div class="slide hero dark" id="sN">
   <div class="cover-frame">
     <div class="kicker anim d1">Act I</div>
     <div class="h-hero anim d2">章节标题</div>
     <div class="lead anim d3" style="max-width:800px">章节引言</div>
   </div>
 </div>
 ```

 ## L03 三列卡片 (3-Column Cards)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>章节</div><div>页码</div></div>
   <div class="frame vstack" style="justify-content:center">
     <div>
       <div class="kicker anim d1">小标题</div>
       <div class="h-xl anim d2">大标题带 <span class="em">强调</span></div>
       <div class="lead anim d3" style="max-width:900px">说明文字</div>
     </div>
     <div class="grid-3 anim d4">
       <div class="b-card"><div class="icon-box">✦</div><h3>卡片标题</h3><p>内容</p></div>
       <div class="b-card teal"><div class="icon-box teal">◎</div><h3>卡片标题</h3><p>内容</p></div>
       <div class="b-card violet"><div class="icon-box violet">◈</div><h3>卡片标题</h3><p>内容</p></div>
     </div>
   </div>
   <div class="foot"><div>左</div><div>右</div></div>
 </div>
 ```

 ## L04 数据大字报 (Big Numbers)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>数据</div><div>页码</div></div>
   <div class="frame vstack" style="justify-content:center">
     <div>
       <div class="kicker anim d1">Metrics</div>
       <div class="h-xl anim d2">关键数据</div>
     </div>
     <div class="grid-6 anim d3">
       <div class="stat-card"><div class="stat-label">Label</div><div class="stat-nb count-up" data-value="64">0</div><div class="stat-note">注释</div></div>
       <div class="stat-card"><div class="stat-label">Label</div><div class="stat-nb count-up" data-value="110" data-suffix="K+">0</div><div class="stat-note">注释</div></div>
       <div class="stat-card"><div class="stat-label">Label</div><div class="stat-nb count-up" data-value="5166" data-suffix="">0</div><div class="stat-note">注释</div></div>
       <div class="stat-card"><div class="stat-label">Label</div><div class="stat-nb count-up" data-value="41" data-suffix="K+">0</div><div class="stat-note">注释</div></div>
       <div class="stat-card"><div class="stat-label">Label</div><div class="stat-nb count-up" data-value="19">0</div><div class="stat-note">注释</div></div>
       <div class="stat-card"><div class="stat-label">Label</div><div class="stat-nb count-up" data-value="608" data-suffix="+">0</div><div class="stat-note">注释</div></div>
     </div>
   </div>
 </div>
 ```

 ## L05 左文右图 (Text + Image)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>章节</div><div>页码</div></div>
   <div class="frame grid-2-7-5" style="align-items:center">
     <div class="vstack">
       <div class="kicker anim d1">Kicker</div>
       <div class="h-xl anim d2">标题</div>
       <div class="lead anim d3">正文说明</div>
       <div class="quote-block anim d4">
         <div class="quote-text">引用内容</div>
         <div class="quote-src">— 来源</div>
       </div>
     </div>
     <!-- T2 Float 自由留白（默认范式,无圆角无阴影,无"产品截图"图注） -->
     <figure class="t-float r-16x10 anim-right d3" data-treatment="t-float">
       <img src="images/xxx.png" alt="描述">
     </figure>
   </div>
 </div>
 ```

 ## L06 图片网格 (Image Grid)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>Gallery</div><div>页码</div></div>
   <div class="frame vstack" style="justify-content:center">
     <div>
       <div class="kicker anim d1">Proof</div>
       <div class="h-xl anim d2">图片网格标题</div>
     </div>
     <div class="t-quiet-grid anim d3" data-treatment="t-quiet">
       <!-- T6 Quiet Frame 多图分组,单图无 figcaption -->
       <figure class="t-quiet" style="height:24vh"><img src="images/1.png" alt="图1"></figure>
       <figure class="t-quiet" style="height:24vh"><img src="images/2.png" alt="图2"></figure>
       <figure class="t-quiet" style="height:24vh"><img src="images/3.png" alt="图3"></figure>
       <figure class="t-quiet" style="height:24vh"><img src="images/4.png" alt="图4"></figure>
       <figure class="t-quiet" style="height:24vh"><img src="images/5.png" alt="图5"></figure>
       <figure class="t-quiet" style="height:24vh"><img src="images/6.png" alt="图6"></figure>
     </div>
   </div>
 </div>
 ```

 ## L07 流水线 (Pipeline)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>Workflow</div><div>页码</div></div>
   <div class="frame vstack" style="justify-content:center">
     <div>
       <div class="kicker anim d1">Pipeline</div>
       <div class="h-xl anim d2">流程标题</div>
     </div>
     <div class="pipeline anim d3">
       <div class="pipeline-step"><div class="step-nb">01</div><h3>步骤一</h3><p>说明</p></div>
       <div class="pipeline-step"><div class="step-nb">02</div><h3>步骤二</h3><p>说明</p></div>
       <div class="pipeline-step"><div class="step-nb">03</div><h3>步骤三</h3><p>说明</p></div>
       <div class="pipeline-step"><div class="step-nb">04</div><h3>步骤四</h3><p>说明</p></div>
     </div>
   </div>
 </div>
 ```

 ## L08 智能图表 (Smart Charts)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>Data</div><div>页码</div></div>
   <div class="frame vstack" style="justify-content:center">
     <div>
       <div class="kicker anim d1">Insights</div>
       <div class="h-xl anim d2">数据洞察</div>
     </div>
     <div class="grid-2 anim d3" style="align-items:stretch;height:520px">
       <div class="smart-chart" data-chart='{"type":"donut","data":[{"name":"Direct","value":35},{"name":"Social","value":45},{"name":"Organic","value":20}]}'></div>
       <div class="smart-chart" data-chart='{"type":"bar","labels":["Mon","Tue","Wed","Thu","Fri"],"series":[{"name":"Visits","data":[120,190,150,220,280]}]}'></div>
     </div>
   </div>
 </div>
 ```

 ## L09 对比页 (Comparison)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>Compare</div><div>页码</div></div>
   <div class="frame center">
     <div class="kicker anim d1">Before vs After</div>
     <div class="h-xl anim d2" style="margin-bottom:var(--space-8)">对比标题</div>
     <div class="compare anim d3">
       <div class="compare-card">
         <div class="label">Before</div>
         <div class="value">3x</div>
         <p class="caption">旧方案</p>
       </div>
       <div class="compare-vs anim-scale d4">VS</div>
       <div class="compare-card">
         <div class="label">After</div>
         <div class="value" style="color:var(--b-accent)">1x</div>
         <p class="caption">新方案</p>
       </div>
     </div>
   </div>
 </div>
 ```

 ## L10 大引用 (Big Quote)

 ```html
 <div class="slide dark" id="sN">
   <div class="chrome"><div>Quote</div><div>页码</div></div>
   <div class="frame center">
     <div class="quote-block anim d1" style="max-width:1100px">
       <div class="quote-text">一句话金句，放在页面中央，字号最大。</div>
       <div class="quote-src">— 作者</div>
     </div>
   </div>
 </div>
 ```

 ## L11 四列小卡 (4-Column)

 ```html
 <div class="slide light" id="sN">
   <div class="chrome"><div>Features</div><div>页码</div></div>
   <div class="frame vstack" style="justify-content:center">
     <div>
       <div class="kicker anim d1">Features</div>
       <div class="h-xl anim d2">四个特性</div>
     </div>
     <div class="grid-4 anim d3">
       <div class="b-card glass"><h3>特性 A</h3><p>说明</p></div>
       <div class="b-card glass"><h3>特性 B</h3><p>说明</p></div>
       <div class="b-card glass"><h3>特性 C</h3><p>说明</p></div>
       <div class="b-card glass"><h3>特性 D</h3><p>说明</p></div>
     </div>
   </div>
 </div>
 ```

 ## L12 收尾页 (Closing)

 ```html
 <div class="slide hero dark" id="sN">
   <div class="cover-frame">
     <div class="h-hero anim d1">Thank You</div>
     <div class="lead anim d2" style="max-width:800px">总结语或行动号召</div>
     <div class="cover-divider anim d3"></div>
     <div class="hstack anim d4" style="gap:var(--space-6)">
       <div class="tag accent">#hashtag</div>
       <div class="tag accent">@handle</div>
     </div>
   </div>
 </div>
 ```

 ## 动画类速查

 | 类名 | 效果 |
 |------|------|
 | `.anim` | 默认从下方弹簧入场 |
 | `.anim-left` | 从左侧弹簧入场 |
 | `.anim-right` | 从右侧弹簧入场 |
 | `.anim-scale` | 缩放弹簧入场 |
 | `.anim-float` | 持续上下漂浮 |
 | `.d1` ~ `.d8` | 延迟触发顺序 |

 ## 智能图表类型

 | type | 必需字段 |
 |------|---------|
 | `bar` | `labels`, `series[].name`, `series[].data` |
 | `line` | `labels`, `series[].name`, `series[].data` |
 | `donut` / `pie` | `data[].name`, `data[].value` |
 | `radar` | `indicators[].name`, `indicators[].max`, `series[].name`, `series[].data` |
