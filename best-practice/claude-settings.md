# Claude Code 设置参考

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2027%2C%202026%206%3A32%20PM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.85-blue?style=flat&labelColor=555)

Claude Code `settings.json` 文件所有可用配置选项的全面指南。截至 v2.1.85，Claude Code 暴露 **60+ 设置** 和 **100+ 环境变量**（使用 `settings.json` 中的 `"env"` 字段可避免包装脚本）。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [设置层级](#设置层级)
2. [核心配置](#核心配置)
3. [权限](#权限)
4. [钩子](#钩子)
5. [MCP 服务器](#mcp-服务器)
6. [沙箱](#沙箱)
7. [插件](#插件)
8. [模型配置](#模型配置)
9. [显示与用户体验](#显示与用户体验)
10. [AWS 与云凭证](#aws-与云凭证)
11. [环境变量](#环境变量-via-env)
12. [实用命令](#实用命令)

---

## 设置层级

设置按优先级从高到低应用：

| 优先级 | 位置 | 范围 | 共享？ | 用途 |
|----------|----------|-------|---------|---------|
| 1 | 托管设置 | 组织 | 是（IT 部署）| 无法覆盖的安全策略 |
| 2 | 命令行参数 | 会话 | 不适用 | 临时单会话覆盖 |
| 3 | `.claude/settings.local.json` | 项目 | 否（git 忽略）| 个人项目特定设置 |
| 4 | `.claude/settings.json` | 项目 | 是（提交到 git）| 团队共享设置 |
| 5 | `~/.claude/settings.json` | 用户 | 不适用 | 全局个人默认值 |

**托管设置**由组织强制执行，无法被任何其他级别覆盖，包括命令行参数。交付方式：
- **服务器托管**设置（远程交付）
- **MDM 配置文件** — macOS plist 位于 `com.anthropic.claudecode`
- **注册表策略** — Windows `HKLM\SOFTWARE\Policies\ClaudeCode`（管理员）和 `HKCU\SOFTWARE\Policies\ClaudeCode`（用户级，最低策略优先级）
- **文件** — `managed-settings.json`（macOS: `/Library/Application Support/ClaudeCode/`，Linux/WSL: `/etc/claude-code/`，Windows: `C:\Program Files\ClaudeCode\`）
- **置入目录** — `managed-settings.d/` 与 `managed-settings.json` 并存，用于独立策略片段（v2.1.83）。遵循 systemd 约定，`managed-settings.json` 首先作为基础合并，然后置入目录中的所有 `*.json` 文件按字母顺序排序并合并覆盖。后文件覆盖前文件的标量值；数组拼接并去重；对象深度合并。以 `.` 开头的隐藏文件被忽略。使用数字前缀控制合并顺序（如 `10-telemetry.json`、`20-security.json`）

在托管层级内，优先级为：服务器托管 > MDM/OS 级策略 > 基文件（`managed-settings.d/*.json` + `managed-settings.json`）> HKCU 注册表（仅 Windows）。只使用一个托管源；源之间不跨层级合并。在基于文件的层级内，置入文件和基础文件一起合并。

> **注意：** 从 v2.1.75 起，已弃用的 Windows 回退路径 `C:\ProgramData\ClaudeCode\managed-settings.json` 已被移除。请使用 `C:\Program Files\ClaudeCode\managed-settings.json`。

**重要**：
- `deny` 规则具有最高安全优先级，无法被低优先级的 allow/ask 规则覆盖。
- 托管设置可能会锁定或覆盖本地行为，即使本地文件指定了不同的值。
- 数组设置（如 `permissions.allow`）在作用域间**拼接并去重** —— 来自所有级别的条目被合并，而非替换。

---

## 核心配置

### 通用设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `$schema` | string | - | JSON Schema URL，用于 IDE 验证和自动补全（如 `"https://json.schemastore.org/claude-code-settings.json"`）|
| `model` | string | `"default"` | 覆盖默认模型。接受别名（`sonnet`、`opus`、`haiku`）或完整模型 ID |
| `agent` | string | - | 设置主会话的默认代理。值为 `.claude/agents/` 中的代理名称。也可通过 `--agent` CLI 标志设置 |
| `language` | string | `"english"` | Claude 的首选响应语言 |
| `cleanupPeriodDays` | number | `30` | 超过此天数的非活跃会话在启动时被删除。设为 `0` 删除所有现有记录并完全禁用会话持久化（不写入 `.jsonl` 文件，`/resume` 无会话，钩子收到空 `transcript_path`）|
| `autoUpdatesChannel` | string | `"latest"` | 发布渠道：`"stable"` 或 `"latest"` |
| `alwaysThinkingEnabled` | boolean | `false` | 默认为所有会话启用扩展思考 |
| `skipWebFetchPreflight` | boolean | `false` | 在获取 URL 前跳过 WebFetch 黑名单检查 *（在 JSON schema 中，不在官方设置页面）* |
| `availableModels` | array | - | 限制用户通过 `/model`、`--model`、Config 工具或 `ANTHROPIC_MODEL` 可选择的模型。不影响默认选项。示例：`["sonnet", "haiku"]` |
| `fastModePerSessionOptIn` | boolean | `false` | 要求用户每次会话选择加入快速模式 |
| `teammateMode` | string | `"auto"` | 代理团队显示模式：`"auto"`（tmux/iTerm2 中分屏，否则进程内）、`"in-process"` 或 `"tmux"` |
| `defaultShell` | string | `"bash"` | 输入框 `!` 命令的默认 shell。接受 `"bash"`（默认）或 `"powershell"`。设置 `"powershell"` 在 Windows 上通过 PowerShell 路由交互式 `!` 命令。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`（v2.1.84）|
| `includeGitInstructions` | boolean | `true` | 在系统提示中包含 git 相关指令 |
| `voiceEnabled` | boolean | - | 启用按说话式语音输入。运行 `/voice` 时自动写入。需要 Claude.ai 账户 |
| `showClearContextOnPlanAccept` | boolean | `false` | 在计划接受屏幕显示"清除上下文"选项。设为 `true` 恢复该选项（v2.1.81 起默认隐藏）|
| `disableDeepLinkRegistration` | boolean | - | 防止启动时注册 `claude-cli://` 协议处理器（v2.1.83）*（在 changelog 中，不在官方设置页面）* |
| `feedbackSurveyRate` | number | - | 符合条件的会话显示质量调查的概率（0-1）。企业管理员可控制调查显示频率。示例：`0.05` = 5% 的符合条件的会话 |

**示例：**
```json
{
  "model": "opus",
  "agent": "code-reviewer",
  "language": "japanese",
  "cleanupPeriodDays": 60,
  "autoUpdatesChannel": "stable",
  "alwaysThinkingEnabled": true
}
```

### 计划与记忆目录

将计划和自动记忆文件存储在自定义位置。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `plansDirectory` | string | `~/.claude/plans` | `/plan` 输出存储目录 |
| `autoMemoryDirectory` | string | - | 自动记忆存储的自定义目录。接受 `~/` 扩展路径。项目设置（`.claude/settings.json`）中不接受，以防止将记忆写入敏感位置；策略、本地和用户设置中接受 |

**示例：**
```json
{
  "plansDirectory": "./my-plans"
}
```

**用例：** 用于将规划工件与 Claude 内部文件分开组织，或将计划保存在团队共享位置。

### Worktree 设置

配置 `--worktree` 如何创建和管理 git worktree。用于减少大型 monorepo 中的磁盘使用和启动时间。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `worktree.symlinkDirectories` | array | `[]` | 从主仓库符号链接到每个 worktree 的目录，避免磁盘上重复大型目录 |
| `worktree.sparsePaths` | array | `[]` | 通过 git sparse-checkout（cone 模式）在每个 worktree 中检出的目录。只有列出的路径写入磁盘 |

**示例：**
```json
{
  "worktree": {
    "symlinkDirectories": ["node_modules", ".cache"],
    "sparsePaths": ["packages/my-app", "shared/utils"]
  }
}
```

### 归属设置

自定义 git 提交和 pull request 的归属消息。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `attribution.commit` | string | Co-authored-by | Git 提交归属（支持 trailers）|
| `attribution.pr` | string | Generated message | Pull request 描述归属 |
| `includeCoAuthoredBy` | boolean | `true` | **已弃用** - 使用 `attribution` 代替 |

**示例：**
```json
{
  "attribution": {
    "commit": "Generated with AI\n\nCo-Authored-By: Claude <noreply@anthropic.com>",
    "pr": "Generated with Claude Code"
  }
}
```

**用例：**
- 禁用归属：`"attribution.commit": ""`
- 自定义归属消息样式
- 符合项目提交消息规范

---

## 权限

权限控制 Claude 可以运行哪些命令和编辑哪些文件。配置最宽松的权限时请谨慎。

权限语法遵循 glob 模式：
- `Bash(npm run *)` — 允许以 `npm run` 开头的命令
- `Edit(/docs/**)` — 允许编辑 `/docs` 目录下的任何文件
- `Read(~/.ssh/**)` — 允许读取 `~/.ssh` 目录下的任何文件

### 权限设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `permissions.allow` | array | `[]` | 允许的工具模式列表 |
| `permissions.deny` | array | `[]` | 拒绝的工具模式列表（最高优先级，无法覆盖）|
| `permissions.defaultMode` | string | `"ask"` | 默认权限模式：`"ask"`、`"acceptEdits"`、`"plan"` 或 `"bypassPermissions"` |
| `permissions.additionalDirectories` | array | `[]` | 允许访问的额外目录（Claude Code 工作目录之外）|

### 权限模式

| 模式 | 行为 |
|------|------|
| `ask` | 每次操作都请求批准（默认）|
| `acceptEdits` | 自动接受文件编辑，但其他操作仍需批准 |
| `plan` | 进入计划模式，只读分析 |
| `bypassPermissions` | 自动批准所有操作（谨慎使用）|

### 权限示例

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git *)",
      "Edit(/docs/**)",
      "Read(~/.config/**)"
    ],
    "deny": [
      "Bash(rm -rf /*)",
      "Edit(/etc/**)"
    ],
    "additionalDirectories": ["~/projects/shared"]
  }
}
```

---

## 钩子

钩子允许在特定事件时运行自定义脚本。

### 钩子事件

| 事件 | 触发时机 |
|------|----------|
| `PreToolUse` | 工具使用前 |
| `PostToolUse` | 工具使用后 |
| `Notification` | 发送通知时 |
| `Stop` | 会话停止时 |
| `SessionStart` | 会话开始时 |
| `SessionEnd` | 会话结束时 |
| `PreCompact` | 压缩前 |
| `PostCompact` | 压缩后 |

### 钩子配置

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `hooks.PreToolUse` | array | 工具使用前的钩子 |
| `hooks.PostToolUse` | array | 工具使用后的钩子 |
| `hooks.Stop` | array | 会话停止时的钩子 |

### 钩子示例

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write ${file_path}"
          }
        ]
      }
    ],
    "Stop": [
      {
        "type": "command",
        "command": "echo 'Session ended' >> ~/.claude/session.log"
      }
    ]
  }
}
```

---

## MCP 服务器

Model Context Protocol (MCP) 服务器扩展 Claude Code 的能力。

### MCP 配置

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `mcpServers` | object | MCP 服务器配置对象 |

### MCP 示例

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "mcp-filesystem",
      "args": ["/path/to/allowed/dir"],
      "env": {
        "MCP_LOG_LEVEL": "debug"
      }
    },
    "postgres": {
      "command": "mcp-postgres",
      "args": ["postgresql://localhost/mydb"]
    }
  }
}
```

---

## 沙箱

沙箱隔离提供额外的安全层。

### 沙箱设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `sandbox.enabled` | boolean | `false` | 启用沙箱隔离 |
| `sandbox.network` | boolean | `true` | 允许网络访问（沙箱启用时）|
| `sandbox.paths.read` | array | `[]` | 允许读取的路径 |
| `sandbox.paths.write` | array | `[]` | 允许写入的路径 |

### 沙箱示例

```json
{
  "sandbox": {
    "enabled": true,
    "network": false,
    "paths": {
      "read": ["~/projects"],
      "write": ["~/projects/output"]
    }
  }
}
```

---

## 插件

插件是可分发的技能、代理、钩子和 MCP 服务器包。

### 插件设置

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `plugins.entries` | array | 已安装的插件列表 |
| `plugins.autoUpdate` | boolean | 自动更新插件 |
| `plugins.marketplaces` | array | 插件市场 URL 列表 |

---

## 模型配置

### 模型设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `model` | string | `"default"` | 默认模型（`haiku`、`sonnet`、`opus` 或完整 ID）|
| `effort` | string | `"auto"` | 模型努力级别（`low`、`medium`、`high`、`max`、`auto`）|
| `alwaysThinkingEnabled` | boolean | `false` | 默认启用扩展思考 |

### 模型别名

| 别名 | 完整模型 ID |
|------|-------------|
| `haiku` | `claude-3-5-haiku-20241022` |
| `sonnet` | `claude-3-5-sonnet-20241022` |
| `opus` | `claude-3-opus-20240229` |

---

## 显示与用户体验

### 显示设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `theme` | string | `"dark"` | 主题（`"dark"` 或 `"light"`）|
| `color` | string | - | 提示栏颜色（如 `"blue"`、`"green"`）|
| `outputStyle` | string | `"normal"` | 输出样式（`"normal"` 或 `"explanatory"`）|
| `spinnerVerbs` | array | - | 加载时显示的动作动词 |
| `statusLine` | object | - | 状态栏配置 |

### 状态栏配置

```json
{
  "statusLine": {
    "showContextUsage": true,
    "showModel": true,
    "showCost": true,
    "showTime": true,
    "format": "${model} | ${context} | ${cost}"
  }
}
```

---

## AWS 与云凭证

### AWS 设置

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `aws.profile` | string | AWS 配置文件名称 |
| `aws.region` | string | AWS 区域 |
| `aws.accessKeyId` | string | AWS 访问密钥 ID |
| `aws.secretAccessKey` | string | AWS 秘密访问密钥 |

---

## 环境变量（via env）

使用 `env` 字段设置环境变量，避免包装脚本。

### 环境变量示例

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-ant-...",
    "ANTHROPIC_MODEL": "claude-3-opus-20240229",
    "CLAUDE_CODE_MAX_TOKENS": "4096",
    "CLAUDE_CODE_TEMPERATURE": "0.7"
  }
}
```

### 常用环境变量

| 变量 | 描述 |
|------|-------------|
| `ANTHROPIC_API_KEY` | Anthropic API 密钥 |
| `ANTHROPIC_MODEL` | 默认模型 |
| `ANTHROPIC_BASE_URL` | API 基础 URL |
| `CLAUDE_CODE_MAX_TOKENS` | 最大输出 token 数 |
| `CLAUDE_CODE_TEMPERATURE` | 生成温度 |
| `CLAUDE_CODE_LOG_LEVEL` | 日志级别 |
| `CLAUDE_CODE_DISABLE_TELEMETRY` | 禁用遥测 |

---

## 实用命令

| 命令 | 描述 |
|------|-------------|
| `/config` | 打开设置界面 |
| `/permissions` | 查看或更新权限 |
| `/model` | 切换模型 |
| `/effort` | 设置努力级别 |
| `/status` | 查看状态 |
| `/doctor` | 检查安装健康状况 |

---

## 来源

- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)
- [Claude Code 权限文档](https://code.claude.com/docs/en/permissions)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
