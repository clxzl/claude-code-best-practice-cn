---
name: agent-browser
description: AI 代理的浏览器自动化 CLI 工具。当用户需要与网站交互时使用，包括导航页面、填写表单、点击按钮、截取屏幕截图、提取数据、测试 Web 应用或自动化任何浏览器任务。触发条件包括"打开网站"、"填写表单"、"点击按钮"、"截取屏幕截图"、"从页面抓取数据"、"测试此 Web 应用"、"登录网站"、"自动化浏览器操作"或任何需要编程式 Web 交互的任务。
allowed-tools: Bash(agent-browser:*)
---

# 使用 agent-browser 进行浏览器自动化

## 核心工作流

每个浏览器自动化都遵循以下模式：

1. **导航**：`agent-browser open <url>`
2. **快照**：`agent-browser snapshot -i`（获取元素引用如 `@e1`、`@e2`）
3. **交互**：使用引用进行点击、填写、选择
4. **重新快照**：在导航或 DOM 变更后，获取新的引用

```bash
agent-browser open https://example.com/form
agent-browser snapshot -i
# 输出：@e1 [input type="email"]，@e2 [input type="password"]，@e3 [button] "Submit"

```
agent-browser fill @e1 "user@example.com"
agent-browser fill @e2 "password123"
agent-browser click @e3
agent-browser wait --load networkidle
agent-browser snapshot -i  # 检查结果
```

## 基本命令

```bash
# 导航
agent-browser open <url>              # 导航（别名：goto，navigate）
agent-browser close                   # 关闭浏览器

# 快照
agent-browser snapshot -i             # 带引用的交互元素（推荐）
agent-browser snapshot -i -C          # 包含光标交互元素（带有 onclick、cursor:pointer 的 div）
agent-browser snapshot -s "#selector" # 限定到 CSS 选择器范围

# 交互（使用快照中的 @引用）
agent-browser click @e1               # 点击元素
agent-browser fill @e2 "text"         # 清除并输入文本
agent-browser type @e2 "text"         # 输入文本（不清除）
agent-browser select @e1 "option"     # 选择下拉选项
agent-browser check @e1               # 勾选复选框
agent-browser press Enter             # 按键
agent-browser scroll down 500         # 滚动页面

# 获取信息
agent-browser get text @e1            # 获取元素文本
agent-browser get url                 # 获取当前 URL
agent-browser get title               # 获取页面标题

# 等待
agent-browser wait @e1                # 等待元素出现
agent-browser wait --load networkidle # 等待网络空闲
agent-browser wait --url "**/page"    # 等待 URL 匹配模式
agent-browser wait 2000               # 等待毫秒数

# 捕获
agent-browser screenshot              # 截图到临时目录
agent-browser screenshot --full       # 全页面截图
agent-browser pdf output.pdf          # 保存为 PDF
```

## 常见模式

### 表单提交

```bash
agent-browser 打开 https://example.com/signup
agent-browser 截图 -i
agent-browser 填写 @e1 "Jane Doe"
agent-browser 填写 @e2 "jane@example.com"
agent-browser 选择 @e3 "California"
agent-browser 勾选 @e4
agent-browser 点击 @e5
agent-browser 等待网络空闲 --load networkidle
```

### 带状态持久化的身份验证

```bash
# 登录一次并保存状态
agent-browser 打开 https://app.example.com/login
agent-browser 快照 -i
agent-browser 填充 @e1 "$USERNAME"
agent-browser 填充 @e2 "$PASSWORD"
agent-browser 点击 @e3
agent-browser 等待 --url "**/dashboard"
agent-browser 保存状态 auth.json
```

# 在未来的会话中复用
agent-browser state load auth.json
agent-browser open https://app.example.com/dashboard
```

### 数据提取

```bash
agent-browser open https://example.com/products
agent-browser snapshot -i
agent-browser get text @e5           # 获取特定元素文本
agent-browser get text body > page.txt  # 获取全部页面文本

# JSON 输出以便解析
agent-browser snapshot -i --json
agent-browser get text @e1 --json
```

### 并行会话

```bash
agent-browser --session site1 open https://site-a.com
agent-browser --session site2 open https://site-b.com

agent-browser --session site1 snapshot -i
agent-browser --session site2 snapshot -i

agent-browser session list
```

### 可视化浏览器（调试）

```bash
agent-browser --headed open https://example.com
agent-browser highlight @e1          # 高亮元素
agent-browser record start demo.webm # 录制会话
```

### 本地文件（PDF、HTML）

```bash
# 使用 file:// URL 打开本地文件
agent-browser --allow-file-access open file:///path/to/document.pdf
agent-browser --allow-file-access open file:///path/to/page.html
agent-browser screenshot output.png
```

### iOS 模拟器（Mobile Safari）

```bash
# 列出可用的 iOS 模拟器
agent-browser device list

# 在特定设备上启动 Safari
agent-browser -p ios --device "iPhone 16 Pro" open https://example.com

# 与桌面端相同的工作流 —— 快照、交互、重新快照
agent-browser -p ios snapshot -i
agent-browser -p ios tap @e1          # 点击（click 的别名）
agent-browser -p ios fill @e2 "text"
agent-browser -p ios swipe up         # 移动端专用手势

# 截取屏幕截图
agent-browser -p ios screenshot mobile.png

# 关闭会话（关闭模拟器）
agent-browser -p ios close
```

**前置要求：** macOS 系统，安装 Xcode、Appium（`npm install -g appium && appium driver install xcuitest`）

**真实设备：** 支持已预配置的物理 iOS 设备。使用 `--device "<UDID>"`，其中 UDID 来自 `xcrun xctrace list devices`。

## 引用生命周期（重要）

引用（`@e1`、`@e2` 等）在页面发生变化时会失效。在以下操作后务必重新快照：

- 点击会触发导航的链接或按钮
- 表单提交
- 动态内容加载（下拉菜单、模态框）

```bash
agent-browser click @e5              # 导航到新页面
agent-browser snapshot -i            # 必须重新快照
agent-browser click @e1              # 使用新的引用
```

## 语义定位器（引用的替代方案）

当引用不可用或不可靠时，使用语义定位器：

```bash
agent-browser 查找文本 "登录" 点击
agent-browser 查找标签 "Email" 填写 "user@test.com"
agent-browser 查找角色 按钮 点击 --名称 "提交"
agent-browser 查找占位符 "搜索" 输入 "查询"
agent-browser 查找测试id "submit-btn" 点击
```

## 深入文档

| 参考文档 | 使用场景 |
|-----------|-------------|
| [references/commands.md](references/commands.md) | 包含所有选项的完整命令参考 |
| [references/snapshot-refs.md](references/snapshot-refs.md) | 引用生命周期、失效规则、故障排查 |
| [references/session-management.md](references/session-management.md) | 并行会话、状态持久化、并发抓取 |
| [references/authentication.md](references/authentication.md) | 登录流程、OAuth、2FA 处理、状态复用 |
| [references/video-recording.md](references/video-recording.md) | 用于调试和文档的录制工作流 |
| [references/proxy-support.md](references/proxy-support.md) | 代理配置、地理位置测试、轮换代理 |

## 即用模板

| 模板 | 描述 |
|----------|-------------|
| [templates/form-automation.sh](templates/form-automation.sh) | 带验证的表单填写 |
| [templates/authenticated-session.sh](templates/authenticated-session.sh) | 登录一次，复用状态 |
| [templates/capture-workflow.sh](templates/capture-workflow.sh) | 带屏幕截图的内容提取 |

```bash
./templates/form-automation.sh https://example.com/form
./templates/authenticated-session.sh https://app.example.com/login
./templates/capture-workflow.sh https://example.com ./output
```