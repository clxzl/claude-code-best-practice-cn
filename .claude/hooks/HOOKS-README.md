# HOOKS-README
包含 hooks 的所有详细信息、脚本和使用说明

## Hook 事件概述 - [官方 22 个 Hooks](https://code.claude.com/docs/en/hooks)
Claude Code 提供了多个 hook 事件，在工作流程的不同阶段运行：

| # | Hook | 描述 | 选项 |
|:-:|------|-------------|---------|
| 1 | `PreToolUse` | 在工具调用之前运行（可以阻止调用） | `async`, `timeout: 5000`, `tool_use_id` |
| 2 | `PermissionRequest` | 当 Claude Code 向用户请求权限时运行 | `async`, `timeout: 5000`, `permission_suggestions` |
| 3 | `PostToolUse` | 在工具调用成功完成后运行 | `async`, `timeout: 5000`, `tool_response`, `tool_use_id` |
| 4 | `PostToolUseFailure` | 在工具调用失败后运行 | `async`, `timeout: 5000`, `error`, `is_interrupt`, `tool_use_id` |
| 5 | `UserPromptSubmit` | 当用户提交提示词时运行，在 Claude 处理之前 | `async`, `timeout: 5000`, `prompt` |
| 6 | `Notification` | 当 Claude Code 发送通知时运行 | `async`, `timeout: 5000`, `notification_type`, `message`, `title` |
| 7 | `Stop` | 当 Claude Code 完成响应时运行 | `async`, `timeout: 5000`, `last_assistant_message`, `stop_hook_active` |
| 8 | `SubagentStart` | 当子代理任务启动时运行 | `async`, `timeout: 5000`, `agent_id`, `agent_type` |
| 9 | `SubagentStop` | 当子代理任务完成时运行 | `async`, `timeout: 5000`, `agent_id`, `agent_type`, `last_assistant_message`, `agent_transcript_path`, `stop_hook_active` |
| 10 | `PreCompact` | 在 Claude Code 即将执行压缩操作之前运行 | `async`, `timeout: 5000`, `once`, `trigger`, `custom_instructions` |
| 11 | `PostCompact` | 在 Claude Code 完成压缩操作之后运行 | `async`, `timeout: 5000`, `trigger`, `compact_summary` |
| 12 | `SessionStart` | 当 Claude Code 启动新会话或恢复已有会话时运行 | `async`, `timeout: 5000`, `once`, `agent_type`, `model`, `source` |
| 13 | `SessionEnd` | 当 Claude Code 会话结束时运行 | `async`, `timeout: 5000`, `once`, `reason` |
| 14 | `Setup` | 当 Claude Code 运行 /setup 命令进行项目初始化时运行 | `async`, `timeout: 30000` |
| 15 | `TeammateIdle` | 当队友代理变为空闲状态时运行（实验性代理团队功能） | `async`, `timeout: 5000`, `teammate_name`, `team_name` |
| 16 | `TaskCompleted` | 当后台任务完成时运行（实验性代理团队功能） | `async`, `timeout: 5000`, `task_id`, `task_subject`, `task_description`, `teammate_name`, `team_name` |
| 17 | `ConfigChange` | 当会话期间配置文件发生变化时运行 | `async`, `timeout: 5000`, `file_path`, `source` |
| 18 | `WorktreeCreate` | 当代理 worktree 隔离为自定义 VCS 设置创建 worktree 时运行 | `async`, `timeout: 5000`, `name` |
| 19 | `WorktreeRemove` | 当代理 worktree 隔离为自定义 VCS 拆除移除 worktree 时运行 | `async`, `timeout: 5000`, `worktree_path` |
| 20 | `InstructionsLoaded` | 当 CLAUDE.md 或 `.claude/rules/*.md` 文件加载到上下文中时运行 | `async`, `timeout: 5000`, `file_path`, `memory_type`, `load_reason`, `globs`, `trigger_file_path`, `parent_file_path` |
| 21 | `Elicitation` | 当 MCP 服务器在工具调用期间请求用户输入时运行 | `async`, `timeout: 5000`, `mcp_server_name`, `message`, `mode`, `url`, `elicitation_id`, `requested_schema` |
| 22 | `ElicitationResult` | 当用户响应 MCP 引导请求后、响应发送回服务器之前运行 | `async`, `timeout: 5000`, `mcp_server_name`, `action`, `content`, `mode`, `elicitation_id` |

> **注意：** Hook 15-16（`TeammateIdle` 和 `TaskCompleted`）需要实验性代理团队功能。启动 Claude Code 时设置 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 以启用它们。

### 未在官方文档中列出

以下项目存在于 [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) 中，但**未列在** [官方 Hooks 参考](https://code.claude.com/docs/en/hooks) 中：

| 项目 | 添加版本 | 更新日志引用 | 备注 |
|------|----------|-------------------|-------|
| `Setup` hook | [v2.1.10](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#2110) | "添加了新的 Setup hook 事件，可通过 `--init`、`--init-only` 或 `--maintenance` CLI 标志触发，用于仓库设置和维护操作" | 未列在官方 hooks 参考页面中（列出 21 个 hooks，Setup 被排除） |
| Agent frontmatter hooks | [v2.1.0](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) | "为 agent frontmatter 添加了 hooks 支持，允许代理定义作用域限于代理生命周期的 PreToolUse、PostToolUse 和 Stop hooks" | 更新日志仅提到 3 个 hooks，但测试确认在代理会话中实际触发 **6 个 hooks**：PreToolUse、PostToolUse、PermissionRequest、PostToolUseFailure、Stop、SubagentStop。并非所有 16 个 hooks 都受支持。 |

## 前提条件

使用 hooks 之前，请确保系统上已安装 **Python 3**。

### 必需软件

#### 所有平台（Windows、macOS、Linux）
- **Python 3**：运行 hook 脚本所必需
- 验证安装：`python3 --version`

**安装说明：**
- **Windows**：从 [python.org](https://www.python.org/downloads/) 下载，或通过 `winget install Python.Python.3` 安装
- **macOS**：通过 `brew install python3` 安装（需要 [Homebrew](https://brew.sh/)）
- **Linux**：通过 `sudo apt install python3`（Ubuntu/Debian）或 `sudo yum install python3`（RHEL/CentOS）安装

### 音频播放器（可选 - 自动检测）

Hook 脚本会自动检测并使用适合您平台的音频播放器：

- **macOS**：使用 `afplay`（内置，无需安装）
- **Linux**：使用 `pulseaudio-utils` 中的 `paplay` - 通过 `sudo apt install pulseaudio-utils` 安装
- **Windows**：使用内置的 `winsound` 模块（Python 自带）

### Hooks 如何执行

Hooks 在 `.claude/settings.json` 中配置为直接使用 Python 3 运行：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py"
}
```

## 配置 Hooks（启用/禁用）

Hooks 可以在全局和单个级别轻松启用或禁用。

### 一次性禁用所有 Hooks

编辑 `.claude/settings.local.json` 并设置：
```json
{
  "disableAllHooks": true
}
```

**注意：** `.claude/settings.local.json` 文件被 git 忽略，因此每个用户可以配置自己的 hook 偏好设置，而不会影响团队在 `.claude/settings.json` 中的共享设置。

> **托管设置：** 如果管理员通过托管策略设置配置了 hooks，则在用户、项目或本地设置中设置的 `disableAllHooks` 无法禁用这些托管 hooks（已在 v2.1.49 中修复）。

### 禁用单个 Hooks

如需精细控制，您可以通过编辑 hooks 配置文件来禁用特定的 hooks。

#### 配置文件

有两个配置文件用于管理单个 hooks：

1. **`.claude/hooks/config/hooks-config.json`** - 提交到 git 的共享/默认配置
2. **`.claude/hooks/config/hooks-config.local.json`** - 您的个人覆盖配置（git 忽略）

本地配置文件（`.local.json`）优先于共享配置，允许每位开发者自定义其 hook 行为而不影响团队。

#### 共享配置

编辑 `.claude/hooks/config/hooks-config.json` 设置团队范围的默认值：

```json
{
  "禁用日志记录": false,
  "禁用工具使用前钩子": false,
  "禁用权限请求钩子": false,
  "禁用工具使用后钩子": false,
  "禁用工具使用失败后钩子": false,
  "禁用用户提示提交钩子": false,
  "禁用通知钩子": false,
  "禁用停止钩子": false,
  "禁用子代理启动钩子": false,
  "禁用子代理停止钩子": false,
  "禁用压缩前钩子": false,
  "禁用压缩后钩子": false,
  "禁用信息抽取钩子": false,
  "禁用信息抽取结果钩子": false,
  "禁用会话启动钩子": false,
  "禁用会话结束钩子": false,
  "禁用设置钩子": false,
  "禁用队友空闲钩子": false,
  "禁用任务完成钩子": false,
  "禁用配置更改钩子": false,
  "禁用工作树创建钩子": false,
  "禁用工作树移除钩子": false,
  "禁用指令加载钩子": false
}
```

**配置选项：**
- `disableLogging`：设置为 `true` 以禁用将 hook 事件记录到 `.claude/hooks/logs/hooks-log.jsonl`（有助于防止日志文件增长）

#### 本地配置（个人覆盖）

创建或编辑 `.claude/hooks/config/hooks-config.local.json` 设置个人偏好：

```json
{
  "disableLogging": true,
  "disablePostToolUseHook": true,
  "disableSessionStartHook": true
}
```

在此示例中，日志记录被禁用，PostToolUse 和 SessionStart hooks 在本地被覆盖。所有其他 hooks 将使用共享配置的值。

**注意：** 单个 hook 开关由 hook 脚本（`.claude/hooks/scripts/hooks.py`）检查。本地设置覆盖共享设置，如果某个 hook 被禁用，脚本将静默退出，不播放任何声音或执行 hook 逻辑。

### 文本转语音 (TTS)
用于生成声音的网站：https://elevenlabs.io/
使用的声音：Samara X

## Agent Frontmatter Hooks

Claude Code 2.1.0 引入了对在 agent frontmatter 文件中定义的 agent 特定 hooks 的支持。这些 hooks 仅在代理的生命周期内运行，并支持 hook 事件的子集。

### 支持的 Agent Hooks

Agent frontmatter hooks 支持 **6 个 hooks**（而非全部 16 个）。更新日志最初仅提到 3 个，但测试确认在代理会话中实际触发 6 个 hooks：
- `PreToolUse`：在代理使用工具之前运行
- `PostToolUse`：在代理完成工具使用之后运行
- `PermissionRequest`：当工具需要用户权限时运行
- `PostToolUseFailure`：当工具调用失败后运行
- `Stop`：当代理完成时运行
- `SubagentStop`：当子代理完成时运行

> **注意：** [v2.1.0 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) 仅提及了 3 个钩子：*"Added hooks support to agent frontmatter, allowing agents to define PreToolUse, PostToolUse, and Stop hooks scoped to the agent's lifecycle"*。然而，使用 `claude-code-voice-hook-agent` 进行测试确认，代理会话中实际上会触发 6 个钩子。其余 10 个钩子（例如 Notification、SessionStart、SessionEnd 等）不会在代理上下文中触发。
>
> **更新（2026年2月）：** [官方钩子参考文档](https://code.claude.com/docs/en/hooks) 现在声明技能/代理 frontmatter 钩子 *"支持所有钩子事件"*。这可能意味着支持范围已扩展到最初测试的 6 个钩子之外。建议重新测试以验证代理会话中是否现在会触发更多钩子。

### 代理声音文件夹

代理特定的声音存储在单独的文件夹中：
- `.claude/hooks/sounds/agent_pretooluse/`
- `.claude/hooks/sounds/agent_posttooluse/`
- `.claude/hooks/sounds/agent_permissionrequest/`
- `.claude/hooks/sounds/agent_posttoolusefailure/`
- `.claude/hooks/sounds/agent_stop/`
- `.claude/hooks/sounds/agent_subagentstop/`

### 创建带钩子的代理

1. 在 `.claude/agents/` 中创建代理定义文件：

```markdown
---
name: my-agent
description: 此Agent的功能描述
hooks:
  PreToolUse:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: 工具使用前
  PostToolUse:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: 工具使用后
  PermissionRequest:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: 权限请求
  PostToolUseFailure:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: 工具使用失败后
  Stop:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: 停止
  SubagentStop:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: Subagent停止
```
---
```

Your agent instructions here...
```

2. 将声音文件添加到代理声音文件夹：
   - `agent_pretooluse/agent_pretooluse.wav`
   - `agent_posttooluse/agent_posttooluse.wav`
   - `agent_permissionrequest/agent_permissionrequest.wav`
   - `agent_posttoolusefailure/agent_posttoolusefailure.wav`
   - `agent_stop/agent_stop.wav`
   - `agent_subagentstop/agent_subagentstop.wav`

### 示例：天气获取代理

请参阅 `.claude/agents/claude-code-voice-hook-agent.md`，了解配置了钩子的代理完整示例。

### 钩子选项：`once: true`

`once: true` 选项确保钩子每个会话只运行一次：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "once": true
}
```

这对于 `SessionStart`、`SessionEnd` 和 `PreCompact` 等只需触发一次的钩子非常有用。

> **注意：** `once` 选项**仅适用于技能，不适用于代理**。它在基于设置的钩子和技能 frontmatter 中有效，但不支持代理 frontmatter 钩子。

### 钩子选项：`async: true`

通过添加 `"async": true`，钩子可以在后台运行而不阻塞 Claude Code 的执行：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true
}
```

**何时使用异步钩子：**
- 日志记录和分析
- 通知和声音效果
- 任何不应拖慢 Claude Code 的副作用

本项目对所有钩子使用 `async: true`，因为语音通知是不需要阻塞执行的副作用。`timeout` 指定异步钩子在被终止前可以运行多长时间。

### 钩子选项：`statusMessage`

`statusMessage` 字段设置钩子运行时向用户显示的自定义加载提示消息：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true,
  "statusMessage": "工具使用前"
}
```

本项目将所有钩子的 `statusMessage` 设置为钩子事件名称，这样加载提示会短暂显示正在触发哪个钩子（例如 "PreToolUse"、"SessionStart"、"Stop"）。这在同步钩子中最为明显；对于异步钩子，消息会在钩子在后台运行前短暂闪现。

## 钩子类型

Claude Code 支持四种钩子处理器类型。本项目对所有声音播放使用 `command` 钩子。

### `type: "command"`（本项目使用）

运行 shell 命令。通过 stdin 接收 JSON 输入，通过退出码和 stdout 传达结果。

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true
}
```

### `type: "prompt"`

向 Claude 模型发送提示进行单轮评估。模型返回是/否决策的 JSON（`{"ok": true/false, "reason": "..."}`）。适用于需要判断力而非确定性规则的决策。

```json
{
  "type": "prompt",
  "prompt": "Check if all tasks are complete. $ARGUMENTS",
  "timeout": 30
}
```

**支持的事件：** PreToolUse、PostToolUse、PostToolUseFailure、PermissionRequest、UserPromptSubmit、Stop、SubagentStop、TaskCompleted。**仅支持 command 的事件（不支持 prompt/agent 类型）：** ConfigChange、Elicitation、ElicitationResult、InstructionsLoaded、Notification、PostCompact、PreCompact、SessionEnd、SessionStart、Setup、SubagentStart、TeammateIdle、WorktreeCreate、WorktreeRemove。

### `type: "agent"`

生成一个具有多轮工具访问权限（Read、Grep、Glob）的子代理，用于在返回决策前验证条件。响应格式与 prompt 钩子相同。适用于验证需要检查实际文件或测试输出的场景。

```json
{
  "type": "agent",
  "prompt": "Verify that all unit tests pass. $ARGUMENTS",
  "timeout": 120
}
```

### `type: "http"` (自 v2.1.63 起)

向 URL 发送 JSON POST 请求并接收 JSON 响应，而不是运行 shell 命令。适用于与外部服务或 webhook 集成。当启用沙箱时，HTTP hook 通过沙箱网络代理进行路由。

```json
{
  "类型": "http",
  "地址": "http://localhost:8080/hooks/pre-tool-use",
  "超时时间": 30,
  "请求头": {
    "Authorization": "Bearer $MY_TOKEN"
  },
  "允许的环境变量": ["MY_TOKEN"]
}
```

**不支持以下事件：** ConfigChange、Elicitation、ElicitationResult、InstructionsLoaded、Notification、PostCompact、PreCompact、SessionEnd、SessionStart、Setup、SubagentStart、TeammateIdle、WorktreeCreate、WorktreeRemove（仅限 command 类型事件）。Headers 支持使用 `$VAR_NAME` 进行环境变量插值，但仅限于在 `allowedEnvVars` 中明确列出的变量。

## 环境变量

Claude Code 为 hook 脚本提供以下环境变量：

| 变量 | 可用性 | 描述 |
|----------|-------------|-------------|
| `$CLAUDE_PROJECT_DIR` | 所有 hook | 项目根目录。对于包含空格的路径请用引号包裹 |
| `$CLAUDE_ENV_FILE` | 仅 SessionStart | 用于为后续 Bash 命令持久化环境变量的文件路径。使用追加（`>>`）以保留其他 hook 设置的变量 |
| `${CLAUDE_PLUGIN_ROOT}` | Plugin hook | Plugin 的根目录，用于与 plugin 一起打包的脚本 |
| `$CLAUDE_CODE_REMOTE` | 所有 hook | 在远程 Web 环境中设为 `"true"`，在本地 CLI 中未设置 |
| `${CLAUDE_SKILL_DIR}` | Skill hook | Skill 自身的目录，用于与 skill 一起打包的脚本（自 v2.1.69 起） |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hook | 以毫秒为单位覆盖 SessionEnd hook 的超时时间。在 v2.1.74 之前，SessionEnd hook 无论配置的 `timeout` 如何都会在 1.5 秒后被终止。现在遵循 hook 的 `timeout` 值，或在设置了此环境变量时使用此值（自 v2.1.74 起） |
| `session_id`（通过 stdin JSON） | 所有 hook | 当前会话 ID，作为 stdin JSON 输入的一部分接收（不是环境变量） |

### 通用输入字段（stdin JSON）

每个 hook 都会在 stdin 上接收一个 JSON 对象，其中包含以下通用字段，以及上方选项列中列出的任何 hook 特定字段：

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `hook_event_name` | string | 触发的 hook 事件名称（例如 `"PreToolUse"`、`"Stop"`） |
| `session_id` | string | 当前会话标识符 |
| `transcript_path` | string | 对话记录 JSON 文件的路径 |
| `cwd` | string | 当前工作目录 |
| `permission_mode` | string | 当前权限模式：`default`、`plan`、`acceptEdits`、`dontAsk` 或 `bypassPermissions` |
| `agent_id` | string | 唯一的子代理标识符。当 hook 在子代理上下文中触发时出现（自 v2.1.69 起） |
| `agent_type` | string | 代理类型名称（例如 `Bash`、`Explore`、`Plan` 或自定义）。使用 `--agent <name>` 标志或在子代理内部时出现（自 v2.1.69 起） |

> **注意：** Hook 特定字段（例如 PreToolUse 的 `tool_name`、Stop 的 `last_assistant_message`）列在上方 [Hook 事件概览](#hook-events-overview---official-19-hooks) 表格的选项列中。

## Hook 管理命令

Claude Code 提供内置命令来管理 hook：

- **`/hooks`** — 交互式 hook 管理界面。无需编辑 JSON 文件即可查看、添加和删除 hook。Hook 按来源标记：`[User]`、`[Project]`、`[Local]`、`[Plugin]`。您还可以从此菜单切换 `disableAllHooks`。
- **`claude hooks reload`** — 重新加载 hook 配置而无需重启会话。在编辑设置文件后使用（自 v2.0.47 起）。

## MCP 工具匹配器

对于 `PreToolUse`、`PostToolUse` 和 `PermissionRequest` hook，您可以使用 `mcp__<server>__<tool>` 模式匹配 MCP（Model Context Protocol）工具：

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "mcp__memory__.*",
      "hooks": [{ "type": "command", "command": "echo 'MCP memory tool 已使用'" }]
    }]
  }
}
```

支持完整的正则表达式：`mcp__memory__.*`（来自 memory 服务器的所有工具）、`mcp__.*__write.*`（来自任何服务器的任何写入工具）。

### 每个 Hook 的匹配器参考

匹配器用于筛选哪些事件会触发 hook。并非所有 hook 都支持匹配器——不支持匹配器的 hook 始终会触发。

| Hook | 匹配器字段 | 可能的值 | 示例 |
|------|--------------|-----------------|---------|
| `PreToolUse` | `tool_name` | 任何工具名称：`Bash`、`Read`、`Edit`、`Write`、`Glob`、`Grep`、`mcp__*` | `"matcher": "Bash"` |
| `PermissionRequest` | `tool_name` | 与 PreToolUse 相同 | `"matcher": "mcp__memory__.*"` |
| `PostToolUse` | `tool_name` | 与 PreToolUse 相同 | `"matcher": "Write"` |
| `PostToolUseFailure` | `tool_name` | 与 PreToolUse 相同 | `"matcher": "Bash"` |
| `Notification` | `notification_type` | `permission_prompt`、`idle_prompt`、`auth_success`、`elicitation_dialog` | `"matcher": "permission_prompt"` |
| `SubagentStart` | `agent_type` | `Bash`、`Explore`、`Plan` 或自定义代理名称 | `"matcher": "Bash"` |
| `SubagentStop` | `agent_type` | `Bash`、`Explore`、`Plan` 或自定义代理名称 | `"matcher": "Bash"` |
| `SessionStart` | `source` | `startup`、`resume`、`clear`、`compact` | `"matcher": "startup"` |
| `SessionEnd` | `reason` | `clear`、`logout`、`prompt_input_exit`、`bypass_permissions_disabled`、`other` | `"matcher": "logout"` |
| `PreCompact` | `trigger` | `manual`、`auto` | `"matcher": "auto"` |
| `PostCompact` | `trigger` | `manual`、`auto` | `"matcher": "manual"` |
| `Elicitation` | `mcp_server_name` | MCP 服务器名称 | `"matcher": "my-mcp-server"` |
| `ElicitationResult` | `mcp_server_name` | MCP 服务器名称 | `"matcher": "my-mcp-server"` |
| `ConfigChange` | `source` | `user_settings`、`project_settings`、`local_settings`、`policy_settings`、`skills` | `"matcher": "project_settings"` |
| `UserPromptSubmit` | — | 不支持匹配器 | 始终触发 |
| `Stop` | — | 不支持匹配器 | 始终触发 |
| `TeammateIdle` | — | 不支持匹配器 | 始终触发 |
| `TaskCompleted` | — | 不支持匹配器 | 始终触发 |
| `WorktreeCreate` | — | 不支持匹配器 | 始终触发 |
| `WorktreeRemove` | — | 不支持匹配器 | 始终触发 |
| `InstructionsLoaded` | — | 不支持匹配器 | 始终触发 |
| `Setup` | — | 不支持匹配器 | 始终触发 |

## 已知问题与解决方案

### Agent Stop Hook Bug（SubagentStop vs Stop）

**Bug 报告：** [GitHub Issue #19220](https://github.com/anthropics/claude-code/issues/19220)

**问题：** 在 agent 的 frontmatter 中定义 `Stop` hook 时，传递给 hook 脚本的 `hook_event_name` 是 `"SubagentStop"` 而不是 `"Stop"`。这与官方文档相矛盾，并且与其他 agent hook（`PreToolUse` 和 `PostToolUse`）的不一致，后者正确传递其配置的名称。

| Hook | 定义为 | 接收到 | 状态 |
|------|------------|-------------|--------|
| PreToolUse | `PreToolUse:` | `"PreToolUse"` | ✅ 正确 |
| PostToolUse | `PostToolUse:` | `"PostToolUse"` | ✅ 正确 |
| Stop | `Stop:` | `"SubagentStop"` | ❌ 不一致 |

**状态：** [官方 hook 参考文档](https://code.claude.com/docs/en/hooks#hooks-in-skills-and-agents) 现已将其记录为预期行为：*"对于子代理，Stop hook 会自动转换为 SubagentStop，因为这是子代理完成时触发的事件。"* 本项目通过 `hooks.py` 中的 `AGENT_HOOK_SOUND_MAP` 处理此问题，其中有一个单独的 `SubagentStop` 条目映射到 `agent_subagentstop` 声音文件夹。

### PreToolUse 决策控制弃用

`PreToolUse` hook 以前使用顶层 `decision` 和 `reason` 字段来阻止工具调用。这些字段现已 **弃用**。请改用 `hookSpecificOutput.permissionDecision` 和 `hookSpecificOutput.permissionDecisionReason`：

| 已弃用 | 当前 |
|-----------|---------|
| `"decision": "approve"` | `"hookSpecificOutput": { "permissionDecision": "allow" }` |
| `"decision": "block"` | `"hookSpecificOutput": { "permissionDecision": "deny" }` |

这不会影响本项目，因为 `hooks.py` 使用异步声音播放，不使用决策控制。

## 决策控制模式

不同的 hook 使用不同的输出模式来阻止或控制执行。本项目不使用决策控制（所有 hook 都是异步声音播放），但仅供参考：

| Hook | 控制方法 | 值 |
|---------|---------------|--------|
| PreToolUse | `hookSpecificOutput.permissionDecision` | `allow`、`deny`、`ask` |
| PreToolUse | `hookSpecificOutput.autoAllow` | `true` — 自动批准此工具的未来使用（自 v2.0.76 起） |
| PermissionRequest | `hookSpecificOutput.decision.behavior` | `allow`、`deny` |
| PostToolUse、PostToolUseFailure、Stop、SubagentStop、ConfigChange | 顶层 `decision` | `block` |
| TeammateIdle、TaskCompleted | `continue` + 退出码 2 | `{"continue": false, "stopReason": "..."}` — JSON 决策控制在 v2.1.70 中添加 |
| UserPromptSubmit | 可修改 `prompt` 字段 | 通过 stdout 返回修改后的 prompt |
| WorktreeCreate | 非零退出 + stdout 路径 | 非零退出会导致创建失败；stdout 提供 worktree 路径 |
| Elicitation | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 控制 MCP elicitation 响应 |
| ElicitationResult | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 在发送到服务器之前覆盖用户响应 |

### 通用 JSON 输出字段

所有 hook 都可以通过 stdout JSON 返回以下字段：

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `continue` | bool | 如果为 `false`，则完全停止 Claude |
| `stopReason` | string | 当 `continue` 为 false 时显示的消息 |
| `suppressOutput` | bool | 在详细模式下隐藏 stdout |
| `systemMessage` | string | 向用户显示的警告消息 |
| `additionalContext` | string | 添加到 Claude 对话中的上下文 |

## Hook 去重与外部更改

- **Hook 去重：** 在多个设置位置定义的相同 hook 处理器在并行运行时只执行一次，防止重复执行。
