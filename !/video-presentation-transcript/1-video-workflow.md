# 视频 1：从氛围编程到代理工程 — Claude Code 工作流

**总时长：约 5 分钟**

---

## 开头 — 问题（0:00 – 0:45）

- "如果你刚开始使用 Claude Code，很可能你在做氛围编程（vibe coding）— 输入提示词，得到结果，重复。这能用，但你只使用了 Claude Code 能力的一小部分。"
- "这个仓库是一个精心策划的最佳实践集合，带你从氛围编程走向代理工程 — 在那里 Claude 不只是回应你，它为你运行工作流。"
- "在第一个视频中，我将介绍基础：**Commands、Agents 和 Skills** — 以及它们如何串联成可重复的工作流。"

---

## 第 1 部分 — 临时方式（0:45 – 2:00）

**演示：氛围编程方式**

- 打开一个全新的 Claude Code 终端
- 输入：*"迪拜的天气怎么样？把它写到一个输出文件中并为它创建一个 SVG 卡片。"*
- 展示结果 — 能用，但指出：
  - SVG 设计每次都不一样（随机颜色、布局、字体）
  - 你必须坐在那里看着它工作
  - 如果明天再运行一次，你会得到一个看起来完全不同的卡片
- **打开第二个终端，再次运行相同的提示**
  - 并排展示 SVG — 它们看起来不同
- "这就是氛围编程的问题。它能用一次。但它不可重复。这不是一个你可以信任的工作流。"

---

## 第 2 部分 — 工作流方式（2:00 – 3:15）

**演示：`/weather-orchestrator` 命令**

- "现在让我展示同一个任务，但以工作流的方式。"
- 输入：`/weather-orchestrator`
- 展示屏幕上发生的事情：
  1. 它**询问你**使用摄氏度还是华氏度（结构化的用户交互）
  2. 它**生成一个 weather-agent** 来获取温度（你会在终端中看到绿色的 Agent）
  3. 它**调用一个 Skill** 来创建 SVG 卡片
  4. 输出：`orchestration-workflow/weather.svg` + `orchestration-workflow/output.md`
- "再运行一次 — 同样的 SVG 布局，同样的文件结构，同样干净的结果。每次都是。"
- "你可以启动它然后走开。它自动运行。"

---

## 第 3 部分 — 工作原理：Command → Agent → Skill（3:15 – 4:30）

**解释三个构建块**

### Commands（`.claude/commands/`）

- "Command 是入口点 — 就像一个脚本。它是一个 Markdown 文件，告诉 Claude *按什么步骤执行*。"
- "我们的 `weather-orchestrator` 是指挥者。它问用户一个问题，调用一个 Agent，然后调用一个 Skill。"
- Commands 位于 `.claude/commands/` 中，并显示为 `/slash-commands`

### Agents（`.claude/agents/`）

- "Agent 是一个专门的工作者。我们的 `weather-agent` 只有一个任务：获取温度。"
- "它有一个**预加载的 Skill** 叫做 `weather-fetcher` — 这个 Skill 在启动时被注入到 Agent 的上下文中，所以它确切知道调用哪个 API 以及如何解析响应。"
- Agents 有自己的工具、模型和权限。它们是隔离的工作者。

### Skills（`.claude/skills/`）

- "Skill 是一组可复用的指令。把它想象成一个食谱。"
- "我们这里有两种 Skill 模式："
  - **Agent Skill**（预加载）：`weather-fetcher` 被内置到 Agent 中 — 它是领域知识
  - **调用型 Skill**：`weather-svg-creator` 通过 Skill 工具独立调用 — 它创建 SVG 卡片
- Skills 可以是背景知识或独立操作

### 流程图（可选在屏幕上展示）

/weather-orchestrator (命令)
    → 询问用户：摄氏度还是华氏度？
    → weather-agent (子智能体 + weather-fetcher 技能)
    → weather-svg-creator (技能)
    → 输出：weather.svg + output.md

---

## 第 4 部分 — 为什么重要 / 总结（4:30 – 5:00）

- "氛围编程和代理工程之间的区别是**结构**。"
  - 氛围编程：你输入，你祈祷，你得到一些东西。
  - 代理工程：你定义一次工作流，它每次以同样的方式运行。
- "Commands、Agents 和 Skills 是三个构建块。一旦你理解了这些，你就可以构建任何工作流。"
- "这个仓库有更多模式 — Hooks、多 Agent 团队、CLAUDE.md 配置 — 我们将在后续视频中介绍。"
- "仓库链接在描述中。Star 它，克隆它，开始构建你自己的工作流。"

---

## 快速参考

| 概念 | 位置 | 用途 |
|---------|----------|---------|
| Command | `.claude/commands/` | 入口点，编排，`/slash-command` |
| Agent | `.claude/agents/` | 拥有自己工具和模型的专门工作者 |
| Skill | `.claude/skills/` | 可复用的指令（预加载或调用） |
