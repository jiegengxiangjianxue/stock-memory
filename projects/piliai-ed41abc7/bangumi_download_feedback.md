---
name: bangumi_download_feedback
description: 番剧下载按钮无响应的修复：需 toast 引导用户至分P面板
type: fact
---
在 pili AI Android 项目的 `VideoPlayerViewModel.downloadCurrent` 中，番剧视频（`currentBvid` 以 `"ep"` 开头）原来与空 bv 合并判断后直接 `return`，无任何提示，导致用户点击下载无反应，误以为按钮损坏。修复方案：先判断 `bv.isEmpty()` 直接 return；再对 `bv.startsWith("ep")` 的情况调用 `showToast("番剧请从分P面板选择要缓存的集数")` 后再 return。