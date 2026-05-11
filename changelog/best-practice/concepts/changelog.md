# 变更日志 — README CONCEPTS 部分

追踪 README CONCEPTS 表格与官方 Claude Code 文档之间的漂移。

## 状态图例

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE (reason)` | 已采取行动并成功解决 |
| ❌ `INVALID (reason)` | 发现不正确、不适用或属预期行为 |
| ✋ `ON HOLD (reason)` | 行动已推迟——等待外部依赖或用户决定 |

---

## [2026-03-02 11:14 AM PKT] Claude Code v2.1.63

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 链接失效 | 修复 Permissions URL，从 `/iam` 改为 `/permissions` | ✅ COMPLETE (URL 已更新为 /permissions) |
| 2 | HIGH | 概念缺失 | 向 CONCEPTS 表添加 Agent Teams 行 | ✅ COMPLETE (已添加行，路径 ~\/\.claude\/teams\/ location) |
| 3 | HIGH | 概念缺失 | 向 CONCEPTS 表添加 Keybindings 行 | ✅ COMPLETE (已添加行，路径 ~\/\.claude\/keybindings\.json location) |
| 4 | HIGH | 概念缺失 | 向 CONCEPTS 表添加 Model Configuration 行 | ✅ COMPLETE (已添加行，路径 \.claude\/settings\.json location) |
| 5 | HIGH | 概念缺失 | 向 CONCEPTS 表添加 Auto Memory 行 | ✅ COMPLETE (已添加行，路径 ~\/\.claude\/projects\/<project>\/memory\/ location) |
| 6 | HIGH | 锚点过期 | 修复 Rules URL 锚点，从 `#modular-rules-with-clauderules` to `#organize-rules-with-clauderules` | ✅ COMPLETE (锚点已更新) |
| 7 | MED | 概念缺失 | 向 CONCEPTS 表添加 Checkpointing 行 | ✅ COMPLETE (已添加行，路径 automatic git-based location) |
| 8 | MED | 概念缺失 | 向 CONCEPTS 表添加 Status Line 行 | ✅ COMPLETE (已添加行，路径 ~\/\.claude\/settings\.json location) |
| 9 | MED | 概念缺失 | 向 CONCEPTS 表添加 Remote Control 行 | ✅ COMPLETE (已添加行，路径 CLI \/ claude\.ai location) |
| 10 | MED | 概念缺失 | 向 CONCEPTS 表添加 Fast Mode 行 | ✅ COMPLETE (已添加行，路径 \.claude\/settings\.json location) |
| 11 | MED | 概念缺失 | 向 CONCEPTS 表添加 Headless Mode 行 | ✅ COMPLETE (已添加行，路径 CLI flag -p location) |
| 12 | LOW | 描述变更 | 更新 Memory 描述以提及 auto memory | ✅ COMPLETE (描述和路径已更新) |
| 13 | LOW | 路径变更 | 更新 MCP Servers 路径以包含 `.mcp.json` | ✅ COMPLETE (路径已更新以包含 .mcp.json) |
| 14 | LOW | 徽章缺失 | 向 Hooks 行添加 Implemented 徽章 | ✅ COMPLETE (已添加 Implemented 徽章链接到 .claude/hooks/) |

---

## [2026-03-02 11:57 AM PKT] Claude Code v2.1.63

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 表格合并 | 将 CONCEPTS 表从 22 行合并为 10 行 — 将相关概念折叠为内联文档链接 | ✅ COMPLETE (22 行 → 10 行) |
| 2 | MED | 概念合并 | 将 Marketplaces 折叠为 Plugins 行的内联链接 | ✅ COMPLETE (链接到 /discover-plugins) |
| 3 | MED | 概念合并 | 将 Agent Teams 折叠为 Sub-Agents 行的内联链接 | ✅ COMPLETE (链接到 /agent-teams) |
| 4 | MED | 概念合并 | 将 Permissions、Model Config、Output Styles、Sandboxing、Keybindings、Status Line、Fast Mode 折叠为 Settings 行的内联链接 | ✅ COMPLETE (7 个概念已折叠并添加文档链接) |
| 5 | MED | 概念合并 | 将 Auto Memory 和 Rules 折叠为 Memory 行的内联链接 | ✅ COMPLETE (链接到 /memory 和 /memory#organize-rules-with-clauderules) |
| 6 | MED | 概念合并 | 将 Headless Mode 折叠为 Remote Control 行的内联链接 | ✅ COMPLETE (链接到 /headless) |
| 7 | LOW | 重新排序 | 按逻辑分组重新排序：构建模块 → 扩展 → 配置 → 上下文 → 运行时 | ✅ COMPLETE (按关注点分组，而非按时间顺序) |

---

## [2026-03-07 08:40 AM PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 链接失效 | Fix `context-management` → `interactive-mode` in TIPS (lines 112, 115, 135) | ✅ COMPLETE (3 处已替换为 interactive-mode) |
| 2 | HIGH | 链接失效 | Fix `model-configuration` → `model-config` in TIPS (lines 115, 116, 135) | ✅ COMPLETE (3 处已替换为 model-config) |
| 3 | HIGH | 链接失效 | Fix `usage-billing` → `costs` in TIPS (line 115) | ✅ COMPLETE (已替换为 costs) |
| 4 | HIGH | 链接失效 | Remove `cowork` URL in STARTUPS (line 167) — page does not exist | ✅ COMPLETE (超链接已移除，保留纯文本) |
| 5 | HIGH | 概念缺失 | Add Scheduled Tasks row to CONCEPTS and Hot section (`/loop`, cron tools) | ✅ COMPLETE (added by user to both tables + /loop tip + Boris tweet) |
| 6 | MED | 路径变更 | Update Agent Teams location from `.claude/agents/<name>.md` to `built-in (env var)` | ✅ COMPLETE (location updated to built-in env var) |

---

## [2026-03-10 01:18 PM PKT] Claude Code v2.1.72

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 链接失效 | Fix Commands URL from `/slash-commands` to `/skills` 在 CONCEPTS 表中 (line 24) — `/slash-commands` serves Skills page content; docs say "commands merged into skills" | ❌ INVALID (URL 仍可访问；用户选择保持原样) |
| 2 | HIGH | 链接失效 | Fix Commands URL from `/slash-commands` to `/skills` in TIPS section (line 108) — same stale URL | ❌ INVALID (URL 仍可访问；用户选择保持原样) |
| 3 | MED | 内联链接缺失 | Add Interactive Mode (`/interactive-mode`) as inline link to CLI Startup Flags row — covers /compact, /clear, /context, /extra-usage | ✅ COMPLETE (内联链接已添加到 CLI Startup Flags 描述) |
| 4 | MED | 内联链接缺失 | Add Costs (`/costs`) as inline link to Settings row — covers /usage, billing, pay-as-you-go | ❌ INVALID (user chose to skip) |
| 5 | LOW | 概念缺失 | Consider adding IDE Integrations row (VS Code, JetBrains, Desktop App, Web) or inline links to Best Practices | ❌ INVALID (user chose to skip — platform surfaces, not configuration concepts) |
| 6 | HIGH | 概念缺失 | 向 Hot 表添加 Code Review 行 — multi-agent PR analysis (research preview, Teams & Enterprise) | ✅ COMPLETE (row 作为第一个 Hot 条目添加，带 blog link and best practice tweet) |
| 7 | MED | 新增徽章 | 创建 `!/tags/beta.svg` 标签（黄色，38x20px）并添加到 Code Review and Agent Teams in Hot table | ✅ COMPLETE (beta.svg 已创建；已添加到 Code Review 和 Agent Teams 行) |
| 8 | MED | 重新排序 | 按发布日期排序 Hot 表（最新在前）: Code Review → Scheduled Tasks → Voice Mode → Agent Teams → Remote Control → Git Worktrees → Ralph Wiggum | ✅ COMPLETE (Voice Mode 和 Agent Teams 已交换以匹配时间顺序) |

---

## [2026-03-12 12:22 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 链接失效 | Fix Commands URL from `/slash-commands` to `/skills` 在 CONCEPTS 表中 (line 24) — `/slash-commands` redirects to `/skills` page | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | LOW | 验证 | 所有外部文档 URL 已验证——未发现失效链接 | ✅ COMPLETE (all 20+ URLs return valid pages) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all badge targets exist on filesystem) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` validated on target page | ✅ COMPLETE (标题存在于 /memory 页面) |
| 5 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查 | ✅ COMPLETE (no description drift detected) |

---

## [2026-03-15 12:48 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | MED | Missing Badges | Remote Control (Hot) has zero badges — only Hot item without any BP or Impl badge | ✅ COMPLETE (BP badge added linking to official docs page) |
| 3 | LOW | 命名 | "Sub-Agents" in README vs "subagents" (one word) in official docs — cosmetic inconsistency | ✅ COMPLETE (renamed to "Subagents" 在 CONCEPTS 表中) |
| 4 | LOW | 验证 | All 27 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all badge targets exist on filesystem) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 7 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (descriptions accurate for all 13 CONCEPTS + 9 Hot rows) |

---

## [2026-03-17 12:46 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | HIGH | 描述变更 | Hooks description says "Deterministic scripts" but hooks now include 4 types: command, HTTP, prompt, and agent — only command hooks are deterministic | ✅ COMPLETE (updated to "User-defined handlers (scripts, HTTP, prompts, agents)" 在 CONCEPTS 表中) |
| 3 | MED | 概念缺失 | Desktop App has dedicated docs page at `/desktop` — not in CONCEPTS or Hot table | ❌ INVALID (user chose to skip — Desktop is a platform surface, not a configuration concept) |
| 4 | MED | URL 变更 | Hooks docs now split into Guide (`/hooks-guide`) and Reference (`/hooks`) — CONCEPTS links only to Reference | ✅ COMPLETE (Guide 链接已作为内联链接添加到 Hooks 行描述中) |
| 5 | LOW | 验证 | All 28 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 6 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20 badge targets exist on filesystem) |
| 7 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查 | ✅ COMPLETE (Hooks description drift detected — see #2) |

---

## [2026-03-18 11:43 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | HIGH | URL 和名称变更 | Voice Mode in Hot table links to tweet instead of official docs `/voice-dictation`; official name is "Voice Dictation" | ✅ COMPLETE (renamed to "Voice Dictation", linked to /voice-dictation, description updated; BP badge kept linking to tweet; also updated in STARTUPS table) |
| 3 | LOW | 验证 | All 29 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 4 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 5 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 6 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |

---

## [2026-03-19 11:59 AM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | LOW | 验证 | All 30 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 5 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |

---

## [2026-03-20 08:38 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 概念缺失 | 向 Hot 表添加 Channels 行 — push events from Telegram/Discord/webhooks into running sessions (research preview, v2.1.80) | ✅ COMPLETE (row 作为第一个 Hot 条目添加，带 beta badge and Reference link) |
| 2 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 3 | MED | 深层链接缺失 | Git Worktrees URL should anchor to `#run-parallel-claude-code-sessions-with-git-worktrees` | ✅ COMPLETE (anchor added to Git Worktrees URL in Hot table) |
| 4 | LOW | 内联链接缺失 | Plugins row could add `[Marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)` sub-link | ✅ COMPLETE (创建 Marketplaces inline link added to Plugins row) |
| 5 | LOW | 验证 | All 31 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 6 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 7 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |

---

## [2026-03-21 09:12 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | LOW | 验证 | All 32 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 5 | LOW | 验证 | Git Worktrees anchor `#run-parallel-claude-code-sessions-with-git-worktrees` confirmed on /common-workflows page | ✅ COMPLETE (节标题存在) |
| 6 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |

---

## [2026-03-23 09:53 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | LOW | 验证 | All 33 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 5 | LOW | 验证 | Git Worktrees anchor `#run-parallel-claude-code-sessions-with-git-worktrees` confirmed on /common-workflows page | ✅ COMPLETE (节标题存在) |
| 6 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |

---

## [2026-03-25 08:12 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | MED | URL 变更 | Simplify & Batch primary link points to tweet instead of official docs `/skills#bundled-skills` — now officially bundled skills | ✅ COMPLETE (主链接已更新为 /skills#bundled-skills; BP badge kept linking to Boris's tweet) |
| 3 | LOW | 验证 | All 34 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 4 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 5 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 6 | LOW | 验证 | Git Worktrees anchor `#run-parallel-claude-code-sessions-with-git-worktrees` confirmed on /common-workflows page | ✅ COMPLETE (节标题存在) |
| 7 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |
| 8 | HIGH | 概念缺失 | 向 Hot 表添加 Auto Mode 行 — background safety classifier replaces permission prompts (research preview, Team/Enterprise) | ✅ COMPLETE (row 作为第一个 Hot 条目添加，带 beta badge, BP badge linking to @claudeai tweet, and blog link) |

---

## [2026-03-26 01:05 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | MED | 概念缺失 | 向 Hot 表添加 Slack 集成 — mention @Claude in Slack to route coding tasks to Claude Code web sessions | ✅ COMPLETE (行已添加在 Channels with @Claude location and web session description) |
| 3 | MED | 概念缺失 | 向 Hot 表添加 GitHub Actions / CI-CD — automate PR reviews, issue triage, and code generation in CI/CD pipelines | ✅ COMPLETE (行已添加在 Code Review with .github/workflows/ location and GitLab CI/CD inline link) |
| 4 | LOW | 验证 | All 35 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 7 | LOW | 验证 | Git Worktrees anchor `#run-parallel-claude-code-sessions-with-git-worktrees` confirmed on /common-workflows page | ✅ COMPLETE (节标题存在) |
| 8 | LOW | 验证 | Auto Mode anchor `#eliminate-prompts-with-auto-mode` confirmed on /permission-modes page | ✅ COMPLETE (节标题存在) |
| 9 | LOW | 验证 | Bundled Skills anchor `#bundled-skills` confirmed on /skills page | ✅ COMPLETE (节标题存在) |
| 10 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |

---

## [2026-03-27 06:37 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | URL 过期 | Commands URL `/slash-commands` serves Skills page — docs say "commands merged into skills" | ❌ INVALID (重复出现，自 2026-03-10; URL 仍可访问；用户选择保持原样) |
| 2 | MED | 概念缺失 | 向 Hot 表添加 Chrome 集成 — browser automation via Claude in Chrome extension (beta, dedicated docs at `/chrome`) | ✅ COMPLETE (行已添加在 GitHub Actions with --chrome location and beta badge) |
| 3 | LOW | 验证 | All 36 external docs URLs validated — no broken links found | ✅ COMPLETE (all URLs return valid pages including /slash-commands redirect) |
| 4 | LOW | 验证 | 所有本地徽章文件路径已验证——未发现缺失文件 | ✅ COMPLETE (all 20+ badge targets exist on filesystem) |
| 5 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (节标题存在) |
| 6 | LOW | 验证 | Git Worktrees anchor `#run-parallel-claude-code-sessions-with-git-worktrees` confirmed on /common-workflows page | ✅ COMPLETE (节标题存在) |
| 7 | LOW | 验证 | Auto Mode anchor `#eliminate-prompts-with-auto-mode` confirmed on /permission-modes page | ✅ COMPLETE (节标题存在) |
| 8 | LOW | 验证 | Bundled Skills anchor `#bundled-skills` confirmed on /skills page | ✅ COMPLETE (节标题存在) |
| 9 | LOW | 验证 | 所有 CONCEPTS 描述已与官方文档对比检查——未检测到漂移 | ✅ COMPLETE (all descriptions accurate) |