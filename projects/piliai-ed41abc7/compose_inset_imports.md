---
name: compose_inset_imports
description: 使用 Compose 的 WindowInsets 或 navigationBarsPadding 时，缺少对应布局导入会直接导致编译失败。
type: fact
---
AppNavigation.kt 曾因 `WindowInsets` 和 `navigationBarsPadding` 未导入而出现 `Unresolved reference`，补充 `androidx.compose.foundation.layout.WindowInsets` 与 `androidx.compose.foundation.layout.navigationBarsPadding` 导入后构建成功。