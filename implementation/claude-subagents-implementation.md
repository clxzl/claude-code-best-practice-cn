# Sub-agents 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026_07%3A59_PM_PKT-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-agent"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

天气 Agent 在本仓库中作为 **Command → Agent → Skill** 架构模式的示例实现，演示了两种不同的 skill 模式。

---

## 天气 Agent

**文件**: [`.claude/agents/weather-agent.md`](../.claude/agents/weather-agent.md)

```yaml
---
name: weather-agent
description: 当你需要获取阿联酋迪拜的天气数据时，请主动使用此代理。此代理通过其预加载的weather-fetcher技能，从Open-Meteo获取实时温度。
tools: WebFetch, Read, Write, Edit
model: sonnet
color: green
maxTurns: 5
permissionMode: acceptEdits
memory: project
skills:
  - weather-fetcher
---
```

# Weather Agent

You are a specialized weather agent that fetches weather data for Dubai,
UAE.

## Your Task

Execute the weather workflow by following the instructions from your preloaded
skill:

1. **获取**：遵循 `weather-fetcher` skill 指令，获取当前温度
2. **报告**：将温度数值及单位返回给调用者
3. **记忆**：更新您的 agent 记忆，记录读数详情以供历史跟踪

...
```

该 Agent 有一个预加载的 skill（`weather-fetcher`），提供从 Open-Meteo 获取数据的指令。它将温度值和单位返回给调用的命令。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> what is the weather in dubai?
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

你可以使用 `/agents` 命令创建 Agent，
```bash
$ claude
> /agents
```

或者让 Claude 为你创建一个——它会在 `.claude/agents/<name>.md` 中生成包含 YAML frontmatter 和正文的 markdown 文件。

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="编排工作流 (Orchestration Workflow)"></a>

天气 Agent 是 Command → Agent → Skill 编排模式中的 **Agent**。它接收来自 `/weather-orchestrator` 命令的工作流，并使用其预加载的 skill（`weather-fetcher`）获取温度。然后命令调用独立的 `weather-svg-creator` skill 创建视觉输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent 架构流程" width="100%">
</p>

| 组件 | 角色 | 本仓库实例 |
|-----------|------|-----------|
| **Command** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 使用预加载 skill 获取数据（Agent Skill） | [`weather-agent`](../.claude/agents/weather-agent.md) 搭配 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（Skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |
