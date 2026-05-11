# 天气 SVG 创建器 — 示例

## 示例 1：摄氏度

### 输入

```
Temperature: 26.2°C
Unit: Celsius
```

### SVG 输出 (`orchestration-workflow/weather.svg`)

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 300 160" width="300" height="160">
  <rect width="300" height="160" rx="12" fill="#1a1a2e"/>
  <text x="150" y="45" text-anchor="middle" fill="#8892b0" font-family="system-ui" font-size="14">Unit: Celsius</text>
  <text x="150" y="100" text-anchor="middle" fill="#ccd6f6" font-family="system-ui" font-size="42" font-weight="bold">26.2°C</text>
  <text x="150" y="140" text-anchor="middle" fill="#64ffda" font-family="system-ui" font-size="16">Dubai, UAE</text>
</svg>
```

### Markdown 输出 (`orchestration-workflow/output.md`)

```markdown
# 天气结果

## 温度
26.2°C

## 位置
Dubai, UAE

## 单位
Celsius

## SVG 卡片
![Weather Card](weather.svg)
```

---

## 示例 2：华氏度

### 输入

```
Temperature: 79.2°F
Unit: Fahrenheit
```

### SVG 输出 (`orchestration-workflow/weather.svg`)

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 300 160" width="300" height="160">
  <rect width="300" height="160" rx="12" fill="#1a1a2e"/>
  <text x="150" y="45" text-anchor="middle" fill="#8892b0" font-family="system-ui" font-size="14">单位：华氏度</text>
  <text x="150" y="100" text-anchor="middle" fill="#ccd6f6" font-family="system-ui" font-size="42" font-weight="bold">79.2°F</text>
  <text x="150" y="140" text-anchor="middle" fill="#64ffda" font-family="system-ui" font-size="16">阿联酋迪拜</text>
</svg>
```

### Markdown 输出 (`orchestration-workflow/output.md`)

```markdown
# 天气结果

## 温度
79.2°F

## 位置
Dubai, UAE

## 单位
Fahrenheit

## SVG 卡片
![Weather Card](weather.svg)
```
