---
description: 跟踪 Claude Code Skills 报告变更并发现需要更新的内容
argument-hint: [number of versions to check, default 10]
---

# 工作流变更日志 — Skills 报告

你是 claude-code-best-practice 项目的协调员。你的任务是启动一个研究代理，等待其结果，并展示一份关于 **Skills 参考**报告 (`best-practice/claude-skills.md`) 漂移情况的报告。

此工作流检查**两种类型的漂移**：
1. **Frontmatter 字段** — 官方文档中新增或移除的任何字段
2. **官方内置 Skills** — 新增或移除的任何内置 skill

**要检查的版本数：** `$ARGUMENTS`（默认：10，如果为空或非数字）

这是一个**读取-然后报告**的工作流。启动代理，合并发现，并生成报告。只有在用户批准后才采取行动。

---

## 阶段 1：启动研究代理

使用以下提示生成 `workflow-claude-skills-agent`：

> 研究 claude-code-best-practice 项目的 skills 报告漂移情况。检查最近 $ARGUMENTS 个版本（默认：10）。
>
> 获取以下 2 个外部来源：
> 1. Skills 参考：https://code.claude.com/docs/en/skills
> 2. 变更日志：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告（`best-practice/claude-skills.md`）。
>
> 检查以下两项内容：
> 1. **Frontmatter 字段**：将官方文档支持的 skill frontmatter 字段与报告的 Frontmatter 字段表进行对比。标记任何新增或移除的字段。
> 2. **官方内置 Skills**：将官方文档的内置 skills 列表（以及变更日志中提到的任何新内置 skills）与报告的官方 skills 表进行对比。标记任何新增或移除的 skills。

---

## 阶段 2：读取之前的变更日志条目

**在代理运行期间**，读取 `changelog/best-practice/claude-skills/changelog.md` 获取最近 25 条条目。解析优先级操作以识别：
- **重复项** — 之前出现过且仍未解决的问题
- **新增项** — 首次出现的问题
- **已解决项** — 之前标记的问题现已修复

---

## 阶段 3：生成报告

**等待代理完成。** 生成包含以下章节的报告：

1. **Frontmatter 字段变更** — 官方文档中新增或移除的字段与我们报告的对比
2. **官方内置 Skill 变更** — 新增或移除的内置 skills 与我们表格的对比

最后以优先级排序的**操作项**汇总表结束。每个条目必须包含一个 `Status` 列，显示 `NEW`、`RECURRING (first seen: <date>)` 或 `RESOLVED`：

```
Priority Actions:
#  | Type              | Action                                | Status
1  | New Field         | Add <field> to frontmatter table      | NEW
2  | Removed Field     | Remove <field> from table             | RECURRING (first seen: <date>)
3  | New Skill         | Add <skill> to official skills table   | NEW
4  | Removed Skill     | Remove <skill> from table             | NEW
```

同时包含一个**自上次运行以来已解决**的章节，列出之前运行中不再是问题的条目。

---

## 阶段 3.5：将摘要追加到变更日志

**此阶段是强制性的 — 必须在向用户展示报告之前执行。**

读取现有的 `changelog/best-practice/claude-skills/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一个新条目。条目格式必须如下：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**状态格式 — 必须使用以下三种格式之一：**
- `COMPLETE (reason)` — 操作已执行并成功解决
- `INVALID (reason)` — 发现不正确、不适用或属于有意设计
- `ON HOLD (reason)` — 操作已推迟，等待外部依赖或用户决定

`(reason)` 是必填项，必须简要说明执行了什么操作或原因。

**追加规则：**
- 始终追加 — 永远不要覆盖或替换之前的条目
- 日期和时间是命令在巴基斯坦标准时间（PKT，UTC+5）执行时的时间；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本来自代理的发现
- 如果 `changelog/best-practice/claude-skills/changelog.md` 不存在或为空，请先创建它并包含状态图例表（参见文件顶部），然后添加第一条条目
- 每条条目用 `---` 分隔
- **仅包含优先级为 HIGH、MEDIUM 或 LOW 的条目** — 省略 NONE 优先级的条目

---

## 阶段 3.6：更新"最后更新"徽章

**此阶段是强制性的 — 必须在阶段 3.5 之后、向用户展示报告之前立即执行。**

更新 `best-practice/claude-skills.md` 顶部的"最后更新"徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码（空格转为 `%20`，逗号转为 `%2C`），并替换徽章中的日期部分。如果 Claude Code 版本已更改，也请更新徽章中的版本。

**不要将徽章更新作为操作项记录在变更日志或报告中。** 徽章同步是每次运行的例行工作，不属于发现项。

---

## 阶段 4：提议采取行动

在展示报告之后（并确认变更日志和徽章均已更新），询问用户：

1. **执行所有操作** — 应用所有更改
2. **执行特定操作** — 用户选择要执行的编号
3. **仅保存报告** — 不做任何更改

执行时：
- **新增字段**：添加到 Frontmatter 字段表中，包含正确的类型、必填状态和来自官方文档的描述
- **移除字段**：在移除前与用户确认
- **新增 Skills**：添加到官方 skills 表中，包含正确的编号、skill 名称和描述
- **移除 Skills**：在移除前与用户确认
- 任何添加或移除后，更新 `## Frontmatter Fields (N)` 和 `## ![Official](...) **(N)**` 标题中的计数

---

## 关键规则

1. **绝不猜测**版本或日期 — 使用代理的数据
2. **交叉验证字段数量** — 报告的字段数量必须与官方文档匹配
3. **交叉验证 skill 数量** — 报告的 skill 数量必须与官方文档匹配
4. **不要自动执行** — 始终先展示报告
5. **始终追加到变更日志** — 阶段 3.5 是强制性的。永远不要跳过它。永远不要覆盖之前的条目。
6. **始终更新"最后更新"徽章** — 阶段 3.6 是强制性的。永远不要跳过它。
7. **与之前的运行对比** — 读取变更日志中最近 25 条条目，并将每个操作项标记为 NEW、RECURRING 或 RESOLVED。
8. **区分内置和可安装** — 仅跟踪随 Claude Code 一起发布（内置）的 skills。不要跟踪来自官方 Skills 仓库（github.com/anthropics/skills）的 skills — 那些是可安装的，不是内置的。
