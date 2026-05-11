# 开发工作流变更日志

**状态图例：**

| 状态 | 含义 |
|--------|---------|
| `COMPLETE (原因)` | 已采取行动并成功解决 |
| `INVALID (原因)` | 发现不正确、不适用或属预期行为 |
| `ON HOLD (原因)` | 行动已推迟，等待外部依赖或用户决定 |

---

## [2026-03-19 05:25 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 仓库变更 | 将 humanlayer 从纯文章仓库变更为 humanlayer/humanlayer (★ 10k, 6 agents, 27 commands) | COMPLETE (用户要求，仓库有实际实现) |
| 2 | HIGH | 数量更新 | 添加了数量统计： context-hub: 0 agents · 7 skills · 7 commands | COMPLETE (之前显示为 —) |
| 3 | HIGH | 数量更新 | 添加了数量统计： agent-os: 0 agents · 0 skills · 5 commands | COMPLETE (之前显示为 —) |
| 4 | MED | 数量更新 | Updated spec-kit commands from 14 to 9+ (9 core, extensions are community-contributed) | COMPLETE (agent 确认了 9 个核心命令模板) |
| 5 | MED | 数量更新 | Updated OpenSpec commands from 10+ to 11 (confirmed exact count) | COMPLETE (agent 确认了 11 个命令) |
| 6 | MED | 数量更新 | Updated gstack from "21 skills · 21 commands" to "21 skills/commands" (skills serve as command surface) | COMPLETE (没有独立的 commands/ 目录，skills 就是命令) |
| 7 | MED | 描述 | Added uniqueness descriptions for context-hub, agent-os, humanlayer | COMPLETE (之前显示为通用描述) |
| 8 | LOW | 排序 | Moved humanlayer up from ★ 1.6k to ★ 10k position (after context-hub) | COMPLETE (仓库变更导致星标数增加) |
| 9 | LOW | 报告更新 | 更新了跨工作流分析报告 "Workflows at a Glance" table with all 9 workflows | COMPLETE (之前只有 6 个，现在包含全部 9 个并按星标排序) |

---

## [2026-03-19 05:29 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 数量更新 | Update obra/superpowers agents from 7 to 5 (v5.0.4 将审核循环合并为整体方案评估，移除了 2 个隐式 agent) | COMPLETE (已更新 README 表格和报告) |
| 2 | HIGH | 数量更新 | Update obra/superpowers skills from 44+ to 14 core (community repo obra/superpowers-skills archived Oct 2025) | COMPLETE (已更新 README 表格和报告) |
| 3 | HIGH | 数量更新 | Update spec-kit: skills 10→0 (v0.3.0 替换为预设系统), commands kept at 9+ with 22 extensions noted in report | COMPLETE (已更新 README 表格和报告) |
| 4 | HIGH | 数量更新 | Update context-hub counts from 7 skills · 7 commands to: 0 agents · 1 skill · 0 commands | COMPLETE (更正了上一次运行的不准确计数; only 1 SKILL.md in cli/skills/get-api-docs/) |
| 5 | MED | 星标更新 | Update spec-kit stars from 78k to 79k (78.5k displayed) | COMPLETE (已更新 README 表格和报告) |
| 6 | MED | 数量更新 | agent-os counts already in README from previous run: 0 agents · 0 skills · 5 commands | COMPLETE (已验证数量一致) |
| 7 | MED | 星标更新 | Update agent-os stars from 4.1k to 4k (4,100 actual) | COMPLETE (已更新 README 表格和报告) |
| 8 | MED | 报告更新 | Update cross-workflow analysis report with current counts for obra, spec-kit, context-hub, agent-os | COMPLETE (updated Workflows at a Glance table) |
| 9 | LOW | 数量更新 | OpenSpec commands: table shows 11, research found 9-11 depending on counting | INVALID (11 在发现范围内，保持当前值) |
| 10 | LOW | 独特性 | Updated spec-kit uniqueness to mention pluggable extension/preset ecosystem (v0.3.0) | COMPLETE (将"预实现门控"替换为"可插拔扩展/预设生态系统") |

---

## [2026-03-20 08:37 AM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 星标更新 | Update Superpowers ★ from 98k to 100k (99,603 actual — 接近 10 万星标里程碑) | COMPLETE (已更新 README 表格) |
| 2 | HIGH | 星标更新 | Update Everything Claude Code ★ from 87k to 89k (88,580 actual) | COMPLETE (已更新 README 表格) |
| 3 | HIGH | 星标更新 | Update Get Shit Done ★ from 35k to 36k (36,307 actual) | COMPLETE (已更新 README 表格) |
| 4 | HIGH | 数量更新 | Update Get Shit Done commands from 46 to 50 (v1.26.0 added /gsd:ship, /gsd:next, /gsd:do, /gsd:ui-phase) | COMPLETE (已更新 README 表格) |
| 5 | MED | 星标更新 | Update gstack ★ from 26k to 29k (28,889 actual — v0.9.0 multi-AI expansion) | COMPLETE (已更新 README 表格) |
| 6 | MED | 数量更新 | Update BMAD-METHOD skills from 43 to 42 (v6.2.0 recount: 30 bmm-skills + 12 core-skills) | COMPLETE (已更新 README 表格) |
| 7 | LOW | 排序 | 按规划类型分组重排表格（命令 → agent → skill，组内按星标降序） | COMPLETE (commands: Spec Kit, OpenSpec, HumanLayer; agents: ECC, GSD; skills: Superpowers, BMAD, gstack) |

---

## [2026-03-21 09:20 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 星标更新 | Update Superpowers ★ from 100k to 103k (102,767 actual) | COMPLETE (已更新 README 表格) |
| 2 | HIGH | 星标更新 | Update Everything Claude Code ★ from 89k to 93k (93,145 actual) | COMPLETE (已更新 README 表格) |
| 3 | HIGH | 数量更新 | Update ECC agents 25→28, commands 57→59, skills 108+→116 (v1.9.0: selective install, ECC Tools Pro, 12 lang ecosystems) | COMPLETE (已更新 README 表格) |
| 4 | HIGH | 星标更新 | Update Get Shit Done ★ from 36k to 38k (37,748 actual) | COMPLETE (已更新 README 表格) |
| 5 | HIGH | 数量更新 | Update GSD agents 16→18, commands 50→52 (v1.27.0: advisor mode, multi-repo workspaces, /gsd:fast, /gsd:review) | COMPLETE (已更新 README 表格) |
| 6 | HIGH | 星标更新 | Update gstack ★ from 29k to 34k (34,456 actual — v0.9.4 Codex reviews, Windows 11 support) | COMPLETE (已更新 README 表格) |
| 7 | HIGH | 架构变更 | Update BMAD agents from 9 to 0 (v6.x pure skills rewrite — agent 角色现在作为 skill 实现在 bmm-skills/ 中) | COMPLETE (已更新 README 表格) |
| 8 | MED | 星标更新 | Update BMAD ★ from 41k to 42k (41,629 actual) | COMPLETE (已更新 README 表格) |
| 9 | MED | 星标更新 | Update OpenSpec ★ from 32k to 33k (32,862 actual) | COMPLETE (已更新 README 表格) |
| 10 | MED | 排序 | Swap gstack (34k) above OpenSpec (33k) — 星标降序 | COMPLETE (已更新 README 表格) |

---

## [2026-03-23 09:53 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 星标更新 | Update Superpowers ★ from 103k to 107k (107,308 actual) | COMPLETE (已更新 README 表格) |
| 2 | HIGH | 星标更新 | Update ECC ★ from 93k to 101k (101,098 actual — 突破 10 万星标里程碑！) | COMPLETE (已更新 README 表格) |
| 3 | HIGH | 数量更新 | Update ECC commands 59→60, skills 116→125 (v1.9.0 continued: new skills pytorch-patterns, documentation-lookup, claude-devfleet, prompt-optimizer) | COMPLETE (已更新 README 表格) |
| 4 | HIGH | 星标更新 | Update gstack ★ from 34k to 41k (41,224 actual — v0.9.x multi-AI expansion, CSO security audit) | COMPLETE (已更新 README 表格) |
| 5 | HIGH | 数量更新 | Update gstack skills 21→27 (6 new: gstack-autoplan, gstack-benchmark, gstack-cso, gstack-design-consultation, gstack-office-hours, gstack-freeze/unfreeze) | COMPLETE (已更新 README 表格) |
| 6 | HIGH | 排序 | Move gstack (41k) above GSD (40k) — 星标降序 | COMPLETE (已更新 README 表格) |
| 7 | HIGH | 星标更新 | Update GSD ★ from 38k to 40k (39,588 actual) | COMPLETE (已更新 README 表格) |
| 8 | HIGH | 数量更新 | Update GSD commands 52→57 (v1.28.0: /gsd:forensics, /gsd:milestone-summary, /gsd:plant-seed, /gsd:profile-user, /gsd:workstreams) | COMPLETE (已更新 README 表格) |
| 9 | MED | 星标更新 | Update Spec Kit ★ from 79k to 81k (81,349 actual — v0.4.0 embedded core pack, 24 platform support) | COMPLETE (已更新 README 表格) |
| 10 | MED | 规划更新 | Update gstack Plan from plan-eng-review to autoplan (higher-level orchestrator that reads CEO, design, eng review sequentially) | COMPLETE (已更新 README 表格) |
| 11 | LOW | 数量更新 | Update OpenSpec commands 11→10 (recount: /opsx:propose, apply, archive, new, continue, ff, verify, sync, bulk-archive, onboard) | COMPLETE (已更新 README 表格) |
| 12 | LOW | 数量更正 | Correct OpenSpec skills 11→0 (no skills/ or .claude/skills/ directory exists — OpenSpec is a CLI tool, not skills-based) | COMPLETE (已更新 README 表格) |

---

## [2026-03-24 08:12 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 星标更新 | Update Superpowers ★ from 107k to 110k (109,846 actual) | COMPLETE (已更新 README 表格) |
| 2 | HIGH | 星标更新 | Update ECC ★ from 101k to 104k (103,960 actual) | COMPLETE (已更新 README 表格) |
| 3 | HIGH | 星标更新 | Update gstack ★ from 41k to 44k (44,300 actual — v0.11.x triple-voice multi-model review) | COMPLETE (已更新 README 表格) |
| 4 | HIGH | 排序 | Move gstack (44k) above BMAD (42k) — 星标降序 | COMPLETE (已更新 README 表格) |
| 5 | HIGH | 数量更新 | Update BMAD skills from 42 to 44 (recount: 32 bmm-skills + 12 core-skills, including 3 nested research sub-skills) | COMPLETE (已更新 README 表格) |
| 6 | HIGH | 数量更新 | Update gstack skills from 27 to 28 (README states 28; 27 confirmed individually) | COMPLETE (已更新 README 表格) |
| 7 | MED | 星标更新 | Update Spec Kit ★ from 81k to 82k (81,780 actual) | COMPLETE (已更新 README 表格) |
| 8 | MED | 星标更新 | Update GSD ★ from 40k to 41k (40,500 actual) | COMPLETE (已更新 README 表格) |
| 9 | MED | 星标更新 | Update OpenSpec ★ from 33k to 34k (33,800 actual) | COMPLETE (已更新 README 表格) |

---

## [2026-03-25 08:12 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 星标更新 | Update Superpowers ★ from 110k to 112k (112,163 actual) | COMPLETE (已更新 README 表格) |
| 2 | HIGH | 星标更新 | Update ECC ★ from 104k to 107k (106,913 actual) | COMPLETE (已更新 README 表格) |
| 3 | HIGH | 数量更新 | Update ECC commands from 60 to 63 (3 new in .claude/commands/: add-language-rules, database-migration, feature-development) | COMPLETE (已更新 README 表格) |
| 4 | HIGH | 星标更新 | Update gstack ★ from 44k to 47k (46,703 actual — infrastructure hardening, test coverage gates) | COMPLETE (已更新 README 表格) |
| 5 | MED | 数量更新 | Update BMAD skills from 44 to 42 (recount: 30 bmm-skills + 12 core-skills; v6.2.1 consolidated 2 sub-skills) | COMPLETE (已更新 README 表格) |
| 6 | LOW | 数量更新 | Update gstack skills from 28 to 27 (27 root-level dirs confirmed; 28th may be root SKILL.md template) | COMPLETE (已更新 README 表格) |

---

## [2026-03-26 01:05 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 星标更新 | Update Superpowers ★ from 112k to 114k (114,107 actual) | COMPLETE (已更新 README 表格) |
| 2 | HIGH | 星标更新 | Update ECC ★ from 107k to 109k (108,839 actual) | COMPLETE (已更新 README 表格) |
| 3 | HIGH | 星标更新 | Update gstack ★ from 47k to 48k (48,303 actual) | COMPLETE (已更新 README 表格) |
| 4 | HIGH | 星标更新 | Update GSD ★ from 41k to 42k (42,092 actual) | COMPLETE (已更新 README 表格) |
| 5 | MED | 数量更新 | Update OpenSpec commands from 10 to 11 (v1.2.0 added /opsx:explore) | COMPLETE (已更新 README 表格) |

---

## [2026-03-27 06:32 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|------|------|------|------|
| 1 | HIGH | 星标更新 | Update Superpowers ★ from 114k to 118k (117,568 actual) | COMPLETE (已更新 README 表格) |
| 2 | HIGH | 星标更新 | Update ECC ★ from 109k to 111k (111,487 actual) | COMPLETE (已更新 README 表格) |
| 3 | HIGH | 星标更新 | Update gstack ★ from 48k to 52k (51,544 actual — v0.12.x skill namespacing, Codex fallback, worktree parallelization) | COMPLETE (已更新 README 表格) |
| 4 | HIGH | 数量更新 | Update gstack skills from 27 to 31 (4 new: canary, codex, connect-chrome, land-and-deploy among others) | COMPLETE (已更新 README 表格) |
| 5 | HIGH | 星标更新 | Update GSD ★ from 42k to 43k (43,136 actual) | COMPLETE (已更新 README 表格) |
| 6 | HIGH | 排序 | Swap GSD (43,136) above BMAD (42,529) — 两者都约为 43k 但 GSD 星标更多 | COMPLETE (已更新 README 表格) |
| 7 | MED | 星标更新 | Update Spec Kit ★ from 82k to 83k (82,878 actual) | COMPLETE (已更新 README 表格) |
| 8 | MED | 星标更新 | Update BMAD ★ from 42k to 43k (42,529 actual) | COMPLETE (已更新 README 表格) |
| 9 | MED | 星标更新 | Update OpenSpec ★ from 34k to 35k (34,821 actual) | COMPLETE (已更新 README 表格) |
| 10 | MED | 数量更新 | Update Compound Engineering agents from 43 to 47 (4 new review/workflow agents) | COMPLETE (已更新 README 表格) |
| 11 | MED | 数量更新 | Update Compound Engineering skills from 44 to 42 (recount: 41 compound-engineering + 1 coding-tutor) | COMPLETE (已更新 README 表格) |