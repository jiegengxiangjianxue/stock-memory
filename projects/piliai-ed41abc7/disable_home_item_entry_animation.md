---
name: disable_home_item_entry_animation
description: 首页大量网格卡片滚动掉帧时，应关闭逐项入场动画以减少协程和动画状态开销。
type: fact
---
首页 LazyVerticalGrid 的大量卡片逐项入场动画会让每个 item 同时启动协程/动画并造成滚动掉帧；已将首页卡片的 `enterAnimation` 设为 `false`，并在关闭时不创建 LaunchedEffect/动画状态。