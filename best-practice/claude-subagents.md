# Subagents 最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2027%2C%202026%206%3A28%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-subagents-implementation.md)

Claude Code 子代理（subagents）—— frontmatter 字段和官方内置代理类型。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段 (16)

| 字段 | 类型 | 必需 | 描述 |
|-------|------|----------|-------------|
| `name` | string | 是 | 使用小写字母和连字符的唯一标识符 |
| `description` | string | 是 | 何时调用。使用 `"PROACTIVELY"` 可由 Claude 自动调用 |
| `tools` | string/list | 否 | 逗号分隔的工具白名单（如 `Read, Write, Edit, Bash`）。如省略则继承所有工具。支持 `Agent(agent_type)` 语法限制可生成的子代理；旧版 `Task(agent_type)` 别名仍然有效 |
| `disallowedTools` | string/list | 否 | 要拒绝的工具，从继承或指定的列表中移除 |
| `model` | string | 否 | 模型别名：`haiku`、`sonnet`、`opus` 或 `inherit`（默认：`inherit`） |
| `permissionMode` | string | 否 | 权限模式：`default`、`acceptEdits`、`dontAsk`、`bypassPermissions` 或 `plan` |
| `maxTurns` | integer | 否 | 子代理停止前最大代理轮数 |
| `skills` | list | 否 | 启动时预加载到代理上下文的技能名称（注入完整内容，而非仅使其可用） |
| `mcpServers` | list | 否 | 此子代理的 MCP 服务器 —— 服务器名称字符串或内联 `{name: config}` 对象 |
| `hooks` | object | 否 | 作用于此子代理的生命周期钩子。支持所有钩子事件；`PreToolUse`、`PostToolUse` 和 `Stop` 最常用 |
| `memory` | string | 否 | 持久化内存作用域：`user`、`project` 或 `local` |
| `background` | boolean | 否 | 设为 `true` 始终作为后台任务运行（默认：`false`） |
| `effort` | string | 否 | 此子代理激活时的努力级别覆盖：`low`、`medium`、`high`、`max`。默认：从会话继承 |
| `isolation` | string | 否 | 设为 `"worktree"` 在临时 git worktree 中运行（如无更改则自动清理） |
| `initialPrompt` | string | 否 | 当此代理作为主会话代理运行时（通过 `--agent` 或 `agent` 设置），自动提交为第一个用户轮次。命令和技能会被处理。添加到任何用户提供的提示之前 |
| `color` | string | 否 | CLI 输出颜色以视觉区分（如 `green`、`magenta`）。功能有效但未出现在官方 frontmatter 表格中 —— 仅在交互式快速入门中记录 |

---

## ![官方](../!/tags/official.svg) 官方内置代理 **(6)**

| # | 代理 | 模型 | 工具 | 描述 |
|---|-------|-------|-------|-------------|
| 1 | `general-purpose` | inherit | 全部 | 复杂多步骤任务 —— 研究、代码搜索和自主工作的默认代理类型 |
| 2 | `Explore` | haiku | 只读（无 Write、Edit） | 快速代码库搜索和探索 —— 针对查找文件、搜索代码和回答代码库问题进行了优化 |
| 3 | `Plan` | inherit | 只读（无 Write、Edit） | 计划模式下的预规划研究 —— 在编写代码前探索代码库并设计实现方案 |
| 4 | `Bash` | inherit | Bash | 在独立上下文中运行终端命令 |
| 5 | `statusline-setup` | sonnet | Read, Edit | 配置用户的 Claude Code 状态栏设置 |
| 6 | `claude-code-guide` | haiku | Glob, Grep, Read, WebFetch, WebSearch | 回答关于 Claude Code 功能、Agent SDK 和 Claude API 的问题 |

---

## 来源

- [创建自定义子代理 — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [CLI 参考 — Claude Code 文档](https://code.claude.com/docs/en/cli-reference)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
