---
name: time-fetcher
description: 通过 bash 命令获取当前迪拜时间的指令
user-invocable: false
---

## 迪拜时间获取器

### 命令

```bash
TZ='Asia/Dubai' date '+%Y-%m-%d %H:%M:%S %Z'
```

### 预期输出格式

`YYYY-MM-DD HH:MM:SS +04`（海湾标准时间）

### 时区详情

- 时区：Asia/Dubai
- 偏移量：UTC+4
- 缩写：GST（海湾标准时间）
- 迪拜不实行夏令时

### 返回格式

提供以下字段：
- `time`：仅时间部分 (HH:MM:SS)
- `timezone`："GST (UTC+4)"
- `formatted`：命令的完整输出字符串
