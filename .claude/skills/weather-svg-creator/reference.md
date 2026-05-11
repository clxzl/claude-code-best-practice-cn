# 天气 SVG 创建器 — 参考

## SVG 模板

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 300 160" width="300" height="160">
  <rect width="300" height="160" rx="12" fill="#1a1a2e"/>
  <text x="150" y="45" text-anchor="middle" fill="#8892b0" font-family="system-ui" font-size="14">单位: [摄氏度/华氏度]</text>
  <text x="150" y="100" text-anchor="middle" fill="#ccd6f6" font-family="system-ui" font-size="42" font-weight="bold">[value]°[C/F]</text>
  <text x="150" y="140" text-anchor="middle" fill="#64ffda" font-family="system-ui" font-size="16">阿联酋迪拜</text>
</svg>
```

### 占位符

| 占位符 | 替换为 | 示例 |
|-------------|-------------|---------|
| `[Celsius/Fahrenheit]` | 输入中的完整单位名称 | `Celsius` |
| `[value]` | 输入中的数值温度 | `26.2` |
| `[C/F]` | 单位缩写 | `C` 或 `F` |

### 设计规格

| 属性 | 值 |
|----------|-------|
| 尺寸 | 300 x 160 px |
| 圆角半径 | 12 px |
| 背景 | `#1a1a2e`（深海军蓝） |
| 单位标签 | `#8892b0`（柔和蓝色），14px |
| 温度 | `#ccd6f6`（浅蓝色），42px 粗体 |
| 位置 | `#64ffda`（青绿色点缀），16px |
| 字体 | `system-ui` |
| 所有文本 | 居中（`text-anchor="middle"` x=150） |

---

## 输出 Markdown 模板

```markdown
# 天气结果

## 温度
[value]°[C/F]

## 位置
Dubai, UAE

## 单位
[Celsius/Fahrenheit]

## SVG 卡片
![Weather Card](weather.svg)
```

---

## 输出路径

| 文件 | 路径 |
|------|------|
| SVG 卡片 | `orchestration-workflow/weather.svg` |
| Markdown 摘要 | `orchestration-workflow/output.md` |
