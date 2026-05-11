# Skills 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-svg-creator"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

本仓库实现了两个 skills，作为 **Command → Agent → Skill** 架构模式的一部分，演示了两种不同的 skill 调用模式：**Agent Skills**（预加载）和 **Skills**（直接调用）。

---

## 天气 SVG 创建器 (Skill)

**文件**: [`.claude/skills/weather-svg-creator/SKILL.md`](../.claude/skills/weather-svg-creator/SKILL.md)

```yaml
---
name: weather-svg-creator
description: 创建一个显示迪拜当前温度的SVG天气卡片。将SVG写入orchestration-workflow/weather.svg并更新orchestration-workflow/output.md。
---
```

# Weather SVG Creator Skill

This skill creates a visual SVG weather card and writes the output files.

## Task
Create an SVG weather card displaying the temperature for Dubai, UAE,
and write it along with a summary to output files.

## Instructions
You will receive the temperature value and unit (Celsius or Fahrenheit)
from the calling context.

### 1. Create SVG Weather Card
Generate a clean SVG weather card...

### 2. Write SVG File
Write the SVG content to `orchestration-workflow/weather.svg`.

### 3. Write Output Summary
Write to `orchestration-workflow/output.md`...

...
```

这是一个 **Skill**——由命令通过 Skill 工具直接调用。它从对话上下文中接收温度数据，创建 SVG 天气卡片和输出摘要。

---

## 天气获取器 (Agent Skill)

**文件**: [`.claude/skills/weather-fetcher/SKILL.md`](../.claude/skills/weather-fetcher/SKILL.md)

```yaml
---
name: weather-fetcher
description: 从 Open-Meteo API 获取阿联酋迪拜当前天气温度数据的说明
user-invocable: false
---
```

# Weather Fetcher Skill

This skill provides instructions for fetching current weather data.

## Task
Fetch the current temperature for Dubai, UAE in the requested unit
(Celsius or Fahrenheit).

## 操作指南
1. 获取天气数据：使用 WebFetch 工具获取当前天气数据
   - 摄氏温度 URL: https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=celsius
   - 华氏温度 URL: https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=fahrenheit
2. 提取温度值：从 JSON 响应中提取 `current.temperature_2m`
3. 返回结果：清晰返回温度值及其单位

...
```

这是一个 **Agent Skill**——在启动时通过 `skills:` frontmatter 字段预加载到 `weather-agent` 中。它不会被直接调用，而是作为领域知识注入到 Agent 的上下文中。注意 `user-invocable: false` 将其从 `/` 命令菜单中隐藏。

---

## 两种 Skill 模式

| 模式 | 调用方式 | 示例 | 关键区别 |
|---------|-----------|---------|----------------|
| **Skill** | `Skill(skill: "name")` | `weather-svg-creator` | 通过 Skill 工具直接调用 |
| **Agent Skill** | 通过 `skills:` 字段预加载 | `weather-fetcher` | 在启动时注入 Agent 上下文 |

---

## ![如何使用](../!/tags/how-to-use.svg)

**Skill** — 通过斜杠命令直接调用：
```bash
$ claude
> /weather-svg-creator
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

让 Claude 为你创建一个——它会在 `.claude/skills/my-skill/SKILL.md` 中生成包含 YAML frontmatter 和正文的 markdown 文件。

# My Skill

Instructions for what the skill does.
```
