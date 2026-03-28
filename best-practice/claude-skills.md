# Skills 最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2027%2C%202026%206%3A25%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-skills-implementation.md)

Claude Code 技能（skills）—— frontmatter 字段和官方内置技能。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段 (13)

| 字段 | 类型 | 必需 | 描述 |
|-------|------|----------|-------------|
| `name` | string | 否 | 显示名称和 `/slash-command` 标识符。如省略则默认为目录名 |
| `description` | string | 推荐 | 技能功能描述。显示在自动补全中，供 Claude 自动发现使用 |
| `argument-hint` | string | 否 | 自动补全时显示的提示（如 `[issue-number]`、`[filename]`） |
| `disable-model-invocation` | boolean | 否 | 设为 `true` 阻止 Claude 自动调用此技能 |
| `user-invocable` | boolean | 否 | 设为 `false` 从 `/` 菜单隐藏 —— 技能仅作为背景知识，供代理预加载使用 |
| `allowed-tools` | string | 否 | 此技能激活时无需权限提示即可使用的工具 |
| `model` | string | 否 | 此技能运行时使用的模型（如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | 否 | 调用时覆盖模型努力级别（`low`、`medium`、`high`、`max`） |
| `context` | string | 否 | 设为 `fork` 在独立子代理上下文中运行技能 |
| `agent` | string | 否 | 当设置 `context: fork` 时的子代理类型（默认：`general-purpose`） |
| `hooks` | object | 否 | 作用于技能的生命周期钩子 |
| `paths` | string/list | 否 | 限制技能自动激活的 glob 模式。接受逗号分隔字符串或 YAML 列表 —— Claude 仅在处理匹配文件时加载技能 |
| `shell` | string | 否 | `` !`command` `` 块的 shell —— `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |

---

## ![官方](../!/tags/official.svg) 官方内置技能 **(5)**

| # | 技能 | 描述 |
|---|-------|-------------|
| 1 | `simplify` | 审查更改的代码以优化复用、质量和效率 —— 重构以消除重复 |
| 2 | `batch` | 批量跨多个文件运行命令 |
| 3 | `debug` | 调试失败的命令或代码问题 |
| 4 | `loop` | 按固定间隔循环运行提示或斜杠命令（最长 3 天） |
| 5 | `claude-api` | 使用 Claude API 或 Anthropic SDK 构建应用 —— 在 `anthropic` / `@anthropic-ai/sdk` 导入时触发 |

另见：[官方技能仓库](https://github.com/anthropics/skills/tree/main/skills) 获取社区维护的可安装技能。

---

## 来源

- [Claude Code 技能 — 文档](https://code.claude.com/docs/en/skills)
- [Monorepo 中的技能发现](../reports/claude-skills-for-larger-mono-repos.md)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
