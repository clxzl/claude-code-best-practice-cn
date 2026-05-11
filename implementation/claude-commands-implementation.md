# Commands 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-orchestrator"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

天气编排命令在本仓库中作为 **Command → Agent → Skill** 架构模式的入口点实现，演示了命令如何编排多步骤工作流。

---

## 天气编排器

**文件**: [`.claude/commands/weather-orchestrator.md`](../.claude/commands/weather-orchestrator.md)

```yaml
---
description: Fetch weather data for Dubai and create an SVG weather card
model: haiku
---

# Weather Orchestrator Command

Fetch the current temperature for Dubai, UAE and create a visual SVG weather card.

## Workflow

### Step 1: Ask User Preference
Use the AskUserQuestion tool to ask the user whether they want the temperature
in Celsius or Fahrenheit.

### 步骤2：获取天气数据  
使用 Agent 工具调用天气代理：  
- subagent_type: weather-agent  
- prompt: 获取阿联酋迪拜当前温度（单位为[unit]）...

### Step 3: Create SVG Weather Card
Use the Skill tool to invoke the weather-svg-creator skill:
- skill: weather-svg-creator

...
```

该命令编排整个工作流：询问用户的温度单位偏好，通过 Agent 工具调用 `weather-agent`，然后通过 Skill 工具调用 `weather-svg-creator` skill。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> /weather-orchestrator
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

让 Claude 为你创建一个——它会在 `.claude/commands/<name>.md` 中生成包含 YAML frontmatter 和正文的 markdown 文件。

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="编排工作流"></a>

天气编排器是 Command → Agent → Skill 编排模式中的 **Command**。它作为入口点——处理用户交互（温度单位偏好），将数据获取委托给 `weather-agent`，并调用 `weather-svg-creator` skill 生成视觉输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent 架构流程" width="100%">
</p>

| 组件 | 角色 | 本仓库实例 |
|-----------|------|-----------|
| **Command** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 使用预加载 skill 获取数据（Agent Skill） | [`weather-agent`](../.claude/agents/weather-agent.md) 搭配 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（Skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |
