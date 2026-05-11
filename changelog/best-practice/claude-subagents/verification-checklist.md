# 验证清单 — Subagents 报告

规则随时间累积。每次 workflow-changelog 运行**必须**在指定深度执行所有规则。当发现现有规则应该捕获但未捕获（或不存在或深度不够）的新类型漂移时，在此追加新规则。

## 深度级别

| 深度 | 含义 | 示例 |
|-------|---------|---------|
| `exists` | 检查章节/表格/文件是否存在 | "报告是否有 Memory Scopes 表？" |
| `presence-check` | 检查特定项目是否在场或缺失 | "`color` 字段是否在 Frontmatter Fields 表中？" |
| `content-match` | 逐字比较实际值与来源 | "`model` 字段描述是否匹配官方文档？" |
| `field-level` | 验证每个单独字段是否已涵盖 | "官方文档中的每个 frontmatter 字段是否出现在表中？" |
| `cross-file` | 相同值必须跨多个文件一致 | "CLAUDE.md agent 章节是否匹配报告的字段列表？" |

---

## 1. Frontmatter Fields 表

验证 Frontmatter Fields 表与官方文档一致的规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 1A | 字段完整性 | 对于官方文档中的每个 agent frontmatter 字段，验证它出现在报告的 Frontmatter Fields 表中 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 确保不遗漏新字段 |
| 1B | 字段类型 | 对于表中的每个字段，验证 Type 列匹配官方文档 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 类型不匹配会导致用户困惑 |
| 1C | 必需状态 | 对于每个字段，验证 Required 列匹配官方文档 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 错误的必需状态会导致 agent 故障 |
| 1D | 字段描述 | 对于每个字段，验证 Description 列准确反映官方文档行为 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 过时描述误导用户 |

---

## 2. Memory Scopes

验证 Memory Scopes 表的规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 2A | 范围完整性 | 验证官方文档中的所有 memory scope 都出现在 Memory Scopes 表中 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 可能会添加新 scope |
| 2B | 存储位置 | 对于每个 scope，验证 Storage Location 列匹配官方文档 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 错误路径会导致数据丢失 |

---

## 3. 示例

验证示例准确性的规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 3A | 最小示例 | 验证最小示例仅使用必需字段且语法有效 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 最小示例应保持最小 |
| 3B | 完整示例 | 验证完整示例展示所有可用的 frontmatter 字段 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 完整示例必须展示每个字段 |

---

## 4. 范围与优先级

验证范围和优先级信息的规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 4A | 优先级顺序 | 验证 Scope and Priority 表按正确的优先级顺序列出所有 agent 位置 | content-match | sub-agents 参考页面 + CLI 参考页面 | 2026-02-28 | 初始清单 — 错误的优先级顺序会导致解析错误 |
| 4B | 调用方法 | 验证调用方法表列出 CLI 参考和 sub-agents 文档中的所有调用方法，包括 `--agent`（单数）、`--agents`（复数）、`/agents`、`claude agents`、Agent 工具和 agent 恢复 | field-level | CLI 参考页面 + sub-agents 参考页面 | 2026-03-07 | 调用表中缺少 `--agent` CLI 标志 — 它是运行 Claude 作为特定 agent 的独立调用方法 |

---

## 5. 跨文件一致性

验证报告与其他仓库文件之间一致性的规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 5A | CLAUDE.md 同步 | 验证 CLAUDE.md 的 Subagent Definition Structure 章节列出与报告的 Frontmatter Fields 表相同的字段 | cross-file | CLAUDE.md vs 报告 | 2026-02-28 | 初始清单 — CLAUDE.md 可能与报告产生漂移 |

---

## 6. 流程

关于工作流验证流程本身的元规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 6A | 来源可信度保护 | 仅在被官方来源（sub-agents 参考页面、CLI 参考页面、GitHub changelog）确认时标记为漂移。第三方博客来源可能过时或错误 — 仅用于线索，在标记之前对照官方文档验证 | content-match | 仅官方文档 | 2026-02-28 | 采用自 hooks 工作流 — 防止博客来源的误报 |

---

## 7. Agent 表

验证 Official Claude Agents 和 Agents in This Repository 表的规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 7A | 内置 Agent 完整性 | 验证"Official Claude Agents"表列出所有内置 agent 类型，包含正确的模型、工具和描述 | field-level | sub-agents 参考页面 + changelog | 2026-02-28 | 报告只有 5 个内置 agent 中的 3 个 — `claude-code-guide` 和 `statusline-setup` 缺失 |
| 7B | 仓库 Agent 完整性 | 扫描 `.claude/agents/**/*.md` 并验证每个 agent 文件出现在"Agents in This Repository"表中，包含正确的模型、颜色、工具、skill 和 memory 列 | field-level | `.claude/agents/**/*.md` 文件 frontmatter | 2026-02-28 | 仓库 agent 是手动维护的 — 添加到仓库的新 agent 未反映在报告中 |
| 7C | 仓库 Agent 链接 | 验证"Agents in This Repository"表中每个 agent 名称都有可点击的链接，解析到正确的 `.md` 文件 | exists | 从 `best-practice/` 解析的文件路径 | 2026-02-28 | Agent 名称已设为可点击 — 文件移动后链接必须保持有效 |

---

## 8. 超链接

验证报告中所有超链接有效的规则。

| # | 类别 | 检查 | 深度 | 比较来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 8A | 本地文件链接 | 验证所有相对文件链接（例如 `../.claude/agents/weather-agent.md`）解析到存在的文件 | exists | 本地文件系统 | 2026-02-28 | 文件移动（reports/ → best-practice/）破坏了相对链接 — 必须捕获未来的损坏 |
| 8B | 外部 URL 链接 | 验证所有外部 URL（例如 `https://code.claude.com/docs/en/sub-agents`）返回有效页面 | exists | HTTP 响应 | 2026-02-28 | 外部文档页面可能被重构或移除 — 必须在每次运行时验证 |
| 8C | 跨文件引用链接 | 验证指向其他报告文件的链接（例如 `../reports/claude-agent-memory.md`）解析到存在的文件 | exists | 本地文件系统 | 2026-02-28 | 报告可能被移动或重命名 — 交叉引用必须保持同步 |
