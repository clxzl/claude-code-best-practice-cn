# Subagents 报告 — 变更日志历史

## 状态图例

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE (原因)` | 已采取操作并成功解决 |
| ❌ `INVALID (原因)` | 发现不正确、不适用或属于预期行为 |
| ✋ `ON HOLD (原因)` | 操作已推迟——等待外部依赖或用户决定 |

---

## [2026-02-28 03:22 PM PKT] Claude Code v2.1.63

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | Agent 表格 | 将 `workflow-changelog-claude-agents-frontmatter-agent` 添加到本仓库 Agent 表格 | ✅ COMPLETE (已添加，模型: opus，继承所有工具，无 skills/memory) |
| 2 | HIGH | Agent 表格 | 修复 presentation-curator 的 skills 列——为 skill 名称添加 `presentation/` 前缀 | ✅ COMPLETE (已更新为 presentation/vibe-to-agentic-framework 等) |
| 3 | MED | 字段文档 | 在 `color` 字段添加说明，表明该字段功能可用但未出现在官方 frontmatter 表格中 | ✅ COMPLETE (已在描述列添加非官方状态说明) |
| 4 | MED | 调用部分 | 扩展调用部分，增加 --agents CLI 标志、/agents 命令、claude agents CLI、agent 恢复功能 | ✅ COMPLETE (已添加包含 5 种方法的调用方法表格) |

---

## [2026-03-07 08:35 AM PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 断开的链接 | 修复 agent memory 链接指向 `reports/claude-agent-memory.md` | ✅ COMPLETE |
| 2 | HIGH | 行为变更 | 更新 `tools` 字段描述：`Task(agent_type)` → `Agent(agent_type)`（v2.1.63 重命名） | ✅ COMPLETE |
| 3 | HIGH | 行为变更 | 更新调用部分：Task 工具 → Agent 工具（v2.1.63 重命名） | ✅ COMPLETE (已更新标题、代码示例，并添加重命名说明) |
| 4 | HIGH | 示例更新 | 更新完整示例：`Task(monitor, rollback)` → `Agent(monitor, rollback)` | ✅ COMPLETE |
| 5 | HIGH | 内置 Agent | 将 `Bash` agent 添加到官方 Claude Agent 表格（模型: inherit，用途: 在独立上下文中执行终端命令） | ✅ COMPLETE (已添加到表格) |
| 6 | HIGH | Agent 表格 | 将 `workflow-concepts-agent` 添加到本仓库 Agent 表格（模型: opus，颜色: green） | ✅ COMPLETE |
| 7 | HIGH | Agent 表格 | 将 `workflow-claude-settings-agent` 添加到本仓库 Agent 表格（模型: opus，颜色: yellow） | ✅ COMPLETE |
| 8 | MED | 内置 Agent | 修复 `statusline-setup` 模型：`inherit` → `Sonnet` | ✅ COMPLETE |
| 9 | MED | 内置 Agent | 修复 `claude-code-guide` 模型：`inherit` → `Haiku` | ❌ NOT APPLICABLE (已从表格中移除) |
| 10 | MED | Agent 表格 | 修复 `weather-agent` 颜色：`teal` → `green` | ✅ COMPLETE |
| 11 | MED | 调用 | 将 `--agent <name>` CLI 标志添加到调用方法表格 | ✅ COMPLETE (已添加为调用方法表格的第一行) |
| 12 | MED | 行为变更 | 更新第 147 行文本：官方 Claude Agent 表格标题中的 "Task tool" → "Agent tool" | ✅ COMPLETE (用户已重写标题文本) |
| 13 | MED | 跨文件 | 更新 CLAUDE.md：`Task(...)` → `Agent(...)` 引用（第 50-53 行、61 行） | ✅ COMPLETE (已更新编排部分和 tools 字段描述) |

---

## [2026-03-12 12:17 PM PKT] Claude Code v2.1.74

未检测到偏差——报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置 agent 匹配。

---

## [2026-03-13 04:21 PM PKT] Claude Code v2.1.74

未检测到偏差——报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置 agent 匹配。

---

## [2026-03-15 12:50 PM PKT] Claude Code v2.1.76

未检测到偏差——报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置 agent 匹配。

---

## [2026-03-17 12:42 PM PKT] Claude Code v2.1.77

未检测到偏差——报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置 agent 匹配。

---

## [2026-03-18 11:41 PM PKT] Claude Code v2.1.78

未检测到偏差——报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置 agent 匹配。

---

## [2026-03-19 11:56 AM PKT] Claude Code v2.1.79

未检测到偏差——报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置 agent 匹配。

---

## [2026-03-20 08:35 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新字段 | 将 `effort` 字段添加到 Frontmatter 字段表格（string，可选——努力级别覆盖：`low`、`medium`、`high`、`max`） | ✅ COMPLETE (已添加在 `background` 和 `isolation` 之间，计数从 14 更新为 15) |

---

## [2026-03-21 09:07 PM PKT] Claude Code v2.1.81

未检测到偏差——报告与官方文档完全同步。所有 15 个 frontmatter 字段和 6 个内置 agent 匹配。

---

## [2026-03-23 09:49 PM PKT] Claude Code v2.1.81

未检测到偏差——报告与官方文档完全同步。所有 15 个 frontmatter 字段（14 个官方 + 1 个非官方 `color`）和 6 个内置 agent 匹配。

---

## [2026-03-25 08:07 PM PKT] Claude Code v2.1.83

未检测到偏差——报告与官方文档完全同步。所有 15 个 frontmatter 字段（14 个官方 + 1 个非官方 `color`）和 6 个内置 agent 匹配。

**关注事项：** `initialPrompt` 已在 v2.1.83 变更日志中添加，但尚未出现在官方文档支持的 frontmatter 字段表格中。一旦出现，报告将需要更新。

---

## [2026-03-26 01:01 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新字段 | 将 `initialPrompt` 添加到 Frontmatter 字段表格（string，可选——当 agent 通过 `--agent` 或 `agent` 设置作为主会话 agent 运行时，自动作为第一个用户回合提交） | ✅ COMPLETE (已添加在 `isolation` 和 `color` 之间，计数从 15 更新为 16) |

---

## [2026-03-27 06:28 PM PKT] Claude Code v2.1.85

未检测到偏差——报告与官方文档完全同步。所有 16 个 frontmatter 字段（15 个官方 + 1 个非官方 `color`）和 6 个内置 agent 匹配。
