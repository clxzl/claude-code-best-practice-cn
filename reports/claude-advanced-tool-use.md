# Claude 高级工具使用模式

API 级别功能（现已正式发布）可减少 Token 消耗、降低延迟并提高工具准确性。随 Opus/Sonnet 4.6 一同发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [概述](#概述)
2. [程序化工具调用 (PTC)](#程序化工具调用-ptc)
3. [网页搜索/抓取的动态过滤](#网页搜索抓取的动态过滤)
4. [工具搜索工具](#工具搜索工具)
5. [工具使用示例](#工具使用示例)
6. [Claude Code 相关性](#claude-code-相关性)

---

## 概述

| 功能 | 解决的问题 | Token 节省 | 可用性 |
|------|-----------|-----------|--------|
| 程序化工具调用 | 多步 Agent 循环在往返中消耗 Token | 减少约 37% | API、Foundry（正式发布） |
| 动态过滤 | 网页搜索/抓取结果因无关内容膨胀上下文 | 减少约 24% 输入 Token | API、Foundry（正式发布） |
| 工具搜索工具 | 过多工具定义膨胀上下文 | 减少约 85% | API、Foundry（正式发布） |
| 工具使用示例 | 仅靠 Schema 无法表达使用模式 | 准确率从 72% 提升至 90% | API、Foundry（正式发布） |

所有功能已于 2026 年 2 月 18 日**正式发布**。

**策略分层** — 从你最大的瓶颈入手：
- 工具定义导致上下文膨胀 → 工具搜索工具
- 大量中间结果 → 程序化工具调用
- 网页搜索噪声 → 动态过滤
- 参数错误 → 工具使用示例

---

## 程序化工具调用 (PTC)

<img src="assets/programmatic-tool-calling-diagram.svg" alt="PTC 图示 — 传统工具调用 vs 程序化工具调用" width="100%" />

### 范式转变

**之前（传统工具调用）：**
```
User prompt → Claude → Tool call 1 → Response 1 → Claude → Tool call 2 → Response 2 → Claude → Tool call 3 → Response 3 → Claude → Final answer
```
每次工具调用都需要完整的模型往返。3 个工具 = 3 次推理。

**之后（程序化工具调用）：**
```
User prompt → Claude → writes Python script → Script calls Tool 1, Tool 2, Tool 3 internally → stdout → Claude → Final answer
```
Claude 编写代码来编排所有工具。只有最终的 `stdout` 进入上下文窗口。3 个工具 = 1 次推理。

### 工作原理

1. 你使用 `allowed_callers: ["code_execution_20250825"]` 定义工具
2. Claude 编写 Python 代码，在沙箱中将这些工具作为异步函数调用
3. 当工具函数被调用时，沙箱暂停，API 返回一个 `tool_use` 块
4. 你提供工具结果 — 它会发送给**正在运行的代码**，而非 Claude 的上下文
5. 代码恢复执行，处理结果，如果需要则调用更多工具
6. 只有最终执行的 `stdout` 才会到达 Claude

### 关键配置

```json
{
  "tools": [
    {
      "type": "code_execution_20250825",
      "name": "code_execution"
    },
    {
      "name": "query_database",
      "description": "执行SQL查询。返回的行数据为JSON对象，包含以下字段：id (字符串)、name (字符串)、revenue (浮点数)。",
      "input_schema": {
        "type": "object",
        "properties": {
          "sql": { "type": "string", "description": "要执行的SQL查询" }
        },
        "required": ["sql"]
      },
      "allowed_callers": ["code_execution_20250825"]
    }
  ]
}
```

### `allowed_callers` 字段

| 值 | 行为 |
|----|------|
| `["direct"]` | 仅传统工具调用（省略时的默认值） |
| `["code_execution_20250825"]` | 仅可从 Python 沙箱调用 |
| `["direct", "code_execution_20250825"]` | 两种模式均可用 |

**建议：** 每个工具选择一种模式，而非两种。这样可以给 Claude 更清晰的指导。

### 响应中的 `caller` 字段

每个工具使用块都包含一个 `caller` 字段，方便你了解其调用方式：

```json
// 直接调用（传统方式）
{ "caller": { "type": "direct" } }

// 程序化调用（来自代码执行）
{ "caller": { "type": "code_execution_20250825", "tool_id": "srvtoolu_abc123" } }
```

### 高级模式

**批量处理** — 在 1 次推理中处理 N 个项目：
```python
regions = ["West", "East", "Central", "North", "South"]
results = {}
for region in regions:
    data = await query_database(f"SELECT SUM(revenue) FROM sales WHERE region='{region}'")
    results[region] = data[0]["revenue"]

top = max(results.items(), key=lambda x: x[1])
print(f"Top region: {top[0]} with ${top[1]:,}")
```

**提前终止** — 满足成功条件后立即停止：
```python
endpoints = ["us-east", "eu-west", "apac"]
for endpoint in endpoints:
    status = await check_health(endpoint)
    if status == "healthy":
        print(f"Found healthy endpoint: {endpoint}")
        break
```

**条件工具选择：**
```python
file_info = await get_file_info(path)
if file_info["size"] < 10000:
    content = await read_full_file(path)
else:
    content = await read_file_summary(path)
print(content)
```

**数据过滤** — 减少 Claude 需要处理的内容：
```python
logs = await fetch_logs(server_id)
errors = [log for log in logs if "ERROR" in log]
print(f"Found {len(errors)} errors")
for error in errors[-10:]:
    print(error)
```

### 模型兼容性

| 模型 | 支持 |
|------|------|
| Claude Opus 4.6 | 是 |
| Claude Sonnet 4.6 | 是 |
| Claude Sonnet 4.5 | 是 |
| Claude Opus 4.5 | 是 |

### 约束条件

| 约束 | 详情 |
|------|------|
| **不在 Bedrock/Vertex 上** | 仅限 API 和 Foundry |
| **不支持 MCP 工具** | MCP 连接器工具无法以程序化方式调用 |
| **不支持网页搜索/抓取** | PTC 中不支持网页工具 |
| **不支持结构化输出** | `strict: true` 工具不兼容 |
| **不支持强制工具选择** | `tool_choice` 无法强制 PTC |
| **容器生命周期** | 约 4.5 分钟后过期 |
| **ZDR** | 不受零数据保留覆盖 |
| **工具结果为字符串** | 验证外部结果以防范代码注入风险 |

### 何时使用 PTC

| 适用场景 | 不太适合 |
|----------|---------|
| 处理需要聚合的大型数据集 | 简单响应的单次工具调用 |
| 3 个以上按顺序依赖的工具调用 | 需要即时用户反馈的工具 |
| 在 Claude 查看之前过滤/转换结果 | 非常快速的操作（开销大于收益） |
| 跨多个项目的并行操作 | |
| 基于中间结果的条件逻辑 | |

### Token 效率

- 程序化调用的工具结果**不会添加到 Claude 的上下文** — 只有最终的 `stdout` 会
- 中间处理在代码中进行，不消耗模型 Token
- 10 个程序化工具 ≈ 10 个直接调用所需 Token 的 1/10

---

## 网页搜索/抓取的动态过滤

### 问题

网页搜索和抓取工具会将完整的 HTML 页面倒入 Claude 的上下文窗口。其中大部分内容都是无关的 — 导航栏、广告、样板文本。Claude 需要对所有内容进行推理，浪费 Token 并降低准确性。

### 解决方案

Claude 现在会**编写并执行 Python 代码来过滤网页结果**，然后再将其放入上下文窗口。Claude 不再对原始 HTML 进行推理，而是在沙箱中进行过滤、解析，仅提取相关内容。

### 工作原理

**之前：**
```
查询 → 搜索结果 → 抓取完整 HTML × N 个页面 → 所有内容进入上下文 → Claude 对所有内容推理
```

**之后：**
```
查询 → 搜索结果 → Claude 编写过滤代码 → 代码仅提取相关内容 → 过滤后的结果进入上下文
```

### API 配置

使用更新的工具类型版本和 Beta 头信息：

```json
{
  "model": "claude-opus-4-6",
  "max_tokens": 4096,
  "tools": [
    {
      "type": "web_search_20260209",
      "name": "web_search"
    },
    {
      "type": "web_fetch_20260209",
      "name": "web_fetch"
    }
  ]
}
```

**需要的请求头：** `anthropic-beta: code-execution-web-tools-2026-02-09`

**默认启用** — 使用 Sonnet 4.6 和 Opus 4.6 的新工具类型版本时自动生效。

### 基准测试结果

**BrowseComp**（在网站上查找特定信息）：

| 模型 | 无过滤 | 有过滤 | 提升 |
|------|--------|--------|------|
| Sonnet 4.6 | 33.3% | **46.6%** | +13.3 个百分点 |
| Opus 4.6 | 45.3% | **61.6%** | +16.3 个百分点 |

**DeepsearchQA**（多步研究，F1 分数）：

| 模型 | 无过滤 | 有过滤 | 提升 |
|------|--------|--------|------|
| Sonnet 4.6 | 52.6% | **59.4%** | +6.8 个百分点 |
| Opus 4.6 | 69.8% | **77.3%** | +7.5 个百分点 |

**Token 效率：** 平均减少 24% 的输入 Token。Sonnet 4.6 成本降低；Opus 4.6 可能略有增加，因为过滤代码更复杂。

### 使用场景

- 筛选技术文档
- 跨多个来源验证引用
- 交叉引用搜索结果
- 多步研究查询
- 在大型页面中查找特定数据点

---

## 工具搜索工具

### 问题

预先加载所有工具定义会浪费上下文。如果你有 50 个 MCP 工具，每个约 1.5K Token，那在用户提问之前就已经占用了 75K Token。

### 解决方案

将不常用的工具标记为 `defer_loading: true`。它们会被排除在初始上下文之外。Claude 通过工具搜索工具按需发现它们。

### 配置

```json
{
  "工具": [
    {
      "类型": "mcp_toolset",
      "mcp服务器名称": "google-drive",
      "默认配置": { "延迟加载": true },
      "配置项": {
        "搜索文件": { "延迟加载": false }
      }
    }
  ]
}
```

### 最佳实践

- 保持 3-5 个最常用的工具始终加载，其余的延迟加载
- 编写清晰、描述性的工具名称和描述（搜索依赖它们）
- 在系统提示中记录可用功能

### 何时使用

- 工具定义消耗超过 10K Token
- 有 10 个以上可用工具
- 使用多个 MCP 服务器
- 因选项过多导致工具选择准确性问题

### Token 节省

工具定义 Token 减少约 85%（在 Anthropic 的基准测试中从 77K 降至 8.7K）。

### Claude Code 等效功能

Claude Code 内置 **MCP 工具搜索自动模式**（自 v2.1.7 起默认启用）。当 MCP 工具描述超过上下文的 10% 时，它们会被延迟并通过 `MCPSearch` 发现。使用 `ENABLE_TOOL_SEARCH=auto:N` 配置阈值，其中 N 是上下文百分比（0-100）。

---

## 工具使用示例

### 问题

JSON Schema 定义了结构，但无法表达：
- 何时包含可选参数
- 哪些参数组合有意义
- 格式约定（日期格式、ID 模式）
- 嵌套结构的使用

### 解决方案

在工具定义中添加 `input_examples` — 超越 Schema 的具体使用模式。

### 配置

```json
{
  "name": "create_ticket",
  "description": "创建支持工单",
  "input_schema": {
    "type": "object",
    "properties": {
      "title": { "type": "string" },
      "priority": { "type": "string", "enum": ["低", "中", "高", "紧急"] },
      "assignee": { "type": "string" },
      "labels": { "type": "array", "items": { "type": "string" } }
    },
    "required": ["title"]
  },
  "input_examples": [
    {
      "title": "登录页返回500错误",
      "priority": "紧急",
      "assignee": "oncall-team",
      "labels": ["缺陷", "认证", "生产环境"]
    },
    {
      "title": "添加深色模式支持",
      "priority": "低",
      "labels": ["功能请求", "用户界面"]
    },
    {
      "title": "更新v2端点的API文档"
    }
  ]
}
```

### 最佳实践

- 使用**真实数据**，而非类似 "example_value" 的占位符字符串
- 展示**多样性**：最少、部分和完整规格
- 保持简洁：**每个工具 1-5 个示例**
- 重点解决歧义 — 侧重行为清晰度而非 Schema 完整性
- 展示参数关联性（例如 `priority: "critical"` 通常带有 `assignee`）

### 结果

在 Anthropic 的基准测试中，复杂参数处理的准确率从 72% 提升至 90%。

---

## Claude Code 相关性

### 直接适用于 Claude Code 用户的功能

| 功能 | Claude Code 状态 | 操作 |
|------|-----------------|------|
| 工具搜索 | 自 v2.1.7 起内置为 MCPSearch 自动模式 | 如果你有很多 MCP 工具，请调整 `ENABLE_TOOL_SEARCH=auto:N` |
| 动态过滤 | CLI 中不可用（API 级别网页工具） | 对进行网页研究的 Agent SDK 用户相关 |
| PTC | CLI 中不可用 | 对构建自定义 Agent 的 Agent SDK 用户相关 |
| 工具使用示例 | CLI 中不可配置 | 对自定义 MCP 服务器作者相关 |

### Agent SDK 开发者

如果你正在使用 `@anthropic-ai/claude-agent-sdk` 构建 Agent，PTC 可以立即应用：

1. 将 `code_execution_20250825` 添加到你的工具数组
2. 为受益于批处理/过滤的工具设置 `allowed_callers`
3. 实现工具结果循环（暂停 → 提供结果 → 恢复）
4. 从工具返回结构化数据（JSON）以便程序化解析

### MCP 服务器作者

如果你正在构建自定义 MCP 服务器，工具使用示例可以改善 Claude 使用你工具的方式：
- 在工具 Schema 中添加 `input_examples`
- 在描述中清晰记录返回格式（PTC 需要解析它们）

---

## 参考来源

- [Anthropic 工程博客：高级工具使用](https://www.anthropic.com/engineering/advanced-tool-use)
- [程序化工具调用文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)
- [代码执行工具文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)
- [通过动态过滤改进网页搜索](https://claude.com/blog/improved-web-search-with-dynamic-filtering)
