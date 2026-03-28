# macOS 安装

[返回 Day 0](README.md)

---

## Terminal

- 打开 Terminal（按 `Cmd + Space`，输入 “Terminal”，然后回车）

## Homebrew

- 先检查 Homebrew 是否已经安装：
  ```bash
  brew --version
  ```
- 如果提示 `command not found`，先安装 Homebrew：
  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```

## 安装 Claude Code

```bash
brew install --cask claude-code
```

## 验证安装

```bash
claude --version
```

如果能看到版本号，说明安装成功。

---

接下来返回 [README.md](README.md) 完成认证设置。
