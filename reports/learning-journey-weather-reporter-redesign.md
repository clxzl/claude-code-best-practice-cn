# 学习旅程 — 天气播报员重新设计计划

← 返回 [README](../README.md)

## 概述

从第 7 张幻灯片开始，围绕一个贯穿始终的示例重新设计所有幻灯片：**天气播报员 Agent**。叙事弧线与可见的目录顺序匹配（Agents → Skills → Context → CLAUDE.md → Commands+Workflow），让观众先认识天气播报员，然后了解他们知道什么、如何思考、遵循什么规则，最后如何用一个命令触发他们。

---

## 1. 当前 → 新版块映射

| 当前版块 | 当前幻灯片 | 操作 | 新位置 |
|---|---|---|---|
| 主题 1: Context | 7-11（版块起始于 7） | 移至主题 3 | 幻灯片 17-21 |
| 主题 2: CLAUDE.md | 12-17（版块起始于 12） | 移至主题 4 | 幻灯片 22-27 |
| 主题 3: Agents | 18-23（版块起始于 18） | 移至主题 1 | 幻灯片 7-12 |
| 主题 4: Skills | 24-29（版块起始于 24） | 移至主题 2 | 幻灯片 13-18 |
| 主题 5: Commands | 30-32（版块起始于 30） | 与 Workflow 合并为主题 5 | 幻灯片 28-32 |
| 主题 6: Workflow | 33-36（版块起始于 33） | 合并入 Commands 版块 | （无单独版块幻灯片） |
| 结束幻灯片 | 37 | 保留，更新副标题 | 幻灯片 33 |

**新版总计：33 张幻灯片**（与当前 37 张相同，减去 Workflow 版块幻灯片和 3 张 Workflow 内容幻灯片合并入 Commands 版块，Commands 版块增加这些幻灯片）。

等一下 — 让我重新计算：

当前：幻灯片 7-37 = 31 张幻灯片。
- Agents 版块：6 张（18-23）→ 变为主题 1（7-12）
- Skills 版块：6 张（24-29）→ 变为主题 2（13-18）
- Context 版块：5 张（7-11）→ 变为主题 3（19-23）
- CLAUDE.md 版块：6 张（12-17）→ 变为主题 4（24-29）
- Commands+Workflow 合并：3 + 1 版块 + 4 内容 = Commands（3）+ Workflow（1 版块 + 3 内容）= 7 张幻灯片 → 变为主题 5（30-36）
- 结束：1 张幻灯片（37）

**新版总计：37 张幻灯片。**（没有幻灯片被删除；Workflow 版块幻灯片成为合并的 Commands+Workflow 版块的一部分 — 我们保留它作为子版块或删除其 `data-level` 以避免第二个版块分隔符。）

**决定**：保留全部 37 张幻灯片。在旧的 Workflow 版块分隔符（幻灯片 33）上删除 `data-level`，使其被视为内容幻灯片而非版块分隔符。Commands 版块覆盖 30-36。Workflow 版块分隔符成为 Commands 版块内的可视化"章节标题"。

实际上，更简单：将 Workflow 版块分隔符保留为没有 `data-level` 的内容幻灯片。进度条停留在 `commands` 级别。版块编号文本从"主题 6"改为只是一个子标题。

---

## 2. 新 LEVELS 映射（键和颜色不变）

新版块顺序为：**Agents → Skills → Context → CLAUDE.md → Commands**。`workflow` 级别键从 `data-level` 使用中退役（版块分隔符失去 `data-level`）。`LEVELS` 映射仍然保留 `workflow` 用于进度条历史显示，但没有幻灯片触发它。

**修订方案**：从 LEVELS 映射中完全删除 `workflow` 级别，因为没有幻灯片携带 `data-level="workflow"`。进度条最高到 `commands`（83%）。这没问题 — Workflow 版块作为 Commands 版块内的高潮呈现，而不是单独的主题。

实际上进度条停在 83% 而不是 100% 对于结束版块来说不太令人满意。更好的方案：**将 Commands+Workflow 合并为一个称为"Commands & Workflow"的版块**，使用 `data-level="commands"`。在 LEVELS 中保留 `workflow` 级别在 100%，并将 `data-level="workflow"` 分配给*旧的* Workflow 版块分隔符幻灯片 — 它成为 Commands 版块内的可视化过渡。这样进度条在 workflow 幻灯片处填满到 100%。

**最终决定**：在 LEVELS 中保留 `commands`（83%）和 `workflow`（100%）。将 `data-level="commands"` 分配给 Commands 版块分隔符，将 `data-level="workflow"` 分配给 Workflow 子版块幻灯片。进度刻度保持不变。这与当前结构完全匹配 — 只是内容幻灯片重新排序。

---

## 3. 逐张幻灯片内容大纲

### 幻灯片 1-6（不变）

幻灯片 1（标题）、2（Boris GIF）、3（Vibe→Agentic）、4（什么是 Vibe Coding）、5（好提示 vs 坏提示）、6（目录 — 仅更新 goToSlide 目标）。

**幻灯片 6 上的目录更新：**
- Agents 行：`goToSlide(7)`（原为 18）
- Skills 行：`goToSlide(13)`（原为 24）
- Context 行：`goToSlide(19)`（原为 7）
- CLAUDE.md 行：`goToSlide(25)`（原为 12）
- Commands 行：`goToSlide(30)`（原为 30 — 不变）

---

### 版块 1: Agents（幻灯片 7-12）— "那个人"

**幻灯片 7** — 版块分隔符（`data-level="agents"`，主题 1）
- 标题："Agents — 天气播报员"
- 描述："Agent 是 Claude 扮演的特定角色。认识天气播报员 — 一位专门为迪拜获取和播报天气数据的专家。"

**幻灯片 8** — "餐厅厨房"（当前幻灯片 19）
- 内容：相同的类比（普通提示 = 在随机厨房里喊叫；Agent = 特定专家）
- 将 Agent 示例更新为全程使用"天气播报员"框架
- 保留双列卡片比较普通提示 vs weather-agent

**幻灯片 9** — "提示 vs. Agent — 对比"（当前幻灯片 20）
- 保留表格不变。已经很好地使用了天气示例。

**幻灯片 10** — "Agent 拥有自己的大脑"（当前幻灯片 21）
- 保留 Thariq 的提示。将其联系到："天气播报员在自己的大脑中工作 — 所有网络获取都远离你的上下文。"

**幻灯片 11** — "如何创建自己的 Agent"（当前幻灯片 22）
- 保留 `/agents` 操作指南模式
- 更新代码块以显示真实的 `weather-agent.md` 路径

**幻灯片 12** — "Agent 配置字段"（当前幻灯片 23）
- 保留字段行表格。添加一个展示 `skills: [weather-fetcher]` 字段在上下文中的提示框。

---

### 版块 2: Skills（幻灯片 13-18）— "播报员知道什么"

**幻灯片 13** — 版块分隔符（`data-level="skills"`，主题 2）
- 标题："Skills — 天气播报员知道什么"
- 描述："Skills 是播报员经过训练的特定能力。我们的播报员有两个：获取数据和将其渲染为卡片。"

**幻灯片 14** — "培训手册"（当前幻灯片 25）
- 重新框定：天气播报员有两个 Skill：weather-fetcher（获取温度）和 weather-svg-creator（创建视觉卡片）。
- 用天气播报员的两个 Skill 替换"Shayan"示例。

**幻灯片 15** — "何时将某些东西变成 Skill"（当前幻灯片 26）
- 保留 Boris 的提示。添加 weather-fetcher 和 weather-svg-creator 作为其中两个示例。

**幻灯片 16** — "为什么要分开 Agent 和 Skill？"（当前幻灯片 27）
- 保留双列。更新以强调：weather-agent = 那个人，weather-fetcher = 他们的培训。

**幻灯片 17** — "如何创建自己的 Skill"（当前幻灯片 28）
- 保留。代码块已经显示了真实的 `weather-fetcher` SKILL.md 内容 — 完美。

**幻灯片 18** — "Skill 配置字段"（当前幻灯片 29）
- 保留。添加说明：`user-invocable: false` 在 weather-fetcher 上设置，因为它仅供 Agent 使用。

---

### 版块 3: Context（幻灯片 19-23）— "播报员的大脑"

**幻灯片 19** — 版块分隔符（`data-level="context"`，主题 3）
- 标题："Context — 播报员的大脑"
- 描述："既然你已经认识了播报员并了解了他们的 Skill，让我们来理解他们实际上能同时在头脑中容纳什么。"

**幻灯片 20** — "Claude 的大脑"（当前幻灯片 8）
- 保留。添加一句话联系到天气播报员："当 weather-agent 被派遣时，它获得自己全新的大脑 — weather-fetcher 在启动时被固定到其中。"
- 保留两张图（context-window.jpeg 保留在这里）。

**幻灯片 21** — "会话启动时加载什么"（当前幻灯片 9）
- 保留。联系到天气播报员："启动时，Claude *知道* weather-fetcher（仅描述）。当命令运行时，完整的 Skill 内容被加载到 Agent 的大脑中。"
- 保留 context.jpg 在这里。

**幻灯片 22** — "保持大脑清晰"（当前幻灯片 10）
- 保留分支点表格。

**幻灯片 23** — "如何管理你的上下文"（当前幻灯片 11）
- 保留 `/context`、`/compact`、`/clear` 操作指南。

---

### 版块 4: CLAUDE.md（幻灯片 24-29）— "口袋规则手册"

**幻灯片 24** — 版块分隔符（`data-level="claude-md"`，主题 4）
- 标题："CLAUDE.md — 播报员的口袋规则手册"
- 描述："播报员在每次上班开始时都会查阅这本手册 — 即使他们的大脑在过夜后会重置。"

**幻灯片 25** — "员工手册"（当前幻灯片 13）
- 保留。更新为天气播报员框架：CLAUDE.md 是播报员在上直播前阅读的规则手册 — "除非被要求，始终以摄氏度报告，始终引用来源。"

**幻灯片 26** — "如何创建你的 CLAUDE.md"（当前幻灯片 14）
- 保留 `/init` 操作指南。

**幻灯片 27** — "随着每次错误成长 CLAUDE.md"（当前幻灯片 15）
- 保留 Boris 的提示。

**幻灯片 28** — "CLAUDE.md 中放什么"（当前幻灯片 16）
- 保留代码块。天气播报员触感：添加一条注释显示天气特定规则。

**幻灯片 29** — "CLAUDE.md 如何加载"（当前幻灯片 17）
- 保留。

---

### 版块 5: Commands + Workflow（幻灯片 30-36）— "触发器"

**幻灯片 30** — 版块分隔符（`data-level="commands"`，主题 5）
- 标题："Commands — 触发器"
- 描述："一个词就能启动整个链条。`/weather-orchestrator` → agent → skill → SVG 卡片。"

**幻灯片 31** — "Commands — 入口点"（当前幻灯片 31）
- 保留。很好的介绍。已经引用了 weather-orchestrator。

**幻灯片 32** — "如何创建自己的 Command"（当前幻灯片 32）
- 保留。代码块已经显示了 weather-orchestrator.md。

**幻灯片 33** — Workflow 子版块（原幻灯片 33，`data-level="workflow"`）
- 将版块编号文本从"主题 6"改为"整合一切"
- 保留 `data-level="workflow"` 使进度条填满到 100%。
- 更新标题为："Workflow — 五个部分合为一体"
- 描述："观看天气播报员示例从一次按键运行到 SVG 卡片输出。"

**幻灯片 34** — "Command → Agent → Skill"（当前幻灯片 34）
- 保留代码块流程图。已经很完美。

**幻灯片 35** — "Skill 的两种使用方式"（当前幻灯片 35）
- 保留双列比较预加载 vs 直接调用。

**幻灯片 36** — "如何连接你自己的 Workflow"（当前幻灯片 36）
- 保留。已经使用天气工作流作为示例。

**幻灯片 37** — 结束（当前幻灯片 37）
- 保留。更新副标题为："五个概念，一个贯穿示例"
- 更新正文引用天气播报员叙事弧线。

---

## 4. 资源复用清单

| 资源 | 当前位置 | 新位置 | 操作 |
|---|---|---|---|
| `context-window.jpeg` | 幻灯片 8（Claude 的大脑） | 幻灯片 20（相同内容，重新编号） | 保留 — 无需更改 |
| `context.jpg` | 幻灯片 9（会话启动时加载什么） | 幻灯片 21（相同内容，重新编号） | 保留 — 无需更改 |
| `../../!/claude-jumping.svg` | 幻灯片 1、页眉 | 不变 | 无操作 |
| `../../!/root/boris-slider.gif` | 幻灯片 2 | 不变 | 无操作 |

两张上下文图表都原封不动地保留 — 包含它们的幻灯片只是被重新编号（8→20、9→21）。

---

## 5. 记账影响

### 新版块分隔符位置和 `data-level` 分配

| 幻灯片 | 主题 | `data-level` |
|---|---|---|
| 7 | Agents | `agents` |
| 13 | Skills | `skills` |
| 19 | Context | `context` |
| 25 | CLAUDE.md | `claude-md` |
| 30 | Commands | `commands` |
| 33 | Workflow 子版块 | `workflow` |

### 幻灯片 6 上的目录 `goToSlide` 目标

| 行 | 主题 | 旧目标 | 新目标 |
|---|---|---|---|
| 行 1 | Agents | 18 | 7 |
| 行 2 | Skills | 24 | 13 |
| 行 3 | Context | 7 | 19 |
| 行 4 | CLAUDE.md | 12 | 25 |
| 行 5 | Commands | 30 | 30 |

### 进度刻度（不变）

进度刻度轨道已经按从上到下排序为：Workflow、Commands、Skills、Agents、CLAUDE.md、Context。这是叙事弧线顺序的*反转*（顶部 = 最高级别 = 最后达成）。无需更改。

### LEVELS 映射（不变）

所有 6 个级别键（`context`、`claude-md`、`agents`、`skills`、`commands`、`workflow`）保留。无键添加或删除。

---

## 6. 实施方法

HTML 是一个大文件。幻灯片的顺序不适合新的叙事弧线。最干净的实现是：

1. 剪切幻灯片 div 并按新顺序粘贴（7-12 = 旧 18-23、13-18 = 旧 24-29、19-23 = 旧 7-11、24-29 = 旧 12-17、30-37 不变）。
2. 将所有 `data-slide` 属性按顺序重新编号。
3. 更新版块幻灯片的 `data-level` 属性。
4. 更新版块分隔符上的版块编号文本和 h1。
5. 更新幻灯片 6 上的目录 `goToSlide` 目标。
6. 更新 Workflow 版块幻灯片（旧 33）的版块编号文本。
7. 在需要的地方进行针对性的内容编辑以适配天气播报员框架。

总幻灯片数：**37**（不变）。

---

## 7. 歧义 — 无影响性

所有歧义已在上面解决。直接进入实施。
