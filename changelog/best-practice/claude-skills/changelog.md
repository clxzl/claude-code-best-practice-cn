# Skills 报告变更日志

**状态图例：**

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE（原因）` | 操作已执行并成功解决 |
| ❌ `INVALID（原因）` | 发现不正确、不适用或属于有意设计 |
| ✋ `ON HOLD（原因）` | 操作已推迟 — 等待外部依赖或用户决定 |

---

## [2026-03-13 04:22 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 多余的内置 Skill | `keybindings-help` 存在于本地报告中但不在官方文档内置 Skill 列表中 — 调查是移除还是保留 | ✅ COMPLETE（从内置 Skill 表中移除 — 它是此仓库中的本地自定义 Skill，不是官方内置 Skill；`/keybindings` 是内置 CLI 命令） |

---

## [2026-03-15 12:49 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 低 | 字段准确性 | `name` 字段的"必需"列在本地报告中显示为"推荐"，但官方文档现在列为"否"（可选）— 更新以匹配 | ✅ COMPLETE（将 `name` 的"必需"从"推荐"更新为"否"以匹配官方文档） |

---

## [2026-03-17 12:42 PM PKT] Claude Code v2.1.77

未检测到漂移 — frontmatter 字段（10 个）和内置 Skill（5 个）与官方文档完全同步。

---

## [2026-03-18 11:38 PM PKT] Claude Code v2.1.78

未检测到漂移 — frontmatter 字段（10 个）和内置 Skill（5 个）与官方文档完全同步。

---

## [2026-03-19 11:54 AM PKT] Claude Code v2.1.79

未检测到漂移 — frontmatter 字段（10 个）和内置 Skill（5 个）与官方文档完全同步。

---

## [2026-03-20 08:32 AM PKT] Claude Code v2.1.80

未检测到漂移 — frontmatter 字段（10 个）和内置 Skill（5 个）与官方文档完全同步。

---

## [2026-03-21 09:07 PM PKT] Claude Code v2.1.81

未检测到漂移 — frontmatter 字段（11 个）和内置 Skill（5 个）与官方文档完全同步。

---

## [2026-03-23 09:48 PM PKT] Claude Code v2.1.81

未检测到漂移 — frontmatter 字段（11 个）和内置 Skill（5 个）与官方文档完全同步。

---

## [2026-03-25 08:06 PM PKT] Claude Code v2.1.83

未检测到漂移 — frontmatter 字段（11 个）和内置 Skill（5 个）与官方文档完全同步。

---

## [2026-03-26 12:59 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新字段 | 向 frontmatter 表添加 `shell` 字段 — 接受 `bash`（默认）或 `powershell`，控制 Skill 内容中 `!command` 块的 shell | ✅ COMPLETE（添加到 frontmatter 表，计数从 11 更新为 12） |

---

## [2026-03-27 06:25 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新字段 | 向 frontmatter 表添加 `paths` 字段 — 接受 glob 模式（字符串或 YAML 列表）限制 Skill 何时自动激活 | ✅ COMPLETE（添加到 frontmatter 表，计数从 12 更新为 13） |
