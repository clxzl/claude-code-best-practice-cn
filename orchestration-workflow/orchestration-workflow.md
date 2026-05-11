# 编排工作流

本文档描述了 **Command → Agent（带 Skill）→ Skill** 的编排工作流，通过天气数据获取和 SVG 渲染系统进行演示。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 系统概览

天气系统在单个编排工作流中展示了两种不同的 Skill 模式：
- **Agent Skill**（预加载）：`weather-fetcher` 在启动时作为领域知识注入到 `weather-agent` 中
- **Skill**（独立调用）：`weather-svg-creator` 由 Command 通过 Skill 工具直接调用

这展示了 **Command → Agent → Skill** 的架构模式，其中：
- Command 编排工作流并处理用户交互
- Agent 使用其预加载的 Skill 获取数据
- Skill 独立创建视觉输出

## 组件概要

| 组件 | 角色 | 示例 |
|-----------|------|---------|
| **Command** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 使用预加载的 Skill 获取数据（Agent Skill） | [`weather-agent`](../.claude/agents/weather-agent.md) 配合 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（Skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |

## 流程图

```
╔══════════════════════════════════════════════════════════════════╗
║              编排工作流                                           ║
║           Command  →  Agent  →  Skill                            ║
╚══════════════════════════════════════════════════════════════════╝

                         ┌───────────────────┐
                         │  用户交互          │
                         └─────────┬─────────┘
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  /weather-orchestrator — Command（入口点）            │
         └─────────────────────────┬───────────────────────────┘
                                   │
                              步骤 1
                                   │
                                   ▼
                      ┌────────────────────────┐
                      │  AskUser — 摄氏度还是华氏度？ │
                      └────────────┬───────────┘
                                   │
                         步骤 2 — Agent 工具
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-agent — Agent ● skill: weather-fetcher     │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          返回：温度 + 单位
                                   │
                         步骤 3 — Skill 工具
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-svg-creator — Skill ● SVG 卡片 + 输出      │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          ┌────────┴────────┐
                          │                 │
                          ▼                 ▼
                   ┌────────────┐    ┌────────────┐
                   │weather.svg │    │ output.md  │
                   └────────────┘    └────────────┘
```

## 组件详情

### 1. Command

#### `/weather-orchestrator`（Command）
- **位置**：`.claude/commands/weather-orchestrator.md`
- **用途**：入口点——编排工作流并处理用户交互
- **操作**：
  1. 询问用户温度单位偏好（摄氏度/华氏度）
  2. 通过 Agent 工具调用 weather-agent
  3. 通过 Skill 工具调用 weather-svg-creator
- **模型**：haiku

### 2. 带预加载 Skill 的 Agent（Agent Skill）

#### `weather-agent`（Agent）
- **位置**：`.claude/agents/weather-agent.md`
- **用途**：使用其预加载的 Skill 获取天气数据
- **Skill**：`weather-fetcher`（作为领域知识预加载）
- **可用工具**：WebFetch, Read
- **模型**：sonnet
- **颜色**：green
- **记忆**：project

该 Agent 在启动时将 `weather-fetcher` 预加载到其上下文中。它遵循 Skill 的指令获取温度并将值返回给 Command。

### 3. Skill

#### `weather-svg-creator`（Skill）
- **位置**：`.claude/skills/weather-svg-creator/SKILL.md`
- **用途**：创建视觉 SVG 天气卡片并写入输出文件
- **调用方式**：通过 Command 的 Skill 工具调用（未预加载到任何 Agent 中）
- **输出**：
  - `orchestration-workflow/weather.svg` — SVG 天气卡片
  - `orchestration-workflow/output.md` — 天气摘要

### 4. 预加载 Skill

#### `weather-fetcher`（Skill）
- **位置**：`.claude/skills/weather-fetcher/SKILL.md`
- **用途**：获取实时温度数据的指令
- **数据来源**：Open-Meteo API（迪拜，阿联酋）
- **输出**：温度值和单位（摄氏度或华氏度）
- **注意**：这是一个 Agent Skill——预加载到 `weather-agent` 中，不直接调用

## 执行流程

1. **用户调用**：用户运行 `/weather-orchestrator` 命令
2. **用户提示**：Command 询问用户首选温度单位（摄氏度/华氏度）
3. **Agent 调用**：Command 通过 Agent 工具调用 `weather-agent`
4. **Skill 执行**（在 Agent 上下文中）：
   - Agent 遵循 `weather-fetcher` Skill 指令从 Open-Meteo 获取温度
   - Agent 将温度值和单位返回给 Command
5. **SVG 创建**：Command 通过 Skill 工具调用 `weather-svg-creator`
   - Skill 在 `orchestration-workflow/weather.svg` 创建 SVG 天气卡片
   - Skill 将摘要写入 `orchestration-workflow/output.md`
6. **结果展示**：向用户显示摘要，包括：
   - 请求的温度单位
   - 获取的温度
   - SVG 卡片位置
   - 输出文件位置

## 示例执行

```
输入：/weather-orchestrator
├─ 步骤 1：询问：摄氏度还是华氏度？
│  └─ 用户：摄氏度
├─ 步骤 2：Agent 工具 → weather-agent
│  ├─ 预加载的 Skill：
│  │  └─ weather-fetcher（领域知识）
│  ├─ 从 Open-Meteo 获取 → 26°C
│  └─ 返回：temperature=26, unit=Celsius
├─ 步骤 3：Skill 工具 → /weather-svg-creator
│  ├─ 创建：orchestration-workflow/weather.svg
│  └─ 写入：orchestration-workflow/output.md
└─ 输出：
   ├─ 单位：摄氏度
   ├─ 温度：26°C
   ├─ SVG：orchestration-workflow/weather.svg
   └─ 摘要：orchestration-workflow/output.md
```

## 关键设计原则

1. **两种 Skill 模式**：展示了 Agent Skill（预加载）和 Skill（直接调用）两种模式
2. **Command 作为编排器**：Command 处理用户交互并协调工作流
3. **Agent 负责数据获取**：Agent 使用其预加载的 Skill 获取数据，然后返回
4. **Skill 负责输出**：SVG 创建器独立运行，从 Command 上下文接收数据
5. **清晰分离**：获取（Agent）→ 渲染（Skill）——每个组件职责单一

## 架构模式

### Agent Skill（预加载）

```yaml
# 在 Agent 定义中（.claude/agents/weather-agent.md）
---
name: weather-agent
skills:
  - weather-fetcher    # 在启动时预加载到 Agent 上下文中
---
```

- **Skill 被预加载**：完整的 Skill 内容在启动时注入到 Agent 的上下文中
- **Agent 使用 Skill 知识**：Agent 遵循预加载 Skill 中的指令
- **无动态调用**：Skill 是参考资料，不单独调用

### Skill（直接调用）

```yaml
# 在 Skill 定义中（.claude/skills/weather-svg-creator/SKILL.md）
---
name: weather-svg-creator
description: Creates an SVG weather card...
---
```

- **通过 Skill 工具调用**：Command 调用 `Skill(skill: "weather-svg-creator")`
- **独立执行**：在 Command 的上下文中运行，不在 Agent 内部
- **从上下文接收数据**：使用对话中已有的温度数据
