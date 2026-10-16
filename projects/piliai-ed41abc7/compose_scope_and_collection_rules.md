---
name: "compose_scope_and_collection_rules"
description: "编写播放器 Compose UI 时应遵守已确认的状态收集和 Modifier 作用域规则。"
type: workflow
---
在 `BoxWithConstraints` 中，scope 对象不属于 Modifier 链，使用 `graphicsLayer` 等 API 时应显式使用 density；`collectAsState` 应在条件分支外调用，避免因 Compose 重组或调用位置变化导致状态收集问题。