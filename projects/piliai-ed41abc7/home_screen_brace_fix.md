---
name: "home_screen_brace_fix"
description: "修改 HomeScreen.kt 后出现多个未解析引用和 local function 错误时，先检查函数外层大括号是否缺失。"
type: fact
---
`HomeScreen.kt` 中缺少一个结束大括号会导致后续函数被解析为局部函数，从而触发 `private is not applicable to 'local function'` 以及 `resolveGridColumnCount`、`HeroCarousel` 等未解析引用。补上缺失的大括号后，项目构建成功。