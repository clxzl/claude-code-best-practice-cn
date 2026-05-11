---
name: presentation-structure
description: 关于演示文稿幻灯片格式、权重系统、导航和章节结构的知识
---

# 演示文稿结构 Skill

关于 `presentation/index.html` 中演示文稿结构的知识。

## 文件位置

`presentation/index.html` — 一个单文件 HTML 演示文稿，包含内联 CSS 和 JS。

## 幻灯片格式

每张幻灯片是一个带有 `data-slide`（顺序编号）和可选 `data-level`（过渡点的旅程级别）的 div：

```html
<!-- 普通幻灯片 — 继承上一个 data-level 幻灯片的级别 -->
<div class="slide" data-slide="12">
    <h1>Slide Title</h1>
    <!-- content -->
</div>

<!-- 级别过渡幻灯片 — 为此幻灯片及后续所有幻灯片设置新级别 -->
<div class="slide section-slide" data-slide="10" data-level="low">
    <h1>Section Name</h1>
    <p class="section-desc">Level: Low — description of this section</p>
</div>

<!-- 标题幻灯片（居中） -->
<div class="slide title-slide" data-slide="1">
    <h1>Presentation Title</h1>
    <p class="subtitle">Subtitle text</p>
</div>
```

## 旅程进度条级别系统

演示文稿使用 4 级系统而非累积百分比：

- 级别通过关键过渡幻灯片（章节分隔符）上的 `data-level` 属性设置
- `data-level` 幻灯片之后的所有幻灯片都继承该级别，直到下一个过渡
- 进度条分别填充至 25% / 50% / 75% / 100%，对应 Low / Medium / High / Pro
- 第 1 张幻灯片（标题幻灯片）上进度条隐藏；从第 2 张开始显示
- 第一个 `data-level` 之前的幻灯片（第 2-9 张）显示空进度条（尚未设置级别）
- `.level-badge` 由 JS 在运行时注入到带有 `data-level` 的幻灯片的 `<h1>` 上 — 不要在 HTML 中硬编码

### 各章节的级别过渡

| 章节 | 幻灯片范围 | data-level | 进度条高度 |
|------|-----------|------------|-----------|
| 第 0 部分：介绍 | 幻灯片 1-4 | （无） | 隐藏 / 空 |
| 第 1 部分：前置条件 | 幻灯片 5-9 | （无） | 空 |
| 第 2 部分：更好的提示 | 幻灯片 10-17 | `low` | 25% |
| 第 3 部分：项目记忆 | 幻灯片 18-24 | `medium` | 50% |
| 第 4 部分：结构化工作流 | 幻灯片 25-28 | （继承 medium） | 50% |
| 第 5 部分：领域知识 | 幻灯片 29-33 | `high` | 75% |
| 第 6 部分：Agent 工程 | 幻灯片 34-46 | `high` | 75% |
| 附录 | 幻灯片 47+ | （继承 high） | 75% |

## 导航系统

- `goToSlide(n)` — 用于目录链接，必须与实际的 `data-slide` 编号匹配
- `totalSlides` 从 DOM 自动计算（`document.querySelectorAll('[data-slide]').length`）
- 方向键、空格键和触摸滑动用于导航
- 幻灯片计数器在左下角显示 `current / total`

## 重新编号规则

添加、删除或重新排列幻灯片后：
1. 将所有 `data-slide` 属性从 1 开始顺序重新编号
2. 更新目录/旅程地图幻灯片中所有的 `goToSlide()` 调用
3. JS 的 `totalSlides` 会自动计算 — 无需手动更新
4. 验证不存在间隔或重复

## 章节分隔符格式

章节分隔符使用 `section-slide` 类。级别过渡的章节分隔符带有 `data-level` 并在描述中显示级别名称：

```html
<div class="slide section-slide" data-slide="10" data-level="low">
    <p class="section-number">第二部分</p>
    <h1>更优提示</h1>
    <p class="section-desc">级别：低——为实现真实效果的有效提示方法。</p>
</div>
```

JS 会在级别过渡时在运行时将 `.level-badge`（例如 "→ Low"）注入到 `<h1>` 中 — 不要在 HTML 中手动添加。
