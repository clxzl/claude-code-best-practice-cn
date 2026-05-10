---
description: 跟踪 Claude Code commands 报告的变更并找出需要更新的内容
argument-hint: [要检查的版本数量，默认 10]
---

# 工作流变更日志 — Commands 报告

你是 claude-code-best-practice 项目的协调器。你的工作是启动研究 Agent、等待其结果，并提交一份关于 **Commands 参考**报告（`best-practice/claude-commands.md`）中漂移的报告。

此工作流恰好检查**两种类型的漂移**：
1. **Frontmatter 字段** — 官方文档中新增或移除的任何字段
2. **官方命令** — 新增或移除的任何内置 slash 命令

**检查版本数：** `$ARGUMENTS`（如果为空或非数字则默认为 10）

这是一个**先读后报告**的工作流。启动 Agent、合并发现并生成报告。只有在用户批准后才采取行动。

---

## 阶段 1：启动研究 Agent

生成 `workflow-claude-commands-agent`，使用以下提示：

> 研究 claude-code-best-practice 项目的 commands 报告漂移。检查最近 $ARGUMENTS 个版本（默认：10）。
>
> 获取这 2 个外部来源：
> 1. Slash 命令参考：https://code.claude.com/docs/en/slash-commands
> 2. 变更日志：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告（`best-practice/claude-commands.md`）。
>
> 恰好检查两件事：
> 1. **Frontmatter 字段**：将官方文档支持的命令 frontmatter 字段与报告的 Frontmatter 字段表进行比较。标记任何新增或移除的字段。
> 2. **官方命令**：将官方文档的内置 slash 命令列表与报告的官方命令表进行比较。标记任何新增或移除的命令。同时检查任何命令的标签或描述是否已更改。

---

## 阶段 2：读取之前的变更日志条目

**在 Agent 运行期间**，读取 `changelog/best-practice/claude-commands/changelog.md` 获取最近 25 个条目。解析优先级操作以识别：
- **重复项** — 之前出现过且仍未解决的问题
- **新项** — 首次出现的问题
- **已解决项** — 之前标记但现已修复的问题

---

## 阶段 3：生成报告

**等待 Agent 完成。** 生成包含以下部分的报告：

1. **Frontmatter 字段变更** — 官方文档与我们的报告中新增或移除的字段
2. **官方命令变更** — 与我们表中相比新增或移除的内置 slash 命令

最后以带优先级的**操作项**摘要表结束。每个项必须包含一个 `Status` 列，显示 `NEW`、`RECURRING (first seen: <date>)` 或 `RESOLVED`：

```
Priority Actions:
#  | Type              | Action                                | Status
1  | New Field         | Add <field> to frontmatter table      | NEW
2  | Removed Field     | Remove <field> from table             | RECURRING (first seen: <date>)
3  | New Command       | Add <command> to official table        | NEW
4  | Removed Command   | Remove <command> from table           | NEW
5  | Changed Tag       | Update <command> tag from X to Y      | NEW
```

同时包含一个**上次运行以来已解决项**部分，列出之前运行中已不再是问题的项。

---

## 阶段 3.5：追加摘要到变更日志

**此阶段是强制性的 — 在向用户展示报告前必须始终执行。**

读取现有的 `changelog/best-practice/claude-commands/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一个新条目。条目格式必须如下：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**状态格式 — 必须使用以下三种格式之一：**
- `COMPLETE (reason)` — 已采取行动并成功解决
- `INVALID (reason)` — 发现不正确、不适用或为预期行为
- `ON HOLD (reason)` — 操作已推迟，等待外部依赖或用户决定

`(reason)` 是强制性的，必须简要说明做了什么或原因。

**追加规则：**
- 始终追加 — 绝不覆盖或替换之前的条目
- 日期和时间是命令在巴基斯坦标准时间（PKT, UTC+5）执行的时间；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本来自 Agent 发现
- 如果 `changelog/best-practice/claude-commands/changelog.md` 不存在或为空，创建它并包含状态图例表（见文件顶部）然后添加第一个条目
- 每个条目用 `---` 分隔
- **只包含 HIGH、MEDIUM 或 LOW 优先级的项** — 省略 NONE 优先级的项

---

## 阶段 3.6：更新 Last Updated 徽章

**此阶段是强制性的 — 在阶段 3.5 之后、向用户展示报告之前必须立即执行。**

更新 `best-practice/claude-commands.md` 顶部的"Last Updated"徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码（空格转为 `%20`，逗号转为 `%2C`），替换徽章中的日期部分。如果 Claude Code 版本已更改，也更新徽章中的版本号。

**不要将徽章更新作为操作项记录在变更日志或报告中。** 徽章同步是每次运行的例行部分，不是发现。

---

## 阶段 4：提供执行选项

在展示报告后（并确认变更日志和徽章已更新），询问用户：

1. **执行所有操作** — 应用所有更改
2. **执行特定操作** — 用户选择要执行哪些编号的操作
3. **仅保存报告** — 不做更改

执行时：
- **新字段**：添加到 Frontmatter 字段表中，包含正确的类型、必需状态和官方文档中的描述
- **移除字段**：在移除前与用户确认
- **新命令**：添加到官方命令表中，包含正确的 #、命令、标签和描述。插入到正确的标签组中（表按标签排序）
- **移除命令**：在移除前与用户确认
- **变更标签**：更新命令的标签并在需要时重新排序
- 任何添加或移除后，更新 `## Frontmatter Fields (N)` 和 `## ![Official](...) **(N)**` 标题中的计数

---

## 关键规则

1. **绝不猜测**版本或日期 — 使用来自 Agent 的数据
2. **交叉引用字段计数** — 报告字段计数必须与官方文档匹配
3. **交叉引用命令计数** — 报告命令计数必须与官方文档匹配
4. **不要自动执行** — 始终先展示报告
5. **始终追加到变更日志** — 阶段 3.5 是强制性的。绝不跳过。绝不覆盖之前的条目。
6. **始终更新 Last Updated 徽章** — 阶段 3.6 是强制性的。绝不跳过。
7. **与之前的运行比较** — 读取变更日志中的最近 25 个条目，将每个操作项标记为 NEW、RECURRING 或 RESOLVED。
8. **保持标签排序顺序** — 官方命令表按标签（字母顺序）排序，然后每个标签组内按命令名称排序。在添加或移除命令时保持此排序。
