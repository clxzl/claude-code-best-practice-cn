---
description: 获取迪拜天气数据并创建 SVG 天气卡片
model: haiku
---

# Weather Orchestrator 命令

获取阿联酋迪拜的当前温度并创建可视化 SVG 天气卡片。

## 工作流

### 步骤 1：询问用户偏好

使用 AskUserQuestion 工具询问用户想要摄氏度还是华氏度温度。

### 步骤 2：获取天气数据

使用 Task 工具调用 weather agent：
- subagent_type: weather-agent
- description: 获取迪拜天气数据
- prompt: 获取阿联酋迪拜的当前温度，单位为[用户请求的单位]。返回数值温度值和单位。该 Agent 有一个预加载的 skill（weather-fetcher）提供详细指令。
- model: haiku

等待 Agent 完成并捕获返回的温度值和单位。

### 步骤 3：创建 SVG 天气卡片

使用 Skill 工具调用 weather-svg-creator skill：
- skill: weather-svg-creator

该 skill 将使用步骤 2 中的温度值和单位（在当前上下文中可用）来创建 SVG 卡片并写入输出文件。

## 关键要求

1. **使用 Task 工具调用 Agent**：不要使用 bash 命令调用 Agent。必须使用 Task 工具。
2. **使用 Skill 工具调用 SVG Creator**：通过 Skill 工具调用 SVG creator，而不是 Task 工具。
3. **传递用户偏好**：调用 Agent 时包含用户的温度单位偏好。
4. **顺序执行**：完成每一步后再进行下一步。

## 输出摘要

向用户提供清晰的摘要，显示：
- 请求的温度单位
- 从迪拜获取的温度
- 在 `orchestration-workflow/weather.svg` 创建的 SVG 卡片
- 写入 `orchestration-workflow/output.md` 的摘要
