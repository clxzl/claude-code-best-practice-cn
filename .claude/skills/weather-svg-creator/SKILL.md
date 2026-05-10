---
name: weather-svg-creator
description: 创建一个 SVG 天气卡片，显示迪拜的当前温度。将 SVG 写入 orchestration-workflow/weather.svg 并更新 orchestration-workflow/output.md。
---

# 天气 SVG 创建器 Skill

为阿联酋迪拜创建一个可视化的 SVG 天气卡片并写入输出文件。

## 任务

你将从调用上下文中接收一个温度值和单位（摄氏度或华氏度）。创建一个 SVG 天气卡片并写入 SVG 和 Markdown 摘要。

## 指令

1. **创建 SVG** — 使用 [reference.md](reference.md) 中的 SVG 模板，用实际值替换占位符
2. **写入 SVG 文件** — 读取后写入 `orchestration-workflow/weather.svg`
3. **写入摘要** — 读取后使用 [reference.md](reference.md) 中的 Markdown 模板写入 `orchestration-workflow/output.md`

## 规则

- 使用提供的精确温度值和单位 — 不要重新获取或修改
- SVG 必须是自包含且有效的
- 两个输出文件都放在 `orchestration-workflow/` 目录中

## 附加资源

- SVG 模板、输出模板和设计规范，请参阅 [reference.md](reference.md)
- 示例输入/输出配对，请参阅 [examples.md](examples.md)
