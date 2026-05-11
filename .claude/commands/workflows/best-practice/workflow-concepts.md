---
description: 使用最新的 Claude Code 功能和概念更新 README CONCEPTS 部分
argument-hint: [要检查的更新日志版本数量，默认 10]
---

# 工作流更新日志 — README 概念

你是 claude-code-best-practice 项目的协调员。你的工作是并行启动两个研究智能体，等待它们的结果，合并发现，并呈现一份关于 **README CONCEPTS 部分**（`README.md`）偏差的统一报告。

**要检查的版本数：** `$ARGUMENTS`（如果为空或非数字，默认为 10）

这是一个**先读后报告**的工作流。启动智能体，合并结果，生成报告。只有在用户批准后才采取行动。

---

## 阶段 0：并行启动两个智能体

**立即**使用 Task 工具在**同一条消息**中生成两个智能体（并行启动）：

### 智能体 1：workflow-concepts-agent

使用 `subagent_type: "workflow-concepts-agent"` 生成。给它以下提示：

> 研究 claude-code-best-practice 项目中 README CONCEPTS 部分的偏差。检查最近 $ARGUMENTS 个版本（默认：10）。
>
> 获取以下 3 个外部来源：
> 1. Claude Code 文档索引：https://code.claude.com/docs/en
> 2. Claude Code 更新日志：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
> 3. Claude Code 功能概览：https://code.claude.com/docs/en/overview
>
> 然后读取本地 README.md（特别是 CONCEPTS 表格）、CLAUDE.md 和 `reports/claude-global-vs-project-settings.md`。分析官方文档列出的 Claude Code 概念/功能与我们的 README CONCEPTS 表格记录之间的差异。返回结构化的发现报告，涵盖缺失概念、变更概念、废弃概念、URL 准确性、描述准确性和徽章准确性。

### 智能体 2：claude-code-guide

使用 `subagent_type: "claude-code-guide"` 生成。给它以下提示：

> 研究最新的 Claude Code 功能和概念。我需要你找到所有应该被记录的 Claude Code 概念/功能的完整列表。对每个概念/功能，提供：
> 1. 官方功能名称
> 2. 官方文档 URL
> 3. 文件系统位置（例如 `.claude/commands/`、`~/.claude/teams/`）
> 4. 简要描述（一行）
> 5. 引入时间（版本/日期，如果已知）
>
> 特别检查以下可能缺失的概念：
> - **Worktrees** — 用于并行开发的 git worktree 隔离
> - **Agent Teams** — 多智能体协调
> - **Tasks** — 跨会话的持久任务列表
> - **Auto Memory** — Claude 自己编写的项目学习记录
> - **Keybindings** — 自定义键盘快捷键
> - **Remote Connections** — SSH、Docker、云开发
> - **IDE Integration** — VS Code、JetBrains 扩展
> - **Model Configuration** — 模型选择和路由
> - **GitHub Integration** — PR 审查、问题分类
> - 任何其他来自最新 Claude Code 版本的概念
>
> 请彻底搜索——搜索网络、获取文档，并为你找到的所有内容提供具体的版本号和细节。

两个智能体独立运行，将返回它们的发现。

---

## 阶段 0.5：读取验证检查清单

**在智能体运行期间**，如果 `changelog/best-practice/concepts/verification-checklist.md` 存在则读取它。此文件包含累积的验证规则。如果尚不存在，跳过此步骤——它将在阶段 2 中创建。

---

## 阶段 1：读取之前的更新日志条目

**在合并发现之前**，如果文件 `changelog/best-practice/concepts/changelog.md` 存在则读取它，以获取之前的更新日志条目。每个条目由 `---` 分隔。解析这些之前条目中的优先操作，以便你可以将它们与当前发现进行比较。这使你可以识别：
- **重复出现的项目** — 之前出现过且仍未解决的问题
- **新解决的项目** — 之前运行中出现但现在已修复的问题
- **新项目** — 本次运行中首次出现的问题

如果文件尚不存在，所有项目均为 `NEW`。

---

## 阶段 2：合并发现并生成报告

**等待两个智能体完成。** 一旦你获得：
- **workflow-concepts-agent 的发现** — 包含本地文件读取、外部文档获取和偏差检测的详细分析
- **claude-code-guide 的发现** — 关于最新 Claude Code 功能和概念的独立研究

交叉引用两者。专用智能体提供 CONCEPTS 特定的偏差分析，而 claude-code-guide 智能体可能会发现它遗漏的内容（例如非常近期的更改、未记录的功能或来自网络搜索的上下文）。标记两者之间的任何矛盾，交由用户解决。

**执行验证检查清单（如果存在）：** 对 `changelog/best-practice/concepts/verification-checklist.md` 中的每条规则执行检查。在报告中包含一个**验证日志**部分。

**按需更新检查清单：** 如果某个发现揭示了一种新类型的偏差，且现有检查清单规则未覆盖，则将新规则追加到 `changelog/best-practice/concepts/verification-checklist.md`。如果文件不存在则创建它。规则必须包含：类别、检查内容、深度级别、对比来源、添加日期和来源。

同时将当前发现与之前的更新日志条目（来自阶段 1）进行比较。对每个优先操作，标记为：
- `NEW` — 该问题首次出现
- `RECURRING` — 在之前运行中出现且仍未解决（包含首次出现的运行日期）
- `RESOLVED` — 在之前运行中出现但现已修复（包含解决日期）

生成包含以下部分的结构化报告：

1. **缺失概念** — 官方文档中有但 CONCEPTS 表格中缺失的功能/概念，包含：
   - 官方名称和文档 URL
   - 推荐的 Location 列值
   - 推荐的 Description 列值
   - 可直接粘贴的精确 markdown 表格行
   - 引入版本（如果已知）
2. **变更概念** — 名称、URL、位置或描述发生变化的概念
3. **废弃/移除概念** — CONCEPTS 表格中存在但官方文档中不再有的概念
4. **URL 准确性** — 每个概念的 URL 验证
5. **描述准确性** — 每个概念的描述/位置验证
6. **徽章准确性** — 徽章链接验证和缺失徽章建议
7. **claude-code-guide 智能体发现** — 专用智能体未捕获的独特见解。仅包含增加新信息的发现。标记矛盾之处。

以优先级**操作项**汇总表结束：

优先操作：
#  | 类型                | 操作                                     | 状态
1  | 缺失概念     | 向 CONCEPTS 表添加 <concept> 行         | NEW
2  | URL已变更         | 更新 <concept> 文档链接                  | NEW
3  | 描述已变更 | 更新 <concept> 描述                | 循环出现 (首次出现: <date>)
4  | 概念已弃用  | 从 CONCEPTS 表移除 <concept> 行    | NEW
5  | 徽章失效        | 修复 <concept> 的徽章链接                | NEW

同时包含一个**自上次运行以来已解决**部分，列出之前运行中不再是问题的任何项目。

---

## 阶段 2.5：将摘要追加到更新日志

**此阶段是强制性的——始终在向用户呈现报告之前执行。**

读取现有的 `changelog/best-practice/concepts/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一个新条目。如果文件不存在，创建它并包含状态图例表，然后是第一个条目。条目格式必须完全如下：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高/中/低 | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**状态格式——必须使用以下三种格式之一：**
- `COMPLETE (reason)` — 已采取行动并成功解决
- `INVALID (reason)` — 发现不正确、不适用或有意为之
- `ON HOLD (reason)` — 行动延迟，等待外部依赖或用户决定

`(reason)` 是必需的，必须简要说明已完成的操作或原因。

**追加规则：**
- 始终追加——绝不覆盖或替换之前的条目
- 日期和时间是命令在巴基斯坦标准时间（PKT，UTC+5）执行的时间；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本来自智能体发现
- 每个条目由 `---` 分隔
- **仅包含优先级为 HIGH、MEDIUM 或 LOW 的项目** — 省略 NONE 优先级的项目

---

## 阶段 2.6：更新 Last Updated 徽章

**此阶段是强制性的——始终在阶段 2.5 之后、呈现报告之前立即执行。**

更新 `README.md` 顶部（第 3 行）的 "Last Updated" 徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，URL 编码它（空格转为 `%20`，逗号转为 `%2C`），替换徽章中的日期部分。

**不要将徽章更新记录为更新日志或报告中的操作项。**

---

## 阶段 2.7：验证所有 CONCEPTS URL

**此阶段是强制性的——始终在阶段 2.6 之后、呈现报告之前执行。**

对 CONCEPTS 表格中的每个概念：

1. **外部文档 URL**（例如 `https://code.claude.com/docs/en/skills`）：使用 WebFetch 获取每个 URL 并验证它返回有效页面。标记任何失效或已移动的链接。
2. **本地徽章链接**（例如 `best-practice/claude-commands.md`）：使用 Read 工具验证文件存在。标记任何断开的链接。
3. **实现徽章链接**（例如 `.claude/commands/`）：验证路径存在。

在报告中包含**URL 验证日志**：

```
URL 验证日志:
#  | 概念       | URL类型  | URL                                           | 状态 | 备注
1  | Commands   | External | https://code.claude.com/docs/en/skills         | OK   |
2  | Commands   | Badge    | best-practice/claude-commands.md               | OK   |
3  | Sub-Agents | External | https://code.claude.com/docs/en/sub-agents     | OK   |
...
```

**如果有任何 URL 失效**，将它们作为 HIGH 优先级操作项添加。

---

## 阶段 3：提供执行选项

呈现报告（并确认更新日志已更新）后，询问用户：

1. **执行所有操作** — 添加缺失概念、更新变更概念、移除废弃概念
2. **执行特定操作** — 用户选择执行哪些编号
3. **仅保存报告** — 不做更改

执行时：
- **缺失概念**：按照现有格式向 `README.md` 中的 CONCEPTS 表格添加新行：
  ```
  | [**Name**](docs-url) | `location` | Description |
  ```
  仅在对应文件存在时添加徽章（best-practice、implemented）。
- **变更概念**：更新发生变化的特定列
- **废弃概念**：移除行之前与用户确认
- **失效 URL**：将 URL 修复为当前有效的链接
- **徽章修复**：更新徽章链接为正确的文件路径
- 保持与现有表格一致的字母或逻辑排序
- 所有操作完成后，重新验证 CONCEPTS 表格的一致性

---

## 关键规则

1. **在单条消息中并行启动两个智能体** — 绝不串行
2. **在生成报告之前等待两个智能体**
3. **绝不猜测**版本、URL 或日期——使用来自智能体的数据
4. **缺失概念是高优先级** — CONCEPTS 表格是开发者首先看到的内容
5. **验证每个 URL** — 失效链接会降低对整个项目的信任
6. **不要自动执行** — 始终先呈现报告
7. **始终追加更新日志** — 阶段 2.5 是强制性的。绝不跳过。绝不覆盖之前的条目。
8. **与之前的运行比较** — 读取更新日志中的之前条目，将每个操作项标记为 NEW、RECURRING 或 RESOLVED。
9. **如果验证检查清单存在则执行它** — 读取 verification-checklist.md 并执行每条规则。如果不存在且有需要持久化规则的发现，则创建该文件。
10. **始终更新 Last Updated 徽章** — 阶段 2.6 是强制性的。
11. **始终验证所有 CONCEPTS URL** — 阶段 2.7 是强制性的。失效 URL 是 HIGH 优先级。
12. **提供可直接粘贴的行** — 对于缺失概念，包含精确的 markdown 表格行以便复制粘贴执行。
13. **尊重现有表格格式** — 匹配现有行的列结构、徽章模式和链接样式。
