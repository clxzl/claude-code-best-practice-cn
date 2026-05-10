创建一个 agent 团队来构建一个时间编排工作流，将当前迪拜时间显示为可视化 SVG 卡片。该工作流遵循 Command → Agent → Skill 架构模式：

- 一个命令编排流程并处理用户交互
- 一个 agent 使用预加载的 skill 获取迪拜的实时当前时间
- 一个 skill 从获取的数据创建可视化 SVG 时间卡片

**重要提示**：所有文件必须创建在 `agent-teams/.claude/` 内 — 不是在仓库根目录的 `.claude/` 目录中。这使 agent 团队的输出自包含且可通过 `cd agent-teams && claude` 运行。不要引用或复制现有的天气工作流 — 从头开始构建所有内容。

分配以下团队成员：

1. **命令架构师** — 在 `agent-teams/.claude/commands/time-orchestrator.md` 中设计并实现 `/time-orchestrator` 命令。该命令应：
   - 通过 Agent 工具（不是 bash）调用 time-agent 获取迪拜（阿联酋）的当前时间（Asia/Dubai 时区，UTC+4）
   - 通过 Skill 工具调用 time-svg-creator skill 从获取的时间数据渲染 SVG 卡片
   - 在 frontmatter 中使用 model: haiku
   - 包含关键要求：顺序流程、正确的工具使用（agent 使用 Agent 工具，skill 使用 Skill 工具）和输出摘要
   通过共享任务列表与其他团队成员协调，就组件间传递的数据契约（{time, timezone, formatted}）达成一致。

2. **Agent 工程师** — 在 `agent-teams/.claude/agents/time-agent.md` 中设计并实现 `time-agent`，以及其预加载的 `time-fetcher` skill 在 `agent-teams/.claude/skills/time-fetcher/SKILL.md` 中。该 agent 应：
   - 使用 Bash 和 `TZ='Asia/Dubai' date '+%Y-%m-%d %H:%M:%S %Z'` 获取迪拜（Asia/Dubai，UTC+4）的当前时间
   - 将时间值、时区名称和格式化字符串返回给命令
   - 使用 frontmatter：tools (Bash)、model: haiku、color: blue、maxTurns: 3
   - 通过 `skills:` 字段预加载 time-fetcher skill
   time-fetcher skill（`agent-teams/.claude/skills/time-fetcher/SKILL.md`）应包含迪拜时间的 bash 命令、预期输出格式，并设置 user-invocable: false，因为它是仅限 agent 的领域知识。
   将商定的数据契约发布到共享任务列表，以便命令架构师和 Skill 设计师可以对齐接口。

3. **Skill 设计师** — 在 `agent-teams/.claude/skills/time-svg-creator/SKILL.md` 中设计并实现 `time-svg-creator` skill，以及支持文件 `reference.md`（SVG 模板 + 输出模板）和 `examples.md`（示例输入/输出配对）。该 skill 应：
   - 从调用上下文中接收时间值、时区和格式化字符串
   - 为迪拜创建一个自包含的 SVG 时间卡片，显示当前时间
   - 将 SVG 写入 `agent-teams/output/dubai-time.svg`
   - 将 Markdown 摘要写入 `agent-teams/output/output.md`
   - 使用提供的确切时间 — 永远不要重新获取
   - 将模板保存在 reference.md（带占位符的 SVG 标记、Markdown 输出模板）中，示例配对保存在 examples.md 中
   同时创建 `agent-teams/output/` 目录用于输出文件。

所有三个团队成员应在共享任务列表中创建任务以协调数据契约：agent 返回 {time, timezone, formatted}，命令通过上下文传递它，skill 消费它。并行启动所有三个组件，因为它们是独立的 — 它们只需要在数据接口上达成一致，不需要等待彼此的实现。
