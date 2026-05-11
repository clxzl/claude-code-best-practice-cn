# Agent Teams 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_12%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#time-orchestration"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

<p align="center">
  <img src="assets/impl-agent-teams.png" alt="Agent Teams 运行效果 — 使用 tmux 分屏模式" width="100%">
</p>

Agent Teams 会生成**多个独立的 Claude Code 会话**，通过共享任务列表进行协调。与 Subagents（单个会话内的隔离上下文分支）不同，每个队友都拥有自己的完整上下文窗口，自动加载 CLAUDE.md、MCP 服务器和 Skills。

---

## ![如何使用](../!/tags/how-to-use.svg)

时间编排工作流完全由 Agent Teams 构建。运行成品：

```bash
cd agent-teams
claude
/time-orchestrator
```

这将调用 **Command → Agent → Skill** 管道：Agent 获取迪拜当前时间，Skill 将时间卡片渲染为 SVG 并输出到 `agent-teams/output/dubai-time.svg`。

---

## ![如何实现](../!/tags/how-to-implement.svg)

你可以使用 Agent Teams 创建天气编排工作流的复制品——在这个示例中，时间编排工作流完全由 Agent Teams 构建。

### 1. 安装 [iTerm2](https://iterm2.com/) 和 tmux

```bash
brew install --cask iterm2
brew install tmux
```

### 2. 启动 iTerm2 → tmux → Claude

```bash
tmux new -s dev
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 claude
```

### 3. 使用团队结构提示词

<a id="time-orchestration"></a>

将以下提示词粘贴到 Claude 中，使用 Agent Teams 引导构建完整的时间编排工作流：

主提示词：**[agent-teams-prompt.md](../agent-teams/agent-teams-prompt.md)**

### 团队协调流程

```
┌──────────────────────────────────────────────────────────────┐
│                         LEAD (你)                             │
│       "创建一个 Agent Team 来构建时间编排"                     │
└──────────────────────────┬───────────────────────────────────┘
                           │ 生成团队（全部并行）
              ┌────────────┼────────────┐
              ▼            ▼            ▼
   ┌────────────────┐ ┌──────────┐ ┌──────────────┐
   │ Command        │ │ Agent    │ │ Skill        │
   │ 架构师         │ │ 工程师   │ │ 设计师       │
   │                │ │          │ │              │
   │ agent-teams/   │ │ agent-   │ │ agent-teams/ │
   │ .claude/       │ │ teams/   │ │ .claude/     │
   │ commands/      │ │ .claude/ │ │ skills/      │
   │ time-          │ │ agents/  │ │ time-svg-    │
   │ orchestrator.md│ │ time-    │ │ creator/     │
   │                │ │ agent.md │ │              │
   └───────┬────────┘ └────┬─────┘ └──────┬───────┘
           │               │              │
           ▼               ▼              ▼
   ┌──────────────────────────────────────────────────┐
   │            共享任务列表                           │
   │  ☐ 达成数据契约：{time, tz, formatted}           │
   │  ☐ Command 使用 Agent 工具（非 bash）            │
   │  ☐ Agent 预加载 time-fetcher skill               │
   │  ☐ Skill 从上下文中读取时间（不重复获取）         │
   │  ☐ 所有文件位于 agent-teams/.claude/ 目录下      │
   └──────────────────────────────────────────────────┘
                       │
                       ▼
          ┌──────────────────────────────┐
          │  cd agent-teams && claude    │
          │    /time-orchestrator        │
          │   Command → Agent → Skill    │
          └──────────────────────────────┘
```
