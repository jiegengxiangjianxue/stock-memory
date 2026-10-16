---
name: "remove_unused_home_strings"
description: "Android lint gate 因未使用的首页字符串资源失败时，读取此经验处理。"
type: fact
---
`android/app/src/main/res/values/string.xml` 和 `values-en/strings.xml` 中未被代码引用的 `home_feed_popular`、`home_feed_precious`、`home_feed_weekly`、`home_feed_current_hint`、`home_hero_for_you` 会触发 `UnusedResources` lint 警告并导致 gate 失败；删除这些资源后重新构建，lint gate 通过且相关回归测试全部通过。