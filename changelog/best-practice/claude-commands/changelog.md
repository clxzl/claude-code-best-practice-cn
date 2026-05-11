# Commands 报告 — 变更历史

## 状态图例

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE（原因）` | 操作已执行并成功解决 |
| ❌ `INVALID（原因）` | 发现不正确、不适用或属于有意设计 |
| ✋ `ON HOLD（原因）` | 操作已推迟 — 等待外部依赖或用户决定 |

---

## [2026-03-13 04:23 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新字段 | 向 frontmatter 表添加 `name` — Skill 的显示名称 | ❌ INVALID（仅限 Skill 的字段，不适用于 Commands frontmatter） |
| 2 | 高 | 新字段 | 向 frontmatter 表添加 `disable-model-invocation` — 防止自动加载 | ❌ INVALID（仅限 Skill 的字段，不适用于 Commands frontmatter） |
| 3 | 高 | 新字段 | 向 frontmatter 表添加 `user-invocable` — 从 `/` 菜单中隐藏 | ❌ INVALID（仅限 Skill 的字段，不适用于 Commands frontmatter） |
| 4 | 高 | 新字段 | 向 frontmatter 表添加 `context` — 在子 Agent 上下文中运行的分支 | ❌ INVALID（仅限 Skill 的字段，不适用于 Commands frontmatter） |
| 5 | 高 | 新字段 | 向 frontmatter 表添加 `agent` — context: fork 的子 Agent 类型 | ❌ INVALID（仅限 Skill 的字段，不适用于 Commands frontmatter） |
| 6 | 高 | 新字段 | 向 frontmatter 表添加 `hooks` — 限定于 Skill 的生命周期 hooks | ❌ INVALID（仅限 Skill 的字段，不适用于 Commands frontmatter） |
| 7 | 高 | 新命令 | 添加 `/btw <question>` — 快速提出侧问题而不添加到对话中 | ✅ COMPLETE（在 Session 标签中作为 #53 添加） |
| 8 | 高 | 新命令 | 添加 `/hooks` — 管理工具事件的 hook 配置 | ✅ COMPLETE（在 Extensions 标签中作为 #30 添加） |
| 9 | 高 | 新命令 | 添加 `/insights` — 生成会话分析报告 | ✅ COMPLETE（在 Context 标签中作为 #17 添加） |
| 10 | 高 | 新命令 | 添加 `/plugin` — 管理 Claude Code 插件 | ✅ COMPLETE（在 Extensions 标签中作为 #33 添加） |
| 11 | 高 | 新命令 | 添加 `/skills` — 列出可用的 Skills | ✅ COMPLETE（在 Extensions 标签中作为 #35 添加） |
| 12 | 高 | 新命令 | 添加 `/upgrade` — 打开升级页面以切换套餐等级 | ✅ COMPLETE（在 Auth 标签中作为 #3 添加） |
| 13 | 高 | 已移除命令 | 移除 `/output-style` — 在 v2.1.73 中已弃用，请使用 `/config` 替代 | ✅ COMPLETE（从 Config 标签中移除） |
| 14 | 高 | 已移除命令 | 移除 `/bug` 行 — 现在作为 `/feedback` 的别名列示 | ✅ COMPLETE（移除行，在 /feedback 描述中添加"别名: /bug"） |
| 15 | 高 | 描述变更 | 更新 `/passes` — 从审查轮次重新定位为推荐分享 | ✅ COMPLETE（更新描述，保留在 Model 标签中） |
| 16 | 高 | 描述变更 | 更新 `/review` — 已弃用，由 `code-review` 市场插件替代 | ✅ COMPLETE（在 Project 标签中更新描述） |
| 17 | 中 | 描述变更 | 更新 `/stickers` — 从 UI 贴纸包更改为订购实物贴纸 | ✅ COMPLETE（在 Config 标签中更新描述） |

---

## [2026-03-15 12:50 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新命令 | 在 Config 标签中添加 `/color [color\|default]` — 为当前会话设置提示栏颜色 | ✅ COMPLETE（在 Config 标签中作为 #4 添加） |
| 2 | 高 | 新命令 | 在 Model 标签中添加 `/effort [low\|medium\|high\|max\|auto]` — 设置模型 effort 级别 | ✅ COMPLETE（在 Model 标签中作为 #38 添加） |
| 3 | 中 | 描述变更 | 更新 `/status` — 现在是"打开设置界面（状态选项卡）"而非"显示简洁的会话状态摘要" | ✅ COMPLETE（在 Context 标签中 #20 更新描述） |
| 4 | 中 | 描述变更 | 更新 `/desktop` — 现在是"在 Claude Code 桌面应用中继续当前会话。仅限 macOS 和 Windows。" | ✅ COMPLETE（在 Remote 标签中 #49 更新描述） |
| 5 | 低 | 参数变更 | 更新 `/init` — 官方文档去掉了 `[prompt]` 参数提示 | ✅ COMPLETE（在 Project 标签中 #45 移除 [prompt] 提示） |

---

## [2026-03-17 12:45 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新别名 | 向 `/fork` 条目添加"别名: /branch"（v2.1.77 将 fork 重命名为 branch） | ✅ COMPLETE（在 Session 标签中 #59 向 /fork 添加"别名: /branch"） |
| 2 | 高 | 新别名 | 向 8 个命令添加别名：`/clear`（+/reset, /new）、`/config`（+/settings）、`/desktop`（+/app）、`/exit`（+/quit）、`/rewind`（+/checkpoint）、`/resume`（+/continue）、`/remote-control`（+/rc）、`/mobile`（+/ios, /android） | ✅ COMPLETE（向所有 8 个命令描述添加了别名标记） |
| 3 | 中 | 描述变更 | 更新 `/diff` — "打开交互式差异查看器，显示未提交的更改和每轮差异" | ✅ COMPLETE（在 Project 标签中 #44 更新描述） |
| 4 | 中 | 描述变更 | 更新 `/memory` — "编辑 CLAUDE.md 记忆文件，启用或禁用自动记忆，查看自动记忆条目" | ✅ COMPLETE（在 Memory 标签中 #37 更新描述） |
| 5 | 中 | 描述变更 | 更新 `/copy` — "将最后一个助手响应复制到剪贴板。显示代码块的交互式选择器" | ✅ COMPLETE（在 Export 标签中 #27 更新描述） |
| 6 | 中 | 描述变更 | 更新 `/mobile` — "显示二维码下载 Claude 移动应用" | ✅ COMPLETE（在 Remote 标签中 #52 更新描述和别名） |
| 7 | 中 | 描述变更 | 更新 `/remote-control` — "使此会话可用于从 claude.ai 进行远程控制" | ✅ COMPLETE（在 Remote 标签中 #53 更新描述和别名） |
| 8 | 低 | Frontmatter 范围 | 6 个仅限 Skill 的字段仍未出现在报告中（有意限定范围） | ❌ INVALID（仅限 Skill 的字段 — 与 v2.1.74 运行相同的判定） |

---

## [2026-03-18 11:38 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新命令 | 在 Config 标签中添加 `/voice` — 切换按键说话语音听写 | ✅ COMPLETE（在 Config 标签中作为 #15 添加） |
| 2 | 高 | 反向别名 | 将 `/fork` → `/branch` 作为主要名称，`/fork` 作为别名 | ✅ COMPLETE（在 Session 标签中 #56 交换为 `/branch`，按字母顺序重新排序） |
| 3 | 中 | 新别名 | 向 `/permissions` 添加 `/allowed-tools` 别名 | ✅ COMPLETE（在 Config 标签中 #7 添加别名） |
| 4 | 中 | 新参数 | 向 `/copy` 添加 `[N]` 参数语法 | ✅ COMPLETE（在 Export 标签中 #28 更新为 `/copy [N]`） |
| 5 | 低 | Frontmatter 范围 | 6 个仅限 Skill 的字段未出现在报告中（有意限定范围） | ❌ INVALID（仅限 Skill 的字段 — 与 v2.1.74 和 v2.1.77 运行相同的判定） |

---

## [2026-03-19 11:54 AM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 低 | Frontmatter 范围 | 6 个仅限 Skill 的字段未出现在报告中（有意限定范围） | ❌ INVALID（仅限 Skill 的字段 — 与 v2.1.74、v2.1.77 和 v2.1.78 运行相同的判定） |

---

## [2026-03-20 08:33 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 新字段 | 向 frontmatter 表添加 `effort` — 在调用命令时覆盖模型 effort 级别（v2.1.80） | ✅ COMPLETE（添加为第 5 个字段，然后在添加完整字段集时重新定位为第 8 个） |
| 2 | 高 | QA 修正 | 添加 6 个缺失字段（`name`、`disable-model-invocation`、`user-invocable`、`context`、`agent`、`hooks`）— 官方文档说明 Commands 支持与 Skills"相同的 frontmatter"；之前 v2.1.74–v2.1.79 的 INVALID 判定不正确 | ✅ COMPLETE（添加了全部 6 个字段，计数从 5 更新为 11，字段顺序与官方文档一致） |
| 3 | 高 | 跨报告修复 | 向 Skills 报告（`claude-skills.md`）添加 `effort` — 该字段在那里也缺失 | ✅ COMPLETE（在 Skills 报告中作为第 8 个字段添加，计数从 10 更新为 11） |

---

## [2026-03-21 09:08 PM PKT] Claude Code v2.1.81

无优先操作项 — 报告与官方文档完全同步（11 个 frontmatter 字段，63 个内置命令）。

---

## [2026-03-23 09:48 PM PKT] Claude Code v2.1.81

无优先操作项 — 报告与官方文档完全同步（11 个 frontmatter 字段，63 个内置命令）。

---

## [2026-03-25 08:07 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新命令 | 在 Remote 标签中添加 `/schedule [description]` — 创建、更新、列出或运行云端定时任务 | ✅ COMPLETE（在 Remote 标签中作为 #56 添加，计数从 63 更新为 64） |

---

## [2026-03-26 01:01 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新字段 | 向 frontmatter 表添加 `shell` — `!command` 块的 shell（`bash` 或 `powershell`） | ✅ COMPLETE（在 `hooks` 之前作为第 12 个字段添加，计数从 11 更新为 12） |
| 2 | 低 | 参数变更 | 向 `/fast` 命令添加 `[on\|off]` 参数提示 | ✅ COMPLETE（在 Model 标签中 #40 将 `/fast` 更新为 `/fast [on\|off]`） |

---

## [2026-03-27 06:25 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新字段 | 向 frontmatter 表添加 `paths` — 限制 Skill 何时激活的 glob 模式 | ✅ COMPLETE（在 `user-invocable` 之后作为第 6 个字段添加，计数从 12 更新为 13） |
