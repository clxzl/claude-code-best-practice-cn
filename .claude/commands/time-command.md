---
description: 显示巴基斯坦标准时间（PKT，UTC+5）的当前时间
---

# 时间 Command

显示巴基斯坦标准时间（PKT，UTC+5）的当前日期和时间。

## 指令

1. 运行以下 bash 命令获取 PKT 当前时间：
   ```
   TZ='Asia/Karachi' date '+%Y-%m-%d %H:%M:%S %Z'
   ```

2. 以以下格式向用户显示结果：
   ```
   Current Time in Pakistan (PKT): YYYY-MM-DD HH:MM:SS PKT
   ```

## 要求

- 始终使用 `Asia/Karachi` 时区（UTC+5）
- 使用 24 小时制
- 日期和时间一起显示
- 保持输出简洁
