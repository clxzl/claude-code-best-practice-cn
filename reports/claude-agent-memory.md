# Claude Code：Agent 记忆 Frontmatter

Subagent 的持久化记忆 — 使 Agent 能够跨会话学习、记忆并构建知识。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

在 **Claude Code v2.1.33**（2026 年 2 月）中引入的 `memory` frontmatter 字段，为每个 Subagent 提供了独立的基于 Markdown 的持久化知识存储。在此之前，每次 Agent 调用都从零开始。

---
name: code-reviewer
description: 审查代码质量与最佳实践
tools: Read, Write, Edit, Bash
model: sonnet
memory: user
---

You are a code reviewer. As you review code, update your agent memory with
patterns, conventions, and recurring issues you discover.
```

---

## 记忆作用域

| 作用域 | 存储位置 | 版本控制 | 共享 | 最佳用途 |
|--------|---------|---------|------|---------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | 否 | 否 | 跨项目知识（推荐默认值） |
| `project` | `.claude/agent-memory/<agent-name>/` | 是 | 是 | 团队应共享的项目特定知识 |
| `local` | `.claude/agent-memory-local/<agent-name>/` | 否（git 忽略） | 否 | 个人的项目特定知识 |

这些作用域与设置层级结构一致（`~/.claude/settings.json` → `.claude/settings.json` → `.claude/settings.local.json`）。

---

## 工作原理

1. **启动时**：`MEMORY.md` 的前 200 行被注入到 Agent 的系统提示中
2. **工具访问**：`Read`、`Write`、`Edit` 被自动启用，使 Agent 可以管理其记忆
3. **执行期间**：Agent 可以自由读写其记忆目录
4. **整理**：如果 `MEMORY.md` 超过 200 行，Agent 会将详细内容移至主题特定文件

```
~/.claude/agent-memory/code-reviewer/     # user 作用域示例
├── MEMORY.md                              # 主文件（前 200 行被加载）
├── react-patterns.md                      # 主题特定文件
└── security-checklist.md                  # 主题特定文件
```

---

## Agent 记忆 vs 其他记忆系统

| 系统 | 谁写入 | 谁读取 | 作用域 |
|------|-------|-------|-------|
| **CLAUDE.md** | 你（手动） | 主 Claude + 所有 Agent | 项目 |
| **自动记忆** | 主 Claude（自动） | 仅主 Claude | 每个项目每个用户 |
| **`/memory` 命令** | 你（通过编辑器） | 仅主 Claude | 每个项目每个用户 |
| **Agent 记忆** | Agent 自身 | 仅该特定 Agent | 可配置（user/project/local） |

这些系统是**互补的** — Agent 同时读取 CLAUDE.md（项目上下文）和自身的记忆（Agent 特定知识）。

---

## 实际示例

```yaml
---
name: api-developer
description: 遵循团队规范实现API端点
tools: Read, Write, Edit, Bash
model: sonnet
memory: project
skills:
  - api-conventions
  - error-handling-patterns
---
```

实现API端点。遵循你预加载技能中的约定。
在工作过程中，将架构决策和模式保存到记忆中。
```

这将 **Skills**（启动时的静态知识）与 **记忆**（随时间构建的动态知识）结合在一起。

---

## 技巧

- **提示记忆用法** — 包含明确指令：`"Before starting, review your memory. After completing, update your memory with what you learned."`
- **调用 Agent 时请求记忆检查**：`"Review this PR, and check your memory for patterns you've seen before."`
- **选择正确的范围** — `user` 用于跨项目，`project` 用于团队共享，`local` 用于个人使用

---

## 参考来源

- [创建自定义 Subagent — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [管理 Claude 的记忆 — Claude Code 文档](https://code.claude.com/docs/en/memory)
- [Claude Code v2.1.33 发布说明](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
