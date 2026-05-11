# 压缩合并 & PR 大小分布 — 技巧 from Boris Cherny

Boris Cherny ([@bcherny](https://x.com/bcherny))，Claude Code 的创建者，于 2026 年 3 月 25 日分享的见解摘要。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 单日 266 次贡献——始终压缩合并

Boris 分享了他的 GitHub 贡献图表，显示 **3 月 24 日有 266 次贡献**——来自 **141 个 PR**，始终使用**压缩合并**，每个 PR 中位数为 **118 行**。

- 压缩合并将分支上的所有提交合并为目标分支上的单个提交——保持历史记录干净和线性
- 每个 PR = 一个提交，便于回滚整个功能，简化 `git bisect`
- 在高速 AI 辅助工作流中（每天 141 个 PR），压缩合并是务实的选择——分支内的"修复 lint"、"试试这个"等提交都是噪音

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/1.png" alt="Boris Cherny — 266次提交，总是压缩合并" width="50%" /></a>

---

## 2/ PR 大小分布——保持 PR 小型化

Boris 分享了这 141 个 PR 的大小分布，总计 **45,032 行变更**（新增 + 删除）：

| 指标 | 行数（新增+删除） | 含义 |
|--------|---------------:|---------|
| **p50** | **118** | PR 大小中位数——一半的 PR 为 118 行或更少 |
| p90 | 498 | 90% 的 PR 少于 500 行 |
| **p99** | **2,978** | 仅约 1 个 PR 超过约 3K 行 |
| min | 2 | 最小的 PR——一个快速的 2 行修复 |
| max | 10,459 | 最大的单个 PR——可能是迁移或生成的代码 |

- **118 行的中位数**意味着大多数 PR 都是聚焦且可审查的，即使每天 141 个 PR
- 分布严重右偏——偶尔的大 PR 是不可避免的（批量重命名、迁移），但常规是紧凑的
- 小型 PR 降低合并冲突风险，更容易审查，与压缩合并完美配合，便于干净地回滚

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/2.png" alt="Boris Cherny — PR规模分布表" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 3 月 25 日](https://x.com/bcherny)
