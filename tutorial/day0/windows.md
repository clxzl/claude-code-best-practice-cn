# Windows 安装

[返回 Day 0](README.md)

---

## 安装 Node.js

- 打开 [nodejs.org](https://nodejs.org)
- 点击 **“Download Node.js (LTS)”** 按钮 —— 会下载 `.msi` 安装包
- 运行这个 `.msi` 文件，然后一路点击 **Next**
- 接受默认选项，点击 **Install**，等待安装完成

## 验证 Node.js

- 打开一个**新的**终端（PowerShell 或 Windows Terminal），运行：
  ```powershell
  node --version
  npm --version
  ```

## 安装 Claude Code

```powershell
npm install -g @anthropic-ai/claude-code
```

- 如果遇到权限错误，请以**管理员身份**运行终端（右键 → 以管理员身份运行）

## 验证安装

```powershell
claude --version
```

如果能看到版本号，说明安装成功。

---

接下来返回 [README.md](README.md) 完成认证设置。
