# Claude Code：全局 vs 项目级功能

全面比较哪些 Claude Code 功能仅限全局（`~/.claude/`），哪些同时具有全局和项目级（`.claude/`）等效功能。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [概述](#概述)
2. [仅限全局的功能](#仅限全局的功能)
3. [双作用域功能](#双作用域功能)
4. [设置优先级](#设置优先级)
5. [目录结构对比](#目录结构对比)
6. [Tasks 系统](#tasks-系统)
7. [Agent Teams](#agent-teams)
8. [设计原则](#设计原则)
9. [参考来源](#参考来源)

---

## 概述

Claude Code 使用**作用域层级结构**，其中部分功能同时存在于全局（`~/.claude/`）和项目（`.claude/`）级别，而其他功能则专属于全局。设计原则是：*个人状态*或*跨项目协调*的内容存放在全局；*团队可共享的项目配置*可以存放在项目级别。

- `~/.claude/` 是你的**用户级主目录**（全局，所有项目）
- 仓库内的 `.claude/` 是你的**项目级主目录**（仅限该项目）

---

## 仅限全局的功能

这些功能**仅**存在于 `~/.claude/` 下，无法限定到项目范围：

| 功能 | 位置 | 用途 |
|------|------|------|
| **Tasks** | `~/.claude/tasks/` | 跨会话和 Agent 的持久化任务列表 |
| **Agent Teams** | `~/.claude/teams/` | 多 Agent 协调配置（实验性，2026 年 2 月） |
| **自动记忆** | `~/.claude/projects/<hash>/memory/` | Claude 为每个项目自行撰写的学习记录（个人使用，永不共享） |
| **凭据与 OAuth** | 系统钥匙串 + `~/.claude.json` | API 密钥、OAuth 令牌（永不放在项目文件中） |
| **快捷键绑定** | `~/.claude/keybindings.json` | 自定义键盘快捷键 |
| **MCP 用户服务器** | `~/.claude.json`（`mcpServers` 键） | 跨所有项目的个人 MCP 服务器 |
| **偏好设置/缓存** | `~/.claude.json` | 主题、模型、输出风格、会话状态 |

---

## 双作用域功能

这些功能同时存在于两个级别，**项目级优先**于全局级：

| 功能 | 全局（`~/.claude/`） | 项目（`.claude/`） | 优先级 |
|------|---------------------|-------------------|--------|
| **CLAUDE.md** | `~/.claude/CLAUDE.md` | `./CLAUDE.md` 或 `.claude/CLAUDE.md` | 项目覆盖全局 |
| **设置** | `~/.claude/settings.json` | `.claude/settings.json` + `.claude/settings.local.json` | 项目 > 全局 |
| **规则** | `~/.claude/rules/*.md` | `.claude/rules/*.md` | 项目覆盖 |
| **Agent/Subagent** | `~/.claude/agents/*.md` | `.claude/agents/*.md` | 项目覆盖 |
| **Commands** | `~/.claude/commands/*.md` | `.claude/commands/*.md` | 两者均可使用 |
| **Skills** | `~/.claude/skills/` | `.claude/skills/` | 两者均可使用 |
| **Hooks** | `~/.claude/hooks/` | `.claude/hooks/` | 两者均执行 |
| **MCP 服务器** | `~/.claude.json`（用户作用域） | `.mcp.json`（项目作用域） | 三个作用域：本地 > 项目 > 用户 |

---

## 设置优先级

用户可写的设置按以下覆盖顺序应用（从高到低）：

| 优先级 | 位置 | 作用域 | 版本控制 | 用途 |
|--------|------|--------|---------|------|
| 1 | 命令行标志 | 会话 | 不适用 | 单次会话覆盖 |
| 2 | `.claude/settings.local.json` | 项目 | 否（git 忽略） | 个人项目特定设置 |
| 3 | `.claude/settings.json` | 项目 | 是（已提交） | 团队共享设置 |
| 4 | `~/.claude/settings.local.json` | 用户 | 不适用 | 个人全局覆盖 |
| 5 | `~/.claude/settings.json` | 用户 | 不适用 | 全局个人设置 |

策略层：`managed-settings.json` 由组织强制执行，无法被本地文件覆盖。

**重要**：`deny` 规则具有最高的安全优先级，无法被低优先级的 allow/ask 规则覆盖。

---

## 目录结构对比

### 全局作用域（`~/.claude/`）

```
~/.claude/
├── settings.json              # 用户级设置（所有项目）
├── settings.local.json        # 个人覆盖
├── CLAUDE.md                  # 用户记忆（所有项目）
├── agents/                    # 用户 Subagent（对所有项目可用）
│   └── *.md
├── rules/                     # 用户级模块化规则
│   └── *.md
├── commands/                  # 用户级 Commands
│   └── *.md
├── skills/                    # 用户级 Skills
│   └── */SKILL.md
├── tasks/                     # 仅限全局：任务列表
│   └── {task-list-id}/
├── teams/                     # 仅限全局：Agent Team 配置
│   └── {team-name}/
│       └── config.json
├── projects/                  # 仅限全局：每项目自动记忆
│   └── {project-hash}/
│       └── memory/
│           ├── MEMORY.md
│           └── *.md
├── keybindings.json           # 仅限全局：键盘快捷键
└── hooks/                     # 用户级 Hooks
    ├── scripts/
    └── config/

~/.claude.json                 # 仅限全局：MCP 服务器、OAuth、偏好设置、缓存
```

### 项目作用域（`.claude/`）

```
.claude/
├── settings.json              # 团队共享设置
├── settings.local.json        # 个人项目覆盖（git 忽略）
├── CLAUDE.md                  # 项目记忆（./CLAUDE.md 的替代方案）
├── agents/                    # 项目 Subagent
│   └── *.md
├── rules/                     # 项目级模块化规则
│   └── *.md
├── commands/                  # 自定义斜杠命令
│   └── *.md
├── skills/                    # 自定义 Skills
│   └── {skill-name}/
│       ├── SKILL.md
│       └── supporting-files/
├── hooks/                     # 项目级 Hooks
│   ├── scripts/
│   └── config/
└── plugins/                   # 已安装的插件

.mcp.json                      # 项目作用域的 MCP 服务器（仓库根目录）
```

---

## Tasks 系统

在 **Claude Code v2.1.16**（2026 年 1 月 22 日）中引入，取代了已弃用的 TodoWrite 系统。

### 存储

Tasks 存储在本地文件系统的 `~/.claude/tasks/` 中（不在云数据库中）。这使得任务状态可审计、可进行版本控制，并能在崩溃后恢复。

### 工具

| 工具 | 用途 |
|------|------|
| **TaskCreate** | 使用 `subject`、`description` 和 `activeForm` 创建新任务 |
| **TaskGet** | 通过 ID 获取特定任务的完整详情 |
| **TaskUpdate** | 更改状态、设置所有者、添加依赖或删除 |
| **TaskList** | 列出所有任务及其当前状态 |

### 任务生命周期

```
pending  →  in_progress  →  completed
```

### 依赖管理

Tasks 可以通过 `addBlockedBy`/`addBlocks` 阻塞其他任务，创建依赖图以防止过早执行。

### 多会话协作

```bash
CLAUDE_CODE_TASK_LIST_ID=my-project-tasks claude
```

共享相同 ID 的所有会话可以实时查看任务更新，支持并行工作流和会话恢复。

### 与旧版 Todos 的关键区别

| 功能 | 旧版 Todos | 新版 Tasks |
|------|-----------|-----------|
| 作用域 | 单次会话 | 跨会话、跨 Agent |
| 依赖关系 | 无 | 完整依赖图 |
| 存储 | 仅内存 | 文件系统（`~/.claude/tasks/`） |
| 持久性 | 会话结束即丢失 | 重启和崩溃后仍然存在 |
| 多会话 | 不可能 | 通过 `CLAUDE_CODE_TASK_LIST_ID` |

---

## Agent Teams

于 **2026 年 2 月 5 日**作为实验性功能发布。Agent Teams 允许多个 Claude Code 会话在共享工作上进行协调。

### 启用

```json
// 在 ~/.claude/settings.json 中
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### 配置

Team 配置位于 `~/.claude/teams/{team-name}/`，支持以下模式：

| 模式 | 描述 | 要求 |
|------|------|------|
| **进程内**（默认） | 所有队友运行在你的终端内 | 无 |
| **分屏** | 每个队友有自己的面板 | tmux 或 iTerm2（不支持 VS Code 终端） |

---

## 设计原则

仅限全局 vs 双作用域的划分遵循清晰的模式：

| 类别 | 作用域 | 理由 |
|------|--------|------|
| **协调状态**（tasks、teams） | 仅限全局 | 需要超越任何单个项目持久存在 |
| **安全状态**（凭据、OAuth） | 仅限全局 | 防止意外提交到版本控制 |
| **个人学习**（自动记忆） | 仅限全局 | 用户特定，不可团队共享 |
| **输入偏好**（快捷键绑定） | 仅限全局 | 用户肌肉记忆，非项目特定 |
| **配置**（设置、规则、agents） | 双作用域 | 团队需要共享项目特定行为 |
| **工作流定义**（commands、skills） | 双作用域 | 可以是个人或团队共享 |

自动记忆（`~/.claude/projects/<hash>/memory/`）是一个显著的混合体：它*关于*特定项目但*全局存储*，因为它代表的是个人学习而非团队可共享的配置。

---

## 参考来源

- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)
- [编排 Claude Code 会话团队](https://code.claude.com/docs/en/agent-teams)
- [Claude Code 中的 Tasks 是什么 — ClaudeLog](https://claudelog.com/faqs/what-are-tasks-in-claude-code/)
- [Claude Code 任务管理 — ClaudeFast](https://claudefa.st/blog/guide/development/task-management)
- [Claude Code Tasks 更新 — VentureBeat](https://venturebeat.com/orchestration/claude-codes-tasks-update-lets-agents-work-longer-and-coordinate-across)
- [Claude Code 全局设置在哪里 — ClaudeLog](https://claudelog.com/faqs/where-are-claude-code-global-settings/)
- [Claude Opus 4.6 Agent Teams — VentureBeat](https://venturebeat.com/technology/anthropics-claude-opus-4-6-brings-1m-token-context-and-agent-teams-to-take)
- [如何设置 Claude Code Agent Teams（完整指南）— r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/comments/1qz8tyy/how_to_set_up_claude_code_agent_teams_full/)
- [Anthropic 用 Tasks 替换了 Claude Code 旧版 'Todos' — r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1qkjznp/anthropic_replaced_claude_codes_old_todos_with/)
