# CLI 启动参数最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2002%2C%202026-white?style=flat&labelColor=555)

从终端启动 Claude Code 时，可用的启动参数、顶级子命令和启动环境变量参考。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 目录

1. [会话管理](#会话管理)
2. [模型与配置](#模型与配置)
3. [权限与安全](#权限与安全)
4. [输出与格式](#输出与格式)
5. [系统提示词](#系统提示词)
6. [Agent 与 Subagent](#agent-与-subagent)
7. [MCP 与插件](#mcp-与插件)
8. [目录与工作区](#目录与工作区)
9. [预算与限制](#预算与限制)
10. [集成](#集成)
11. [初始化与维护](#初始化与维护)
12. [调试与诊断](#调试与诊断)
13. [设置覆盖](#设置覆盖)
14. [版本与帮助](#版本与帮助)
15. [子命令](#子命令)
16. [环境变量](#环境变量)

---

## 会话管理

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--continue` | `-c` | 在当前目录继续最近一次对话 |
| `--resume` | `-r` | 通过 ID 或名称恢复指定会话，或显示交互式选择器 |
| `--from-pr <NUMBER\|URL>` | | 恢复与特定 GitHub PR 关联的会话 |
| `--fork-session` | | 恢复时创建新的会话 ID（与 `--resume` 或 `--continue` 搭配使用）|
| `--session-id <UUID>` | | 使用指定的会话 ID（必须是有效 UUID）|
| `--no-session-persistence` | | 禁用会话持久化（仅 print 模式）|
| `--remote` | | 在 claude.ai 上创建新的网页会话 |
| `--teleport` | | 在本地终端中恢复网页会话 |

---

## 模型与配置

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--model <NAME>` | | 通过别名（`sonnet`、`opus`、`haiku`）或完整模型 ID 设置模型 |
| `--fallback-model <NAME>` | | 当默认模型过载时自动切换的后备模型（仅 print 模式）|
| `--betas <LIST>` | | 在 API 请求中包含的 Beta 头（仅 API key 用户）|

---

## 权限与安全

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--dangerously-skip-permissions` | | 跳过**所有**权限提示。请极其谨慎使用 |
| `--allow-dangerously-skip-permissions` | | 允许把跳过权限作为一个可选项，但并不立即启用 |
| `--permission-mode <MODE>` | | 以指定权限模式启动：`default`、`plan`、`acceptEdits`、`bypassPermissions` |
| `--allowedTools <TOOLS>` | | 无需提示即可执行的工具（使用权限规则语法）|
| `--disallowedTools <TOOLS>` | | 从模型上下文中彻底移除的工具 |
| `--tools <TOOLS>` | | 限制 Claude 可使用的内置工具（使用 `""` 可禁用全部）|
| `--permission-prompt-tool <TOOL>` | | 指定在非交互模式下处理权限提示的 MCP 工具 |

---

## 输出与格式

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--print` | `-p` | 直接打印响应，不进入交互模式（headless/SDK 模式）|
| `--output-format <FORMAT>` | | 输出格式：`text`、`json`、`stream-json` |
| `--input-format <FORMAT>` | | 输入格式：`text`、`stream-json` |
| `--json-schema <SCHEMA>` | | 返回符合 schema 校验的 JSON（仅 print 模式）|
| `--include-partial-messages` | | 包含部分流式事件（需要 `--print` 和 `--output-format=stream-json`）|
| `--verbose` | | 启用详细日志，显示完整逐轮输出 |

---

## 系统提示词

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--system-prompt <TEXT>` | | 用自定义文本替换整个系统提示词 |
| `--system-prompt-file <PATH>` | | 从文件加载系统提示词并替换默认值（仅 print 模式）|
| `--append-system-prompt <TEXT>` | | 在默认系统提示词后追加自定义文本 |
| `--append-system-prompt-file <PATH>` | | 将文件内容追加到默认提示词后（仅 print 模式）|

---

## Agent 与 Subagent

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--agent <NAME>` | | 为当前会话指定一个 agent |
| `--agents <JSON>` | | 通过 JSON 动态定义自定义 subagents |
| `--teammate-mode <MODE>` | | 设置 agent team 的显示模式：`auto`、`in-process`、`tmux` |

---

## MCP 与插件

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--mcp-config <PATH\|JSON>` | | 从 JSON 文件或 JSON 字符串加载 MCP 服务器 |
| `--strict-mcp-config` | | 只使用 `--mcp-config` 中指定的 MCP 服务器，忽略其他所有来源 |
| `--plugin-dir <PATH>` | | 仅为本次会话从指定目录加载插件（可重复使用）|

---

## 目录与工作区

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--add-dir <PATH>` | | 添加 Claude 可访问的额外工作目录 |
| `--worktree` | `-w` | 在独立的 git worktree 中启动 Claude（从 HEAD 分支出来）|

---

## 预算与限制

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--max-budget-usd <AMOUNT>` | | API 调用允许的最高美元预算，超过即停止（仅 print 模式）|
| `--max-turns <NUMBER>` | | 限制代理执行轮数（仅 print 模式）|

---

## 集成

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--chrome` | | 启用 Chrome 浏览器集成以进行 Web 自动化 |
| `--no-chrome` | | 为本次会话禁用 Chrome 浏览器集成 |
| `--ide` | | 如果启动时恰好存在一个有效 IDE，则自动连接 |

---

## 初始化与维护

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--init` | | 运行初始化 hooks 并启动交互模式 |
| `--init-only` | | 只运行初始化 hooks 后退出（不启动交互会话）|
| `--maintenance` | | 运行维护 hooks 后退出 |

---

## 调试与诊断

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--debug <CATEGORIES>` | | 启用调试模式，并可选择指定分类过滤（如 `"api,hooks"`）|

---

## 设置覆盖

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--settings <PATH\|JSON>` | | 要加载的 settings JSON 文件路径或 JSON 字符串 |
| `--setting-sources <LIST>` | | 逗号分隔的设置来源列表：`user`、`project`、`local` |
| `--disable-slash-commands` | | 为当前会话禁用所有 skills 和 slash commands |

---

## 版本与帮助

| 参数 | 短参数 | 描述 |
|------|-------|-------------|
| `--version` | `-v` | 输出版本号 |
| `--help` | `-h` | 显示帮助信息 |

---

## 子命令

以下是以 `claude <subcommand>` 形式运行的顶级命令：

| 子命令 | 描述 |
|------------|-------------|
| `claude` | 启动交互式 REPL |
| `claude "query"` | 带初始提示词启动 REPL |
| `claude agents` | 列出已配置的 agents |
| `claude auth` | 管理 Claude Code 认证 |
| `claude doctor` | 在命令行中运行诊断 |
| `claude install` | 安装或切换 Claude Code 原生构建版本 |
| `claude mcp` | 配置 MCP 服务器（`add`、`remove`、`list`、`get`、`enable`）|
| `claude plugin` | 管理 Claude Code 插件 |
| `claude remote-control` | 管理远程控制会话 |
| `claude setup-token` | 创建用于订阅使用的长期 token |
| `claude update` / `claude upgrade` | 更新到最新版本 |

---

## 环境变量

这些仅在启动阶段生效的环境变量，需要在 shell 中设置后再启动 Claude Code（不能通过 `settings.json` 配置）：

| 变量 | 描述 |
|----------|-------------|
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | 启用实验性的 agent teams |
| `CLAUDE_CODE_TMPDIR` | 覆盖内部文件使用的临时目录 |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | 启用额外目录中的 CLAUDE.md 加载 |
| `DISABLE_AUTOUPDATER=1` | 禁用自动更新 |
| `CLAUDE_CODE_EFFORT_LEVEL` | 控制思考深度 —— 见 [设置参考](./claude-settings.md#环境变量-via-env) |
| `USE_BUILTIN_RIPGREP=0` | 使用系统安装的 ripgrep 而不是内置版本（适用于 Alpine Linux）|
| `CLAUDE_CODE_SIMPLE` | 启用简化模式（仅 Bash + Edit 工具）|
| `CLAUDE_BASH_NO_LOGIN=1` | 为 BashTool 跳过 login shell |

关于可通过 `settings.json` 中 `"env"` 键配置的环境变量（包括 `MAX_THINKING_TOKENS`、`CLAUDE_CODE_SHELL`、`CLAUDE_CODE_ENABLE_TASKS`、`CLAUDE_CODE_DISABLE_BACKGROUND_TASKS`、`CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` 等），请参见 [Claude 设置参考](./claude-settings.md#环境变量-via-env)。

---

## 来源

- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
- [Claude Code Headless Mode](https://code.claude.com/docs/en/headless)
- [Claude Code Setup](https://code.claude.com/docs/en/setup)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code Common Workflows](https://code.claude.com/docs/en/common-workflows)

