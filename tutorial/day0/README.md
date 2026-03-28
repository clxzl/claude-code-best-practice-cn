# Day 0 —— Claude Code 安装与登录

本指南会带你完成 Claude Code 的安装与认证，让你可以在自己的机器上直接开始使用它。

## 步骤 1：安装 Claude Code

请选择你的操作系统：

| 操作系统 | 指南 |
|----|-------|
| Windows | [windows.md](windows.md) |
| Linux | [linux.md](linux.md) |
| macOS | [mac.md](mac.md) |

先按照你的系统对应指南完成安装，然后再回到这里进行认证。

---

## 步骤 2：验证安装

完成系统对应的安装步骤后，确认一切都正常：

```bash
node --version    # 应显示 v18.x 或更高
claude --version  # 应显示已安装的 Claude Code 版本
```

---

## 步骤 3：登录

<img src="assets/login.png" alt="Claude Code login screen" width="50%">

在终端中运行 `claude`。首次启动时，它会要求你选择登录方式。

### 方法 1：订阅用户（Claude Pro / Max）

- 选择 **Claude.ai account**
- 浏览器会自动打开 —— 登录并授权
- 返回终端后，你就已经登录完成

### 方法 2a：API Key（团队邀请）

你的团队管理员会从 Anthropic 控制台邀请你。

- 你会收到一封**邀请邮件** —— 接受邀请并创建 Anthropic 账户
- 在终端运行 `claude`
- 选择 **Anthropic API Key**
- 你的 key 会在控制台**自动生成** —— 不需要手动创建
- Claude Code 会立即开始工作

### 方法 2b：API Key（你已经有 key）

如果有人已经把 key 发给你（比如通过 Slack、邮件等），或者这是你自己创建的：

- 在终端运行 `claude`
- 选择 **Anthropic API Key**
- 粘贴你的 key（以 `sk-ant-` 开头）
- 这个 key 会被**永久保存** —— 之后不会再反复要求输入

---

认证完成后，你就可以正式开始使用 Claude Code 了。\n看不懂的地方别硬扛，回来继续问。🍮
