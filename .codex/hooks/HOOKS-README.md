# HOOKS-README
包含 Codex CLI Hooks 的所有详情、脚本和说明。

## Hook 事件概览

Codex CLI 提供了 **3 个 Hook**，分布在两个配置系统中：

| # | Hook | 事件类型 | 配置文件 | 描述 |
|:-:|------|------------|-------------|-------------|
| 1 | `agent-turn-complete` | `agent-turn-complete` | `config.toml` | 当 Codex Agent 完成响应时运行 |
| 2 | `SessionStart` | `SessionStart` | `hooks.json` | 在会话启动时运行一次——注入上下文并播放声音 |
| 3 | `Stop` | `stop` | `hooks.json` | 在会话结束时运行——播放声音 |

> Hook 2 和 3 需要 **Codex CLI v0.114.0+** 并启用 hooks 引擎：
> ```bash
> codex -c features.codex_hooks=true
> ```

### Hook 如何被调用

**agent-turn-complete hook**（config.toml）—— JSON 作为 CLI 参数传递：
```
python3 .codex/hooks/scripts/hooks.py '{"type":"agent-turn-complete"}'
```

**SessionStart / Stop hooks**（hooks.json）—— 使用 `--hook` 标志调用：
```
python3 .codex/hooks/scripts/hooks.py --hook SessionStart
python3 .codex/hooks/scripts/hooks.py --hook Stop
```

### SessionStart 上下文注入

SessionStart Hook 将上下文输出到 **stdout**，直接馈送到模型的上下文窗口中。包括：
- 当前日期/时间
- Git 分支名称
- 工作树状态（干净或有未提交的更改）
- 工作目录路径

> **与 Claude Code 的关键区别：** Claude Code 通过 **stdin** 传递 JSON，而 Codex CLI 将其作为 **CLI 参数** 传递。新的 hooks.json 系统使用 `--hook` 标志。

## 前提条件

在使用 Hooks 之前，请确保您的系统上已安装 **Python 3**。

### 所需软件

#### 所有平台（Windows、macOS、Linux）
- **Python 3**：运行 hook 脚本所必需
- 验证安装：`python3 --version`

**安装说明：**
- **Windows**：从 [python.org](https://www.python.org/downloads/) 下载或通过 `winget install Python.Python.3` 安装
- **macOS**：通过 `brew install python3` 安装（需要 [Homebrew](https://brew.sh/)）
- **Linux**：通过 `sudo apt install python3`（Ubuntu/Debian）或 `sudo yum install python3`（RHEL/CentOS）安装

### 音频播放器（自动检测）

Hook 脚本会自动检测并使用适合您平台的音频播放器：

- **macOS**：使用 `afplay`（内置，无需安装）
- **Linux**：使用 `pulseaudio-utils` 中的 `paplay`——通过 `sudo apt install pulseaudio-utils` 安装
- **Windows**：使用内置的 `winsound` 模块（Python 自带）

### 配置文件

有 **三个** 配置文件：

1. **`.codex/config.toml`** —— 注册 `agent-turn-complete` hook（通过 `notify`）
2. **`.codex/hooks.json`** —— 注册 `SessionStart` 和 `Stop` hooks（v0.114.0+）
3. **`.codex/hooks/config/hooks-config.json`** —— 启用/禁用各个 hooks 和日志记录

#### config.toml（agent-turn-complete hook）

```toml
notify = ["python3", ".codex/hooks/scripts/hooks.py"]
```

#### hooks.json（SessionStart + Stop hooks）

```json
{
  "hooks": {
    "SessionStart": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook SessionStart",
        "statusMessage": "正在初始化会话钩子...",
        "timeout": 10
      }
    ],
    "Stop": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook Stop",
        "statusMessage": "正在运行会话停止钩子...",
        "timeout": 10
      }
    ]
  }
}
```

## 配置 Hooks（启用/禁用）

### 禁用单个 Hooks

编辑 `.codex/hooks/config/hooks-config.json`：
```json
{
  "disableAgentTurnCompleteHook": false,
  "disableSessionStartHook": false,
  "disableStopHook": false,
  "disableLogging": true
}
```

**配置选项：**
- `disableAgentTurnCompleteHook`：设置为 `true` 以禁用 agent-turn-complete 声音
- `disableSessionStartHook`：设置为 `true` 以禁用会话启动的上下文注入和声音
- `disableStopHook`：设置为 `true` 以禁用会话停止声音
- `disableLogging`：设置为 `true` 以禁用将 hook 事件记录到 `.codex/hooks/logs/hooks-log.jsonl`

### 配置回退机制

有两个配置文件：

1. **`.codex/hooks/config/hooks-config.json`** - 提交到 git 的共享/默认配置
2. **`.codex/hooks/config/hooks-config.local.json`** - 您的个人覆盖（git 忽略）

本地配置文件（`.local.json`）优先于共享配置，允许每位开发者自定义其 hook 行为而不影响团队。

#### 本地配置（个人覆盖）

创建或编辑 `.codex/hooks/config/hooks-config.local.json` 以设置个人偏好：

```json
{
  "disableAgentTurnCompleteHook": true,
  "disableSessionStartHook": false,
  "disableStopHook": true,
  "disableLogging": true
}
```

### 日志记录

当日志记录启用时（`"disableLogging": false`），hook 事件以 JSON Lines 格式记录到 `.codex/hooks/logs/hooks-log.jsonl`。每个条目包含从 Codex CLI 接收的完整 JSON 负载。

## 测试

运行测试套件：
```bash
python3 -m unittest tests.test_hooks -v
```

## 未来可扩展性

本项目可通过以下方式扩展：

1. 在 `hooks.py` 的 `HOOK_SOUND_MAP` 中添加新条目
2. 在 `.codex/hooks/sounds/` 中添加对应的声音文件
3. 在 `hooks-config.json` 中添加切换键
4. 在 `hooks.json` 中添加新的 hook 条目
