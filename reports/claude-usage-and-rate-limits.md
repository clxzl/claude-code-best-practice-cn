# Claude Code：使用量、速率限制和额外使用量

理解 Claude Code 中使用量限制的工作方式，以及达到限制后如何继续工作。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

订阅计划（Pro、Max 5x、Max 20x）上的 Claude Code 有使用量限制，在滚动窗口内重置。三个内置斜杠命令帮助你监控和管理使用量：

| 命令 | 描述 | 可用对象 |
|------|------|---------|
| `/usage` | 检查计划限制和速率限制状态 | Pro、Max 5x、Max 20x |
| `/extra-usage` | 达到限制时配置按需付费溢出 | Pro、Max 5x、Max 20x |
| `/cost` | 显示当前会话的 Token 使用量和花费 | API 密钥用户 |

---

## `/usage` — 检查你的限制

显示你当前计划的使用量限制和速率限制状态。可用于检查在达到限制前还剩多少容量。

---

## `/extra-usage` — 超过限制后继续工作

`/extra-usage` 命令配置**按需付费溢出计费**，这样当你达到计划的速率限制时，Claude Code 会继续无缝工作，而不是阻止你。

### 工作原理

1. 你达到计划的速率限制（限制每 5 小时重置一次）
2. 如果启用了额外使用量且有可用资金，Claude Code 继续无中断工作
3. 溢出 Token 按**标准 API 费率**计费，与订阅费分开

### 设置方法

CLI 中的 `/extra-usage` 命令会引导你完成配置。你也可以在 claude.ai 的网页上配置，路径为 **设置 > 使用量**：

1. 启用额外使用量
2. 添加支付方式
3. 设置**月度消费上限**（或选择无限制）
4. 可选：添加**预充值资金**，当余额低于阈值时自动充值

### 关键详情

| 详情 | 值 |
|------|-----|
| 每日兑换限额 | $2,000/天 |
| 计费方式 | 与订阅分开，按标准 API 费率 |
| 限制重置窗口 | 每 5 小时 |

### 已知问题

截至 2026 年 2 月，`/extra-usage` CLI 命令[未记录文档](https://github.com/anthropics/claude-code/issues/12396)，可能会打开登录窗口而没有明确的配置选项。目前通过 **claude.ai 网页界面**配置是更可靠的途径。

---

## `/cost` — 会话花费（API 用户）

对于使用 API 密钥认证的用户（非订阅计划），`/cost` 显示：

- 当前会话的总费用
- API 持续时间和实际时间
- Token 使用量明细
- 已做的代码更改

此命令与 Pro/Max 订阅用户无关。

---

## 快速模式和额外使用量

快速模式（`/fast`）使用 Claude Opus 4.6 并提供更快的输出。它与额外使用量有特殊的计费关系：

- 快速模式的使用量**从第一个 Token 起就计入额外使用量**
- 即使你的订阅计划中还有剩余使用量，此规则也适用
- 快速模式不消耗计划包含的速率限制

这意味着你需要启用并充值额外使用量才能使用 `/fast`。

---

## CLI 启动标志

两个启动标志与使用量预算相关（仅限 API 密钥用户，打印模式）：

| 标志 | 描述 |
|------|------|
| `--max-budget-usd <AMOUNT>` | API 调用的最大美元金额，达到后停止 |
| `--max-turns <NUMBER>` | 限制 Agent 轮次数量 |

参见 [CLI 启动标志参考](claude-cli-startup-flags.md) 获取完整列表。

---

## 参考来源

- [付费 Claude 计划的额外使用量 — Claude 帮助中心](https://support.claude.com/en/articles/12429409-extra-usage-for-paid-claude-plans)
- [使用 Claude Code 的 Pro 或 Max 计划 — Claude 帮助中心](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)
- [/extra-usage 斜杠命令未记录文档 — GitHub Issue #12396](https://github.com/anthropics/claude-code/issues/12396)
- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
