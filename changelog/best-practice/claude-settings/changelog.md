# 设置报告 — 变更历史

## 状态图例

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE (reason)` | 已采取行动并成功解决 |
| ❌ `INVALID (reason)` | 发现不正确、不适用或属预期行为 |
| ✋ `ON HOLD (reason)` | 行动已推迟——等待外部依赖或用户决定 |

---

## [2026-03-05 06:18 AM PKT] Claude Code v2.1.69

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 设置缺失 | Add 13 non-hook missing settings keys (`$schema`, `availableModels`, `fastModePerSessionOptIn`, `teammateMode`, `prefersReducedMotion`, `sandbox.filesystem.*`, `sandbox.network.allowManagedDomainsOnly`, `sandbox.enableWeakerNetworkIsolation`, `allowManagedMcpServersOnly`, `blockedMarketplaces`, `includeGitInstructions`, `pluginTrustMessage`, `fileSuggestion` table entry) | ✅ COMPLETE (已添加到报告) |
| 2 | HIGH | 环境变量缺失 | Add missing environment variables including `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING`, `CLAUDE_CODE_DISABLE_1M_CONTEXT`, `CLAUDE_CODE_ACCOUNT_UUID`, `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS`, `ENABLE_CLAUDEAI_MCP_SERVERS`, and more | ✅ COMPLETE (added 13 missing env vars to report) |
| 3 | HIGH | 努力级别默认值 | Update effort level default from "High" to "Medium" for Max/Team subscribers; add Sonnet 4.6 support (changed v2.1.68) | ✅ COMPLETE (已更新默认值并添加 Sonnet 说明) |
| 4 | MED | 设置层级 | Add managed settings via macOS plist/Windows Registry (v2.1.61/v2.1.69); document array merge behavior across scopes | ✅ COMPLETE (已添加 plist/registry 和合并说明) |
| 5 | MED | 沙盒文件系统 | Add `sandbox.filesystem.allowWrite`, `denyWrite`, `denyRead` with path prefix semantics (`//`, `~/`, `/`, `./`) | ✅ COMPLETE (已添加到沙盒表) |
| 6 | MED | 权限语法 | Add `Agent(name)` permission pattern; document `MCP(server:tool)` syntax form | ✅ COMPLETE (已添加到工具语法表) |
| 7 | MED | 插件缺失 | Add `blockedMarketplaces`, `pluginTrustMessage` | ✅ COMPLETE (已添加到插件表) |
| 8 | MED | 模型配置 | Add `availableModels` setting | ✅ COMPLETE (已添加到通用设置表) |
| 9 | MED | 可疑键 | Verify `sandbox.network.deniedDomains`, `sandbox.ignoreViolations`, `pluginConfigs` — present in report but not in official docs | ✋ ON HOLD (保留在报告中等待验证) |
| 10 | LOW | 头部计数 | Update header from "38 settings and 84 env vars" to reflect actual counts (~55+ settings, ~110+ env vars) | ✅ COMPLETE (已更新头部) |
| 11 | LOW | CLAUDE.md 同步 | Update CLAUDE.md configuration hierarchy (add managed/CLI/user levels) | ✋ ON HOLD (awaiting user approval) |
| 12 | LOW | 示例更新 | Update Quick Reference example with `$schema`, sandbox filesystem, `Agent(*)`, remove hooks example | ✅ COMPLETE (已更新示例) |
| 13 | MED | Hooks 重定向 | Replace hooks section with redirect to claude-code-hooks repo | ✅ COMPLETE (hooks 已外部化到专用仓库) |

---

## [2026-03-07 02:17 PM PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 行为变更 | Fix `teammateMode`: type `boolean` → `string`, default `false` → `"auto"`, description → "Agent team display: auto, in-process, tmux" | ✅ COMPLETE (类型、默认值和描述已更新) |
| 2 | HIGH | 新设置 | Add `allowManagedPermissionRulesOnly` to Permissions table (boolean, managed only) | ✅ COMPLETE (已添加到 Permission Keys 表) |
| 3 | HIGH | 环境变量缺失 | Add ~31 missing env vars including confirmed (`CLAUDE_CODE_MAX_OUTPUT_TOKENS`, `CLAUDE_CODE_DISABLE_FAST_MODE`, `CLAUDE_CODE_DISABLE_AUTO_MEMORY`, `CLAUDE_CODE_USER_EMAIL`, `CLAUDE_CODE_ORGANIZATION_UUID`, `CLAUDE_CONFIG_DIR`) and agent-reported (Foundry, Bedrock, mTLS, shell prefix, etc.) | ✅ COMPLETE (added 31 env vars to table) |
| 4 | MED | 默认值变更 | Fix `plansDirectory` default from `.claude/plans/` to `~/.claude/plans` | ✅ COMPLETE (已更新默认值) |
| 5 | MED | 描述变更 | Fix `sandbox.enableWeakerNetworkIsolation` description to "(macOS only) Allow access to system TLS trust; reduces security" | ✅ COMPLETE (已更新描述) |
| 6 | MED | 作用域修复 | Fix `extraKnownMarketplaces` scope from "Any" to "Project" | ✅ COMPLETE (已更新作用域和描述) |
| 7 | MED | 边界违规 | Replace `CLAUDE_CODE_EFFORT_LEVEL` in `claude-cli-startup-flags.md` with cross-reference to settings report | ✅ COMPLETE (已替换为链接) |
| 8 | MED | 版本徽章 | Update report version from v2.1.69 to v2.1.71 | ✅ COMPLETE (已更新徽章和头部) |
| 9 | LOW | 可疑键 | Verify `skipWebFetchPreflight`, `sandbox.ignoreViolations`, `sandbox.network.deniedDomains`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` | ✋ ON HOLD (kept in report pending verification — recurring from 2026-03-05) |
| 10 | LOW | CLAUDE.md 同步 | Update CLAUDE.md configuration hierarchy (3 levels → 5+) | ✅ COMPLETE (已更新为包含托管层的 5 级层级) |

---

## [2026-03-12 12:23 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 行为变更 | 修正 `dontAsk` 权限模式描述："自动接受所有工具" → "除非预先通过 `/permissions` 或 `permissions.allow` 规则批准，否则自动拒绝工具" | ✅ 完成（根据官方权限文档更正描述） |
| 2 | HIGH | 新设置 | 在模型配置部分添加 `modelOverrides`（对象类型，将 Anthropic 模型 ID 映射到特定供应商 ID，如 Bedrock ARN） | ✅ 完成（已添加示例和描述） |
| 3 | HIGH | 新设置 | 将 `allow_remote_sessions` 添加到仅托管设置列表（布尔值，默认为 `true`，控制远程控制/Web 会话访问） | ✅ 完成（已添加到权限键表） |
| 4 | HIGH | 默认值变更 | 根据官方文档将 `$schema` URL 从 `https://www.schemastore.org/...` 修正为 `https://json.schemastore.org/...` | ✅ 完成（已在描述、示例和来源中更新） |
| 5 | MED | 描述变更 | 根据官方文档将 `ANTHROPIC_CUSTOM_HEADERS` 格式描述从 "JSON 字符串" 更正为 "Name: Value 格式，以换行符分隔" | ✅ 完成（已根据官方文档更新描述） |
| 6 | MED | 未验证模式 | 官方文档中未记载 `askEdits` 和 `viewOnly` 权限模式 — 文档仅记录了 5 种模式（default, acceptEdits, plan, dontAsk, bypassPermissions） | ✅ 完成（已在表格中标记为“官方文档未记载 — 未验证”） |
| 7 | MED | 环境变量缺失 | 添加 `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS`、`CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY`、`CLAUDE_CODE_DISABLE_TERMINAL_TITLE`、`CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL`、`CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | ✅ 完成（已添加 5 个环境变量及 `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS`） |
| 8 | MED | 新设置 | 在核心配置中添加 `autoMemoryDirectory`（字符串类型，自定义自动记忆目录）— 版本不确定（智能体存在分歧：v2.1.68 vs v2.1.74），设置页面未列出 | ✅ 完成（已添加在 plansDirectory 附近 — 版本问题未解决） |
| 9 | LOW | 可疑键 | 验证 `skipWebFetchPreflight`、`sandbox.ignoreViolations`、`sandbox.network.deniedDomains`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` — 仍未见于官方文档 | ✋ 暂缓（保留在报告中待验证 — 自 2026-03-05 以来持续存在） |
| 10 | LOW | 环境变量缺失 | 将 `CLAUDE_CODE_SUBAGENT_MODEL` 添加到环境变量表（已在模型环境变量示例块中，但在表中缺失） | ✅ 完成（已添加到环境变量表） |
| 11 | LOW | 示例更新 | 更新快速参考示例，以包含 `modelOverrides` 和修正后的 `$schema` URL | ✅ 完成（示例已更新包含两者） |

---

## [2026-03-14 01:35 AM PKT] Claude Code v2.1.75

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 设置层级 | 重构以匹配官方5级层级结构：Managed (#1) > CLI args > Local > Project > User。移除 `~/.claude/settings.local.json` 行。添加 managed-tier 内部优先级（server-managed > MDM > file > HKCU）。备注：Managed "不能被任何其他层级覆盖，包括 CLI args" | ✅ COMPLETE (重构表格为5级，Managed 为 #1，添加了交付方式、内部优先级和文件路径) |
| 2 | HIGH | 行为变更 | 修复 `availableModels` 描述：根据官方文档，从复杂对象数组 (`title`/`modelId`/`effortOptions`) 更改为简单字符串数组 `["sonnet", "haiku"]` | ✅ COMPLETE (更新描述以匹配官方文档格式) |
| 3 | HIGH | 行为变更 | 添加 `cleanupPeriodDays` `0` 值行为："设置为 `0` 会在启动时删除所有现有会话记录，并完全禁用会话持久化" | ✅ COMPLETE (在描述中添加了0值行为) |
| 4 | HIGH | 权限语法 | 在权限部分添加评估顺序说明："规则按顺序评估：先评估拒绝规则，然后询问，最后允许。第一个匹配的规则生效。" | ✅ COMPLETE (在 Bash 通配符说明前添加了评估顺序) |
| 5 | MED | 描述变更 | 添加 `autoMemoryDirectory` 作用域限制："不接受项目设置 (`.claude/settings.json`) 中的配置。接受来自策略、本地和用户设置中的配置" | ✅ COMPLETE (在描述中添加了作用域限制) |
| 6 | MED | 描述变更 | 添加 `permissions.defaultMode` 远程环境说明：在远程环境中仅遵循 `acceptEdits` 和 `plan` (v2.1.70) | ✅ COMPLETE (在描述中添加了远程限制) |
| 7 | MED | 模型配置 | 添加 Opus 4.6 1M 上下文默认说明：自 v2.1.75 起，Max/Team/Enterprise 计划默认使用 1M 上下文 | ✅ COMPLETE (添加到 Effort Level 说明中) |
| 8 | MED | 设置层级 | 添加 Windows 受管路径说明：v2.1.75 移除了已弃用的 `C:\ProgramData\ClaudeCode\` 回退路径 — 请使用 `C:\Program Files\ClaudeCode\managed-settings.json` | ✅ COMPLETE (在层级部分添加了弃用说明) |
| 9 | MED | Display & UX | 添加 `fileSuggestion` stdin JSON 格式 (`{"query": "..."}`) 和 15 条路径输出限制详情 | ✅ COMPLETE (在文件建议部分添加了 stdin 格式和输出限制) |
| 10 | MED | 设置层级 | 根据官方文档，将数组合并说明从 "merged" 更新为 "concatenated and deduplicated" | ✅ COMPLETE (更新了层级重要说明部分的措辞) |
| 11 | LOW | 可疑键 | `sandbox.ignoreViolations`、`sandbox.network.deniedDomains` 仍未出现在官方文档或 JSON schema 顶层中 | ✋ ON HOLD (保留在报告中待验证 — 自2026-03-05起重复出现) |
| 12 | LOW | 可疑键 | `skipWebFetchPreflight`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` — 已在 JSON schema 中确认，但未出现在官方设置页面 | ✋ ON HOLD (保留在报告中 — 根据 schema 有效，自2026-03-05起重复出现) |

---

## [2026-03-15 12:52 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 新设置 | 在通用设置或模型配置中添加 `effortLevel` 项 —— 使 effort 级别在会话间持久化（可选值：`"low"`, `"medium"`, `"high"`）。已在官方设置页面确认 | ✋ 暂停（等待用户批准） |
| 2 | HIGH | 新设置 | 添加 Worktree 设置部分，包含 `worktree.sparsePaths`（数组，稀疏检出锥形模式）和 `worktree.symlinkDirectories`（数组，符号链接目录以避免重复）。已在官方设置页面确认 | ✋ 暂停（等待用户批准） |
| 3 | HIGH | 新设置 | 在通用设置中添加 `feedbackSurveyRate` 项 —— 用于会话质量调查的概率值（0-1）。已在官方设置页面确认 | ✋ 暂停（等待用户批准） |
| 4 | HIGH | 环境变量缺失 | 将 20 个缺失的环境变量添加至表格：`CLAUDE_CODE_AUTO_COMPACT_WINDOW`、`CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION`、`CLAUDE_CODE_PLAN_MODE_REQUIRED`、`CLAUDE_CODE_TEAM_NAME`、`CLAUDE_CODE_TASK_LIST_ID`、`CLAUDE_ENV_FILE`、`FORCE_AUTOUPDATE_PLUGINS`、`HTTP_PROXY`、`HTTPS_PROXY`、`NO_PROXY`、`MCP_TOOL_TIMEOUT`、`MCP_CLIENT_SECRET`、`MCP_OAUTH_CALLBACK_PORT`、`IS_DEMO`、`SLASH_COMMAND_TOOL_CHAR_BUDGET`、`VERTEX_REGION_CLAUDE_3_5_HAIKU`、`VERTEX_REGION_CLAUDE_3_7_SONNET`、`VERTEX_REGION_CLAUDE_4_0_OPUS`、`VERTEX_REGION_CLAUDE_4_0_SONNET`、`VERTEX_REGION_CLAUDE_4_1_OPUS`。已在官方 /en/env-vars 页面确认 | ✋ 暂停（等待用户批准） |
| 5 | HIGH | 环境变量缺失 | 将 `ANTHROPIC_DEFAULT_OPUS_MODEL`、`ANTHROPIC_DEFAULT_SONNET_MODEL`、`MAX_THINKING_TOKENS` 从仅代码块移至通用环境变量表格 | ✋ 暂停（等待用户批准） |
| 6 | HIGH | 链接失效 | 修复 `https://claudelog.com/configuration/` —— 返回 ECONNREFUSED 错误。 |

| # | 优先级 | 类别 | 描述 | 状态 |
|---|--------|------|------|------|
| 6 | MED | 来源更新 | 移除或替换为可用的源 | ✋ 暂停（等待用户批准） |
| 7 | MED | 描述变更 | 更新 `cleanupPeriodDays` 的描述，添加："当设置为 0 时，hooks 接收一个空的 `transcript_path`"。依据官方文档。 | ✋ 暂停（等待用户批准） |
| 8 | MED | 未验证的环境变量 | 将报告中但非官方文档中的 7 个环境变量标记为未验证：`CLAUDE_CODE_DISABLE_MCP`、`CLAUDE_CODE_DISABLE_TOOLS`、`CLAUDE_CODE_HIDE_ACCOUNT_INFO`、`CLAUDE_CODE_MAX_TURNS`、`CLAUDE_CODE_PROMPT_CACHING_ENABLED`、`CLAUDE_CODE_SKIP_SETTINGS_SETUP`、`DISABLE_NON_ESSENTIAL_MODEL_CALLS` | ✋ 暂停（等待用户批准） |
| 9 | MED | 新来源 | 将 `https://code.claude.com/docs/en/env-vars` 添加到“来源”部分 —— 官方环境变量参考页面 | ✋ 暂停（等待用户批准） |
| 10 | MED | 示例更新 | 更新快速参考示例，加入 `effortLevel` 和 `worktree` 设置 | ✋ 暂停（等待用户批准） |
| 11 | LOW | 可疑键 | `sandbox.ignoreViolations`、`sandbox.network.deniedDomains` 仍未在官方文档的沙盒表格中 | ✖ 暂停（保留在报告中，待验证 —— 2026-03-05 以来持续出现） |
| 12 | LOW | 可疑键 | `skipWebFetchPreflight`、`skippedMarketplaces`、`skippedPlugins`、`pluginConfigs` —— 仍在 JSON schema 中，但未出现在官方设置页面 | ✖ 暂停（保留在报告中 —— 根据 schema 有效，2026-03-05 以来持续出现） |

---

## [2026-03-15 01:10 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 新设置 | 在模型配置中添加 `effortLevel` — 使努力级别在会话间持续有效（`"low"`, `"medium"`, `"high"`）。同时在"常用命令"部分添加了 `/effort` 命令，并更新了努力级别操作指南章节 | ✅ 已完成（已添加至模型覆盖表，已更新操作指南，已添加 /effort 命令） |
| 2 | HIGH | 新设置 | 添加工作树设置部分，包括 `worktree.sparsePaths`（数组，稀疏检出锥模式）和 `worktree.symlinkDirectories`（数组，符号链接目录以避免重复） | ✅ 已完成（在核心配置中新建工作树设置子章节，包含表格和示例） |
| 3 | HIGH | 新设置 | 在通用设置中添加 `feedbackSurveyRate` — 会话质量调查的概率（0-1） | ✅ 已完成（已添加至通用设置表） |
| 4 | HIGH | 环境变量缺失 | 向表格中添加了23个缺失的环境变量（20个是真正新增的 + 3个原先仅出现在代码块中的） | ✅ 已完成（所有23个环境变量均已添加至常用环境变量表） |
| 5 | HIGH | 失效链接 | 之前的运行标记 `https://claudelog.com/configuration/` 为 ECONNREFUSED — 现在已可成功加载 | ✅ 已完成（链接已恢复，无需操作） |
| 6 | MED | 权限语法 | 添加了读取/编辑 gitignore 风格路径模式（`//path`, `~/path`, `/path`, `./path`），单词边界通配符详细说明，以及旧版 `:*` 的弃用说明 | ✅ 已完成（添加了路径模式表、单词边界说明和 `:*` 弃用说明） |
| 7 | MED | 描述变更 | 更新了 `cleanupPeriodDays` 的描述，增加了当设为0时"hooks 会收到一个空的 `transcript_path`" | ✅ 已完成（已添加到描述中） |
| 8 | MED | 未验证的环境变量 | 标记了7个未在官方文档中出现的环境变量为未验证 | ✅ 已完成（添加了"未在官方文档中出现 — 未验证"标记） |
| 9 | MED | 新来源 | 将 `https://code.claude.com/docs/en/env-vars` 和 `https://code.claude.com/docs/en/permissions` 添加到来源部分 | ✅ 已完成（两个URL均已添加） |
| 10 | MED | 示例更新 | 更新快速参考示例，以包含 `effortLevel` 和 `worktree` 设置 | ✅ 已完成（在示例中添加了 effortLevel 和 worktree 块） |
| 11 | LOW | 可疑键 | `sandbox.ignoreViolations`, `sandbox.network.deniedDomains` 仍未出现在官方文档的沙盒设置表中 | ✋ 暂缓（保留在报告中等待验证 — 重现自 2026-03-05） |
| 12 | LOW | 可疑键 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 仍在JSON schema中，但不在官方设置页面上 | ✋ 暂缓（保留在报告中 — 按 schema 视为有效，重现自 2026-03-05） |

---

## [2026-03-17 12:54 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 新设置 | 在 Sandbox Settings 表格中添加 `sandbox.filesystem.allowRead` —— 重新允许在 `denyRead` 区域内进行读取访问（数组类型，默认值 `[]`）。已在 v2.1.77 更新日志中确认 | ✅ 已完成（添加到 Sandbox Settings 表格的 denyRead 行之后） |
| 2 | HIGH | 描述变更 | 更新 `CLAUDE_CODE_MAX_OUTPUT_TOKENS` 描述：Opus 4.6 的默认值提升至 64k，Opus 4.6 和 Sonnet 4.6 的上限提升至 128k（v2.1.77 更新日志） | ✅ 已完成（描述已更新，包含特定模型的默认值和上限） |
| 3 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表格中添加 `CLAUDECODE` —— 在生成的 shell 环境中设置为 `1`。已在官方 /en/env-vars 页面确认 | ✅ 已完成（添加到环境变量表格） |
| 4 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表格中添加 `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` —— 当组织状态检查失败时允许快速模式。已在官方 /en/env-vars 页面确认 | ✅ 已完成（添加到环境变量表格） |
| 5 | MED | 环境变量表格 | 将 `ANTHROPIC_MODEL` 和 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 从仅代码块部分移至 Common Environment Variables 表格。两者均已在官方 /en/env-vars 页面确认 | ✅ 已完成（在环境变量表格中其他 ANTHROPIC_ 变量附近添加） |
| 6 | MED | 疑似密钥升级 | `sandbox.network.deniedDomains` —— 连续 8 次处于 ON HOLD 状态（自 2026-03-05 起）。未在官方文档页面或 JSON schema 中出现。根据规则 10B：标记为“未在官方文档中 — 未验证” | ✅ 已完成（在描述中添加了未验证注释） |
| 7 | MED | 疑似密钥升级 | `allow_remote_sessions` —— 未在官方文档页面或 JSON schema 中出现。 |

| 8 | 低 | 关键配置解析 | `sandbox.ignoreViolations` — 连续 8 次处于 ON HOLD 状态。已在 JSON schema 中确认。注释："在 JSON schema 中，不在官方设置页面上" | ✅ 已完成（已在描述中添加 schema 注释） |
| 9 | 低 | 关键配置解析 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 连续 8 次处于 ON HOLD 状态。均已在 JSON schema 中确认。注释："在 JSON schema 中，不在官方设置页面上" | ✅ 已完成（已为所有 4 项描述添加 schema 注释） |
| 10 | 低 | 标题计数 | 将环境变量标题计数从 "160+" 更新为 "100+" — 实际表格包含 97 个环境变量 | ✅ 已完成（标题已更新为 "100+ 环境变量"，版本更新为 v2.1.77） |

---

## [2026-03-18 11:53 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | Missing Setting | 在通用设置表中添加 `voiceEnabled` —— 启用按键通话语音听写功能（布尔值，通过 `/voice` 命令写入，需 Claude.ai 账户）。已在官方设置页面确认 | ✅ 已完成（添加至通用设置表中 feedbackSurveyRate 条目之前） |
| 2 | HIGH | Missing Setting | 在沙盒设置表中添加 `filesystem.allowManagedReadPathsOnly` —— 仅限托管模式，仅接受托管的 `allowRead` 路径（布尔值，默认 false）。已在官方设置页面确认 | ✅ 已完成（添加至沙盒设置表中 enableWeakerNetworkIsolation 条目之前） |
| 3 | HIGH | Display Location | 将 `showTurnDuration` 和 `terminalProgressBarEnabled` 从显示设置表移至独立的“全局配置设置 (~/.claude.json)”子章节。官方文档声明：“将其添加到 settings.json 将触发架构验证错误” | ✅ 已完成（创建新子章节并添加表格；已从 settings.json 的显示设置表及示例中移除） |
| 4 | HIGH | 默认值变更 | 将 `MAX_MCP_OUTPUT_TOKENS` 默认值从 50000 修正为 25000。官方 /en/env-vars 页面确认默认值为 25000 | ✅ 已完成（已更新默认值，添加阈值警告说明） |
| 5 | HIGH | 环境变量缺失 | 在环境变量表中添加 `CLAUDE_CODE_NEW_INIT`、`CLAUDE_CODE_PLUGIN_SEED_DIR`、`DISABLE_FEEDBACK_COMMAND`。所有变量均在官方 /en/env-vars 页面确认 | ✅ 已完成（已将全部 3 个环境变量添加至表格） |
| 6 | MED | Verification Fix | 移除 `allow_remote_sessions` 的“未验证”注释 —— 现已在官方权限页面确认其为仅限托管模式的设置 | ✅ 已完成 |

| 编号 | 优先级 | 类型 | 说明 | 状态 |
|:---|:---|:---|:---|:---|
| 6 | HIGH | 状态标记 | 上一次运行 (v2.1.77 #7) 错误地标记为未验证 | ✅ 已完成 (已移除“未验证”注解) |
| 7 | MED | 环境变量重命名 | 更新 `DISABLE_BUG_COMMAND` 为 `DISABLE_FEEDBACK_COMMAND` — 官方文档称 `DISABLE_FEEDBACK_COMMAND` 是当前名称，`DISABLE_BUG_COMMAND` 是“旧名称” | ✅ 已完成 (已重命名并添加别名说明) |
| 8 | MED | 描述变更 | 更新 `CLAUDE_CODE_EFFORT_LEVEL` 以包含 `max` (仅限 Opus 4.6) 和 `auto` 值。官方 /en/env-vars 页面确认：“值：low, medium, high, max (仅限 Opus 4.6), 或 auto” | ✅ 已完成 (描述已更新，包含所有值及优先级说明) |
| 9 | MED | 描述变更 | 修正 `CLAUDE_CODE_ENABLE_TASKS` 描述 — 官方说明：“设为 true 以启用非交互模式 (-p 标志) 下的任务跟踪。任务在交互模式下默认启用。” 当前报告称“设为 false 以禁用” | ✅ 已完成 (描述已修正以符合官方文档) |
| 10 | MED | 描述变更 | 更新 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 以注明：“相当于设置 DISABLE_AUTOUPDATER, DISABLE_FEEDBACK_COMMAND, DISABLE_ERROR_REPORTING, 和 DISABLE_TELEMETRY” | ✅ 已完成 (描述已更新，列出等效变量) |
| 11 | MED | 示例更新 | 从快速参考示例中移除 `showTurnDuration` — 根据官方文档，它不应出现在 settings.json 中 | ✅ 已完成 (已从快速参考示例及显示与 UX 示例中移除) |
| 12 | LOW | 环境变量默认值 | 验证 `MCP_TIMEOUT` 默认值 (报告称 10000) — 官方文档未指定默认值 | ✅ 已完成 (已移除未验证的默认值 — 官方文档省略) |

---

## [2026-03-19 12:38 PM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | 高 | 环境变量缺失 | 在通用环境变量表格中添加 `ANTHROPIC_CUSTOM_MODEL_OPTION`、`ANTHROPIC_CUSTOM_MODEL_OPTION_NAME`、`ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` — 用于为 `/model` 选择器添加自定义条目的模型配置变量。已确认官方 /en/env-vars 页面内容 | ✅ 已完成（在表格中 ANTHROPIC_BASE_URL 之后添加了 3 个环境变量） |
| 2 | 高 | 描述变更 | 将 `CLAUDE_CODE_PLUGIN_SEED_DIR` 从单数更新为复数形式："指向一个或多个只读插件种子目录的路径，在 Unix 系统中以 `:` 分隔，在 Windows 系统中以 `;` 分隔"。此变更记录于 v2.1.79 更新日志中。已确认官方 /en/env-vars 页面内容 | ✅ 已完成（描述已更新为支持多目录） |
| 3 | 高 | 沙箱路径前缀 | 修正 sandbox.filesystem 路径前缀文档：`/` = 绝对路径（标准 Unix），`./` = 项目相对路径，`//` = 旧语法仍有效。当前报告显示的惯例相反。官方文档明确指出："此语法与读取和编辑权限规则不同" | ✅ 已完成（已使用正确的前缀惯例更新所有 4 个 sandbox.filesystem 条目，添加了指向读取/编辑权限规则的交叉引用说明，补充了跨作用域合并的细节） |
| 4 | 中 | 描述变更 | 扩展 `CLAUDE_CODE_AUTO_COMPACT_WINDOW` 的描述 — 当前的 "自动压缩窗口行为配置" 过于简略。官方文档描述了：token 容量、默认值（标准 200K / 扩展 1M）、与 `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 的交互、状态行解耦 | ✅ 已完成（已扩展描述，包含 token 容量、模型默认值、AUTOCOMPACT_PCT 交互以及状态行解耦说明） |

---

## [2026-03-20 08:41 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 新设置 | Add `channelsEnabled` to MCP Settings table — managed-only boolean, controls channel message delivery for Team and Enterprise users. Confirmed on official settings page | ✅ COMPLETE (added to MCP Settings table after allowManagedMcpServersOnly) |
| 2 | MED | 版本徽章 | Update report version from v2.1.79 to v2.1.80 | ✅ COMPLETE (已更新徽章和头部) |

---

## [2026-03-21 09:17 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | 高 | 设置缺失 (~/.claude.json) | 在全局配置设置表格中添加 `autoConnectIde`（布尔值，默认为 `false`）和 `autoInstallIdeExtension`（布尔值，默认为 `true`）。已在官方设置页面的"全局配置设置"下确认 | ✅ 已完成（在 showTurnDuration 之前将两个键添加到 ~/.claude.json 表格中） |
| 2 | 高 | 设置不正确 | `allow_remote_sessions` 在权限键表格中列为仅限托管的布尔值，但官方权限页面说明："访问远程控制和 Web 会话不受托管设置键控制。" 标记为未验证或移除 | ✅ 已完成（重新添加了未验证注释，并附官方文档引文和管理员 UI 链接） |
| 3 | 中 | 版本更新 | 将报告版本徽章从 v2.1.80 更新至 v2.1.81 | ✅ 已完成（徽章、页眉版本和页眉文本均已更新） |
| 4 | 中 | 新设置 | 添加 `showClearContextOnPlanAccept` — 已在 v2.1.81 更新日志中确认。设为 `true` 时，将在接受计划时恢复"清除上下文"选项（默认隐藏）。尚未出现在官方设置页面 — 可能是 `~/.claude.json` 键 | ✅ 已完成（添加到全局配置设置表格，并附更新日志来源说明） |
| 5 | 中 | 插件文档 | 在插件设置部分将 `source: 'settings'` 记录为市场来源类型。官方设置页面将其列为 `extraKnownMarketplaces` 的 7 种来源类型之一 | ✅ 已完成（添加了所有 7 种来源类型列表及行内市场示例） |
| 6 | 中 | 状态行字段 | 将 `rate_limits` 字段组添加到状态行输入字段表格 — 包含 `five_hour.used_percentage`、`five_hour.resets_at`、`seven_day.used_percentage`、`seven_day.resets_at`。 |

新增于 v2.1.80 | ✅ 已完成（向 Status Line Input Fields 表格添加了4个 rate_limits 字段） |

---

## [2026-03-23 10:02 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | 高 | 缺失设置 (~/.claude.json) | 在全局配置设置表中添加 `editorMode`（字符串类型，默认值为 `"normal"`，可选值为 `"normal"` 或 `"vim"`）。运行 `/vim` 时会自动写入。已在官方设置页面确认 | ✅ 已完成 (添加到全局配置设置表中，位于 autoInstallIdeExtension 之后) |
| 2 | 高 | 文件范围修复 | 将 `showClearContextOnPlanAccept` 从全局配置设置 (~/.claude.json) 移动到通用设置 (settings.json)。官方文档现在将其列在主可用设置表中，而非全局配置表中。移除过时的注释"not yet on official settings page" | ✅ 已完成 (移动到通用设置表中，位于 feedbackSurveyRate 之前，并移除了过时的注释) |
| 3 | 中 | 描述变更 | 根据官方文档，将 `terminalProgressBarEnabled` 支持的终端从 "Windows Terminal, iTerm2" 修正为 "ConEmu, Ghostty 1.2.0+, and iTerm2 3.6.6+" | ✅ 已完成 (终端列表已更新) |
| 4 | 中 | 描述变更 | 在 `availableModels` 描述中添加 "Config tool" — 官方文档说明为 "via `/model`, `--model`, Config tool, or `ANTHROPIC_MODEL`"。当前报告缺少 "Config tool" | ✅ 已完成 (在描述中添加了 "Config tool") |

---

## [2026-03-25 08:16 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 新设置 | 在权限部分添加 `autoMode` — 包含 `environment`、`allow`、`soft_deny` 数组的对象，用于配置自动模式分类器。不从共享项目设置（`.claude/settings.json`）中读取。可在用户、本地和托管设置中使用。已在官方设置与权限页面确认 | ✅ 已完成（已添加至权限键表，包含完整描述、作用域限制及 `claude auto-mode defaults` 说明） |
| 2 | HIGH | 新设置 | 在权限部分添加 `disableAutoMode` — 字符串类型，设置为 `"disable"` 可阻止自动模式激活。从 Shift+Tab 循环中移除 `auto` 选项。可在任何设置层级配置，在托管设置中最为有用。已在官方设置与权限页面确认 | ✅ 已完成（已添加至权限键表中 `autoMode` 条目之后） |
| 3 | HIGH | 新权限模式 | 在权限模式表中添加 `auto` 模式 — 后台分类器替代手动提示。研究预览版。需要团队计划 + Sonnet/Opus 4.6。已在官方权限模式页面确认 | ✅ 已完成（已添加至权限模式表，包含分类器详情及回退行为） |
| 4 | HIGH | 新设置 | 在沙盒设置表中添加 `sandbox.failIfUnavailable` — 布尔值，默认为 `false`，当沙盒启用但无法启动时以错误退出，而非在非沙盒环境下运行。已在 v2.1.83 更新日志中确认 | ✅ 已完成（已添加至沙盒设置表中 `sandbox.enabled` 条目之后） |
| 5 | HIGH | 新设置 | 在通用设置表中添加 `disableDeepLinkRegistration` — 布尔值，阻止 `claude-cli://` 协议处理程序注册。 |

| 6 | 高 | 缺少环境变量 | 在常用环境变量表中添加 `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` —— 设置为 `1` 可从子进程环境（Bash 工具、hooks、MCP stdio 服务器）中剥离 Anthropic 和云服务商凭证。已在 v2.1.83 更新日志中确认 | ✅ 已完成（在 `CLAUDE_CODE_SUBAGENT_MODEL` 后添加至环境变量表） |
| 7 | 高 | 设置层级 | 在管理设置章节添加 `managed-settings.d/` 插件目录 —— 作为独立的策略片段，与 `managed-settings.json` 并列，按字母顺序合并。已在 v2.1.83 更新日志中确认 | ✅ 已完成（作为管理设置交付方法下的列表项添加） |
| 8 | 高 | 失效链接 | 修复源中的 `https://claudelog.com/configuration/` —— 返回 403 Forbidden。需移除或替换为有效源 | ✅ 已完成（替换为经验证有效的 `https://claudelog.com/claude-code-changelog/`） |
| 9 | 中 | 版本徽章 | 将报告版本从 v2.1.81 更新至 v2.1.83 | ✅ 已完成（在第 2.6 阶段更新了徽章和标题） |
| 10 | 中 | 示例更新 | 在快速参考示例中添加 `autoMode` 以演示自动模式分类器配置 | ✅ 已完成（在 `permissions` 块前添加了包含 `environment` 数组的 `autoMode` 块） |
| 11 | 中 | 路径变更 | 将 Windows 注册表路径从 `Software\Anthropic\ClaudeCode` 修正为 `SOFTWARE\Policies\ClaudeCode`（HKLM 和 HKCU）。官方文档已更新为使用 `Policies` 子键 | ✅ 已完成（更新为 `HKLM\SOFTWARE\Policies\ClaudeCode` 和 `HKCU\SOFTWARE\Policies\ClaudeCode`，并附优先级说明） |
| 12 | 低 | 缺少别名 | 在模型别名表中添加 `opus[1m]` —— Opus 4.6 拥有 1M 上下文，自 v2.1.75 起在 Max/Team/Enterprise 版默认可用 | ✅ 已完成（在 `sonnet[1m]` 后添加至模型别名表） |

---

## [2026-03-26 01:04 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 新设置 | 在通用设置中添加 `defaultShell` —— 字符串类型，默认值为 `"bash"`，接受 `"bash"` 或 `"powershell"`。在 Windows 系统上通过 PowerShell 路由交互式的 `!` 命令。需要设置 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`。已在官方设置页面确认 | ✅ 完成（已添加到通用设置表中 teammateMode 之后） |
| 2 | HIGH | 新设置 | 在 MCP 设置中添加 `allowedChannelPlugins` —— 数组类型，仅限管理。允许推送消息的渠道插件的白名单。设置后将替换默认的 Anthropic 白名单。需要 `channelsEnabled: true`。已在官方设置页面确认 | ✅ 完成（已添加到 MCP 设置表中 channelsEnabled 之后） |
| 3 | HIGH | 新设置 | 在权限键中添加 `useAutoModeDuringPlan` —— 布尔类型，默认值为 `true`。当自动模式可用时，计划模式使用自动模式语义。不从共享项目设置读取。已在官方设置页面确认 | ✅ 完成（已添加到权限键表中 disableAutoMode 之后） |
| 4 | HIGH | 环境变量缺失 | 添加 9 个模型自定义环境变量：`ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU}_MODEL_{NAME,DESCRIPTION,SUPPORTED_CAPABILITIES}`，用于在 Bedrock/Vertex/Foundry 上自定义 `/model` 选择器。已在官方 /en/env-vars 页面确认 | ✅ 完成（已在每个基础模型变量 Haiku、Opus、Sonnet 之后各添加 3 个变量） |
| 5 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` —— 在流式传输失败时禁用非流式回退。防止通过代理重复执行工具。 |

| 6 | 高 | 缺失环境变量 | 添加 `CLAUDE_CODE_USE_POWERSHELL_TOOL` — 在Windows上启用PowerShell工具（可选预览功能）。仅限原生Windows，不支持WSL。已在官方/en/env-vars页面确认 | ✅ 已完成（添加于CLAUDE_CODE_USE_FOUNDRY之后） |
| 7 | 高 | 失效链接 | 修复来源中的 `https://claudelog.com/claude-code-changelog/` — 返回403禁止访问。替换为官方GitHub变更日志URL | ✅ 已完成（替换为github.com/anthropics/claude-code/blob/main/CHANGELOG.md） |
| 8 | 中 | 设置层级 | 更新托管层级优先级："基于文件的层级（`managed-settings.d/*.json` + `managed-settings.json`）"并添加"跨层级"限定说明。根据官方文档添加层级内合并注释 | ✅ 已完成（更新了基于文件的层级优先级描述及跨层级限定说明） |
| 9 | 中 | 设置层级 | 扩展即时目录合并语义：systemd约定、标量覆盖、数组合并去重、深度合并、隐藏文件排除、数字前缀提示。根据官方设置页面 | ✅ 已完成（扩展了完整的systemd约定细节及数字前缀提示） |
| 10 | 中 | 注释标注 | 根据规则1F的反向完整性检查，为 `disableDeepLinkRegistration` 添加"仅见于变更日志，未在官方设置页面列出"的注释 | ✅ 已完成（在描述中添加了注释标注） |
| 11 | 中 | 示例更新 | 在快速参考示例中添加 `defaultShell` 以展示PowerShell配置 | ✅ 已完成（在示例中添加了 "defaultShell": "bash"） |

---

## [2026-03-27 06:32 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | 高 | 环境变量缺失 | 将 `CLAUDE_STREAM_IDLE_TIMEOUT_MS` 添加到通用环境变量表格 — 流式空闲监控关闭停滞连接前的超时时间（单位：毫秒）（默认值：90000）。已在官方 /en/env-vars 页面确认。于 v2.1.84 版本添加，但在之前的运行中遗漏 | ✅ 已完成（已添加到环境变量表格，位于 CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS 之后） |
| 2 | 高 | 版本号提升 | 将报告版本徽章从 v2.1.84 更新为 v2.1.85 | ✅ 已完成（徽章、页眉版本和页眉文本在第 2.6 阶段已更新） |
| 3 | 中 | 新增环境变量 | 将 `OTEL_LOG_TOOL_DETAILS` 添加到环境变量表格 — 控制 OpenTelemetry 事件中 `tool_parameters` 的输出。仅 v2.1.85 更新日志有提及（尚未出现在官方 env-vars 页面）。添加时需注明来源为更新日志 | ✅ 已完成（添加时附有“来自 v2.1.85 更新日志，尚未出现在官方 env-vars 页面”的注释） |
| 4 | 中 | 新增环境变量（归属） | 决定 `CLAUDE_CODE_MCP_SERVER_NAME` 和 `CLAUDE_CODE_MCP_SERVER_URL` 的归属 — 传递给 MCP `headersHelper` 脚本的环境变量（v2.1.85 更新日志）。可能属于 hooks 仓库而非 settings 报告 | ✅ 已完成（已添加到 settings 报告并附更新日志注释 — 这些环境变量可通过 `env` 键配置，并非仅限于 hook） |