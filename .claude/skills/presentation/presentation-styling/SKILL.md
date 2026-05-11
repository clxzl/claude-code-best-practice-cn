---
name: presentation-styling
description: 关于演示文稿中 CSS 类、组件模式和语法高亮的知识
---

# 演示文稿样式 Skill

`presentation/index.html` 中使用的 CSS 类和 HTML 模式。

## CSS 组件类

### 布局

- `.two-col` — 2 列网格布局，间距 24px
- `.info-grid` — 信息卡片的 2 列网格
- `.col-card` — 列中的卡片（添加 `.good` 为绿色边框，`.bad` 为红色边框）
- `.info-card` — 信息网格中的卡片

### 内容块

- `.trigger-box` — 灰色盒子带深色左边框（用于关键概念、前置条件）
- `.how-to-trigger` — 绿色盒子带绿色边框（用于"尝试此操作"动作）
- `.warning-box` — 橙色盒子带警告边框（用于重要警告）
- `.code-block` — 深色代码显示块，使用等宽字体

### 列表

- `.use-cases` — 图标+文本列表项的容器
- `.use-case-item` — 带图标和文本的单个项目
- `.feature-list` — 简单的带边框列表

### 标签和徽章

- `.matcher-tag` — 灰色内联药丸标签
- `.weight-badge` — 绿色药丸徽章（由 JS 为带权重的幻灯片自动注入）

## 代码块语法高亮

在 `.code-block` 内，使用以下 span 进行语法着色：

```html
<div class="code-block">
<span class="comment"># 这是一条注释</span>
<span class="key">field_name</span>: <span class="string">value</span>
<span class="cmd">&gt;</span> 要运行的命令
</div>
```

- `.comment` — 绿色 (#6a9955) 用于注释
- `.key` — 蓝色 (#9cdcfe) 用于属性名/键名
- `.string` — 橙色 (#ce9178) 用于字符串值
- `.cmd` — 黄色 (#dcdcaa) 用于命令/提示符

## 幻灯片类型模式

### 带双栏的内容幻灯片（好与坏对比）
```html
<div class="slide" data-slide="N" data-weight="5">
    <h1>Title</h1>
    <div class="two-col">
        <div class="col-card bad">
            <h4>Before (Vibe Coding)</h4>
            <!-- bad example -->
        </div>
        <div class="col-card good">
            <h4>After (Agentic)</h4>
            <!-- good example -->
        </div>
    </div>
</div>
```

不要在幻灯片 HTML 中硬编码 `<span class="weight-badge">`。演示文稿 JavaScript 会自动注入和移除权重徽章。

### 带代码示例的内容幻灯片
```html
<div class="slide" data-slide="N">
    <h1>Title</h1>
    <div class="trigger-box">
        <h4>Key Concept</h4>
        <p>Description</p>
    </div>
    <div class="code-block"><span class="comment"># Example</span>
<span class="key">field</span>: <span class="string">value</span></div>
</div>
```

### 图标列表模式
```html
<div class="use-cases">
    <div class="use-case-item">
        <span class="use-case-icon">EMOJI</span>
        <div class="use-case-text">
            <strong>标题</strong>
            <span>描述文本</span>
        </div>
    </div>
</div>
```

## 旅程进度条特定

- `.journey-bar` — 进度条下方的固定条
- `.journey-bar.hidden` — 在标题幻灯片上隐藏
- 旅程进度条颜色通过 HSL 插值从红色 (0%) 过渡到绿色 (100%)
- 权重徽章由 JS 自动注入到带权重幻灯片的 `h1` 元素中
