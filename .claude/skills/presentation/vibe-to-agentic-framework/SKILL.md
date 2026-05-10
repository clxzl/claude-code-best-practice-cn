---
name: vibe-to-agentic-framework
description: 演示文稿背后的概念框架 —— "从 Vibe Coding 到 Agentic Engineering"的含义、旅程为何如此结构化，以及每张幻灯片如何契合叙事弧线
---

# "从 Vibe Coding 到 Agentic Engineering"框架

本技能教授演示文稿背后的**概念模型**。每张幻灯片和每个章节都在讲述一个故事：开发者如何从无结构的"vibe coding"（低级）逐步进阶到高级的 agentic engineering（高级）。

## 核心概念

**Vibe Coding（低级）** 是指开发者在没有任何结构的情况下使用 Claude Code —— 没有项目上下文、没有约定、没有可复用的知识。每次提示都像抛硬币一样。Claude 可能会创建随机的端点、忽略现有模式、跳过测试，并生成不一致的代码。代码库在每次交互中都趋向混乱。

**Agentic Engineering（高级）** 是指 Claude Code 作为一个完全配置的工程系统运行。它了解项目架构（CLAUDE.md）、遵循作用域约定（Rules）、按需加载领域专业知识（Skills）、委派给专业工作者（Agents）、编排多步骤工作流（Commands）、自动化生命周期事件（Hooks），并连接外部工具（MCP Servers）。每次提示都能产生一致的、经过测试的、生产就绪的代码。

这两个极端之间的旅程是**渐进且累积的**。每个最佳实践都建立在前一个之上，演示文稿按照开发者应采用的顺序来教授它们。

## 4 级旅程系统

演示文稿使用 4 级评分系统，而非百分比进度条：

| 级别 | 顺序 | 颜色 | 旅程条高度 | 描述 |
|-------|-------|-------|--------------------|-------------|
| Low | 1 | 红/橙色 (`hsl(0, 70%, 45%)`) | 25% | Vibe coding 地带 —— 无结构 |
| Medium | 2 | 黄色 (`hsl(40, 70%, 45%)`) | 50% | 结构化工作流，部分自动化 |
| High | 3 | 浅绿色 (`hsl(80, 70%, 45%)`) | 75% | 领域知识、Skills、自定义 Agents |
| Pro | 4 | 深绿色 (`hsl(120, 70%, 45%)`) | 100% | 完全的 agentic engineering，多代理团队 |

旅程条在第 1 张幻灯片（标题幻灯片）上隐藏，从第 2 张幻灯片开始显示。级别通过关键过渡幻灯片上的 `data-level` 属性设置，并被后续幻灯片继承，直到下一次级别变更。当级别变更时，`.level-badge` 通过 JS 注入到幻灯片的 `h1` 上（不要在 HTML 中硬编码这些）。

## 贯穿示例：TodoApp Monorepo

每项技术都在一个真实的全栈项目上进行演示。演示文稿展示了从普通项目（vibe coding）到具有完整 Claude Code 配置（agentic engineering）的项目的转变：

**改造前（Vibe Coding）：**
```
todoapp/
├── backend/          # FastAPI (Python)
│   ├── main.py
│   ├── routes/
│   ├── models/
│   └── tests/
└── frontend/         # Next.js (TypeScript)
    ├── components/
    ├── pages/
    └── lib/
```

**改造后（Agentic Engineering）：**
```
todoapp/
├── .claude/                  # Claude Code 配置
│   ├── agents/               # 自定义 Subagents
│   ├── skills/               # 领域知识
│   ├── commands/             # Slash Commands
│   ├── hooks/                # 生命周期脚本
│   ├── rules/                # 模块化指令
│   ├── settings.json         # 团队设置
│   └── settings.local.json   # 个人设置
├── backend/
│   └── CLAUDE.md             # 后端指令
├── frontend/
│   └── CLAUDE.md             # 前端指令
├── .mcp.json                 # 托管的 MCP 服务器
└── CLAUDE.md                 # 项目指令
```

**为什么选择 TodoApp？** 它足够小，可以在幻灯片上展示，但又足够复杂，能演示真实问题：一个具有路由模式和测试约定的后端，一个具有组件层次结构和设计令牌的前端，以及一个 monorepo 结构，其中跨领域关注点（如添加新功能）需要两侧协调。

TodoApp 使 vibe coding 问题变得具体：没有结构时，要求 Claude "添加笔记功能"会产生一个随机的 `/api/notes` 端点，不遵循 `routes/todos.py` 的模式，一个没有侧边栏导航的独立页面，以及零测试。有了完整的 agentic 设置，相同的请求会产生遵循现有模式的路由、集成到侧边栏的页面，以及匹配 `test_todos.py` 风格的测试。

## 旅程弧线：为什么是这个顺序

演示文稿遵循一个刻意设计的教学序列。每个章节解锁一个新的能力层：

### 第 0 部分：介绍（幻灯片 1-4，无权重）
**目的：** 铺垫。介绍 TodoApp，定义 vibe coding，并展示目标。
- 标题幻灯片建立旅程隐喻
- 示例项目展示转变：TodoApp 的前后对比 —— 普通项目结构 vs 具有完整 Claude Code 配置（.claude/、CLAUDE.md、.mcp.json 等）的项目
- "什么是 Vibe Coding？"建立 0% 基线 —— 痛点
- 旅程地图提供一个可点击的目录，展示前方的完整路径

### 第 1 部分：前置准备（幻灯片 5-9，无权重）
**目的：** 安装并运行 Claude Code。这纯粹是后勤工作 —— 还没有工程实践。
- 安装、认证、第一次会话、界面概述
- 无权重，因为知道如何安装工具并不能提高代码质量
- "第一次会话"本身就是 vibe coding —— 这是有意为之的，让开发者亲身体验 0% 的状态

### 第 2 部分：更好的提示（幻灯片 10-17，级别：Low）
**目的：** 第一个真正的改进。即使没有任何项目配置，更好的输入也能产生更好的输出。
- **好提示 vs 坏提示：** 具体、有范围的提示 vs 模糊的请求。最简单的改进。
- **提供上下文：** 使用 `@files` 给 Claude 它需要的代码。立即减少幻觉。
- **上下文窗口与 /compact：** 理解有限的上下文窗口可以防止长会话中响应质量下降。
- **Plan Mode：** `/plan` 强制在编码前思考。防止在错误方案上浪费精力。

**为什么是 Low 级别：** 提示是基础但有限的。它改善单次交互但不会创建持久的项目知识。每次会话都从零开始。

### 第 3 部分：项目记忆（幻灯片 18-24，级别：Medium）
**目的：** 从会话级知识到项目级知识的飞跃。Claude 现在可以跨会话记忆。
- **CLAUDE.md 与 /init：** 项目的"面向 Claude 的 README"。建立架构、技术栈和约定。这是最具影响力的单个文件。
- **应包含什么：** 编写有效 CLAUDE.md 内容的实用指南（保持在 150 行以内，聚焦 Claude 需要知道的内容）。
- **Rules：** `.claude/rules/` 中的路径作用域约定。Rules 是乘数 —— 它们自动应用于每个匹配的文件，无需开发者额外努力就能强制一致性。一个 `backend-testing.md` 规则就能确保每个测试永远遵循相同的模式。

**为什么是 Medium 级别：** 项目记忆将 Claude 从无状态工具转变为上下文感知的协作者。但仅有知识并不能创建工作流。

### 第 4 部分：结构化工作流（幻灯片 25-28，级别：Medium）
**目的：** 防止浪费精力并提高执行质量的系统方法。
- **任务列表：** 将复杂工作分解为可追踪的步骤。防止范围蔓延并确保完整性。
- **模型选择：** 选择合适的模型（Opus 用于架构，Sonnet 用于实现，Haiku 用于快速任务）优化成本和质量。

**为什么仍然是 Medium 级别：** 工作流很重要但概念相对简单。它们建立在第 3 部分的项目记忆之上，并更系统地使用它。跃升到 High 需要领域知识。

### 第 5 部分：领域知识（幻灯片 29-33，级别：High）
**目的：** 可复用的、按需的专业知识。Skills 是静态记忆（CLAUDE.md/Rules）和动态 Agents 之间的桥梁。
- **什么是 Skills：** Skills 作为打包的领域知识，Claude 在相关时加载。渐进式披露的概念。
- **创建 Skills：** 实操：为 TodoApp 构建一个 `frontend-conventions` 技能，教授 Tailwind 令牌、组件模式和侧边栏集成。
- **Skills Frontmatter 与调用：** 技术细节：YAML frontmatter、手动 vs 自动发现调用、`context: fork` 选项。

**为什么是 High 级别：** Skills 是第一个"乘数"概念 —— 一个技能定义改善其领域内的每次未来交互。但 Skills 是被动知识；它们需要 Agents 才能变得主动。

### 第 6 部分：Agentic Engineering（幻灯片 34-46，级别：High）
**目的：** 本演示文稿涵盖的目标。自主的、专业的 Agents 协调构建端到端功能。
- **什么是 Agents：** 具有受限工具和预加载 Skills 的专业 Subagents 的概念。
- **前端工程师 Agent：** 一个具体的 Agent，使用 TodoApp 的前端约定、将路由添加到侧边栏、遵循设计令牌。前后对比展示转变。
- **后端工程师 Agent：** 后端的并行 Agent —— 遵循 FastAPI 路由模式、SQLAlchemy 模型、编写匹配现有风格的测试。
- **Commands 与编排：** 压轴模式：Command → Agent → Skills。一个 `/add-feature` 命令协调前端 + 后端 Agents，每个都有自己的 Skills，交付完整的功能。这是架构的巅峰。
- **Hooks 与 MCP：** 生命周期自动化（预提交检查、声音通知）和外部工具集成。最后的自动化层。
- **Command → Agent → Skills：** 完整的架构图。展示所有组件如何连接：Commands 调用 Agents，Agents 加载 Skills，Skills 提供知识。这是"High level"的理解幻灯片。

**为什么是 High 级别：** 本节涵盖本演示文稿中教授的最高价值实践。之前的一切都是为此做准备。编排和 agentic 工作流代表了本课程涵盖的天花板 —— 完整的 Pro（多代理团队、高级编排模式）超出了本演示文稿的范围。

### High Level 幻灯片（幻灯片 44）
庆祝时刻。展示完整的 TodoApp 配置：
- CLAUDE.md 用于项目上下文
- Rules 用于路径作用域约定
- Skills 用于领域知识
- Agents 用于一致执行
- Commands 用于编排工作流
- Hooks 用于生命周期自动化
- MCP 服务器用于外部工具

### 附录（幻灯片 47+，无权重）
**目的：** 参考材料。每个命令、设置和配置选项。无权重，因为这些是参考查询，不是旅程里程碑。包括：工具使用、所有 Slash Commands、提交/PR 工作流、自定义选项、调试技巧和黄金法则。

## 编辑幻灯片时如何使用此框架

创建或修改幻灯片时，考虑：

1. **这个概念在旅程中的位置？** 关于"提示中更好的错误消息"的幻灯片属于第 2 部分（提示，Low 级别）。关于"Agent 记忆作用域"的幻灯片属于第 6 部分（agentic，High 级别）。

2. **前后对比是什么？** 每张重要幻灯片都应该隐式或显式地展示对比：在 Low 级别（vibe coding）时会发生什么 vs 使用此技术后会发生什么。使用 TodoApp 使其具体化。

3. **级别分配是否合理？** 级别转换发生在 Part 章节边界。章节内的单张幻灯片继承该章节的级别。

4. **是否建立在之前的基础上？** Skills 假设开发者已经了解 CLAUDE.md 和 Rules。Agents 假设他们了解 Skills。Commands 假设他们了解 Agents。不要在概念所属章节之前引用它。

5. **使用 TodoApp。** 抽象的解释会让观众失去兴趣。展示实际的 `routes/todos.py` 代码、实际的 `Sidebar.tsx` 组件、实际的 `CLAUDE.md` 内容。贯穿示例是使框架具体化的关键。

## 级别转换参考表

| 幻灯片 | 幻灯片名称 | data-level | 级别标签 |
|-------|-----------|------------|-------------|
| 10 | Better Prompting（章节分隔符） | `data-level="low"` | Low |
| 18 | Project Memory（章节分隔符） | `data-level="medium"` | Medium |
| 29 | Domain Knowledge（章节分隔符） | `data-level="high"` | High |
| 34 | Agentic Engineering（章节分隔符） | `data-level="high"` | High |

所有其他幻灯片继承在其之前设置的最后一个 `data-level` 属性的级别。幻灯片 1-9（介绍 + 前置准备）没有级别，旅程条保持隐藏，直到第 2 张幻灯片显示"Low"（幻灯片 2-9 位于第 10 张幻灯片的第一次级别转换之前，因此旅程条在第 10 张幻灯片之前显示为空/零）。

**注意：** 主演示文稿（`presentation/index.html`）最高到 **High** 级别 —— 不使用 `data-level="pro"`。Pro 标记在旅程条上作为理论天花板保持可见，但填充永远不会到达它。视频演示文稿（`1-video-workflow.html`）最高到 **Medium** 级别。
