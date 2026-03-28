# Linux 安装

[返回 Day 0](README.md)

## 前提条件

你需要 **Node.js v18 或更高版本**，以及 **npm**。

## 步骤 1：安装 Node.js

### 方案 A：通过 nodejs.org 下载页配合 fnm 安装（推荐）

**fnm**（Fast Node Manager）是 Node.js 官方推荐的方案之一。它速度快、很轻量，而且以后需要切换 Node 版本时也很方便。

1. 打开浏览器，访问 [nodejs.org/en/download](https://nodejs.org/en/download)。

2. 页面上会有一排下拉框，大致显示为：**“Get Node.js® vXX.XX.X (LTS) for __ using __ with __”**。请按下面选择：

   | 下拉项 | 选择 |
   |----------|--------|
   | Version | **vXX.XX.X (LTS)** —— 保持默认 LTS 版本，不要改 |
   | OS | **Linux** |
   | Package Manager | **fnm**（在 “Recommended (Official)” 下） |
   | Package Format | **npm** —— 保持默认 |

3. 页面会给出具体命令。打开终端直接复制粘贴即可，大概长这样：

   ```bash
   # 步骤 1 —— 安装 fnm
   curl -fsSL https://fnm.vercel.app/install | bash

   # 步骤 2 —— 重启终端或重新加载 shell 配置
   source ~/.bashrc   # 或：source ~/.zshrc（如果你用 zsh）

   # 步骤 3 —— 安装 Node.js
   fnm install 24   # 页面会显示具体版本号
   ```

   > 上面的版本号可能和你看到的不一样——永远以官网显示为准。

4. **关闭并重新打开终端**（或运行上面的 `source` 命令），确保 `fnm`、`node` 和 `npm` 都已生效。

> **为什么推荐 fnm？** 因为它在 Node.js 下载页里属于“Recommended (Official)”分类。和 nvm 一样，它把 Node 安装在你的 home 目录里，所以以后全局安装 npm 包通常不需要 `sudo`。但 fnm 更快（Rust 写的），而且在 Windows、macOS、Linux 上体验比较一致。

### 方案 B：使用发行版自带包管理器

这种方式更快，但有可能安装到过旧版本的 Node.js。**安装后务必检查版本** —— 如果低于 v18，就改用方案 A。

**Ubuntu / Debian：**

```bash
sudo apt update
sudo apt install -y nodejs npm

# 检查版本
node --version   # 必须是 v18 或更高
```

**Fedora：**

```bash
sudo dnf install -y nodejs npm
```

**Arch Linux：**

```bash
sudo pacman -S nodejs npm
```

### 方案 C：NodeSource（通过 apt 安装最新 LTS，不用 nvm）

适合 Ubuntu/Debian 用户，如果你想装最新 LTS，但又不想用 nvm/fnm：

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

## 步骤 2：验证 Node.js

```bash
node --version
npm --version
```

两个命令都应该输出版本号，其中 `node --version` 必须是 v18.x 或更高。

## 步骤 3：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

> **遇到权限错误？**
> - 如果你使用的是 **fnm** 或 **nvm**：正常情况下不该出现。检查它是否真的生效（`which node` 应该指向你 home 目录里的路径，而不是 `/usr/...`）。
> - 如果你使用的是系统安装版 Node：你可以用 `sudo npm install -g @anthropic-ai/claude-code`，或者修正 npm 的全局目录权限：
>   ```bash
>   mkdir -p ~/.npm-global
>   npm config set prefix '~/.npm-global'
>   echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
>   source ~/.bashrc
>   ```

## 步骤 4：验证 Claude Code

```bash
claude --version
```

如果看到 Claude Code 的版本号，就说明安装成功。

接下来回到 [README.md](README.md) 完成认证设置。

---

## 注意事项

- **WSL（Windows Subsystem for Linux）**：这份指南同样适用于 WSL，直接在 WSL 终端里照做即可。
- **PATH 问题**：如果安装后找不到 `claude`，请确认 npm 的全局 bin 目录已经加入 PATH。运行 `npm config get prefix`，然后把该路径下的 `bin/` 子目录加入 PATH。
