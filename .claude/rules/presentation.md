# Glob: presentation/**

## 委派规则

任何更新、修改或修复演示文稿（`presentation/index.html`）的请求都必须由 `presentation-curator` agent 处理。始终通过 Task 工具将演示文稿工作委派给此 agent — 永远不要直接编辑演示文稿。

```
Task(subagent_type="presentation-curator", description="...", prompt="...")
```

## 原因

presentation-curator agent 预加载了三个 Skills，使其与演示文稿的结构、样式和概念框架保持同步。它还在每次执行后自我演进，更新自身的 Skills 以防止知识漂移。绕过该 agent 可能会破坏幻灯片编号、级别过渡或样式一致性。
