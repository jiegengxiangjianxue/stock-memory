---
name: toutiao_hot_v2_endpoint
description: 实现或修复头条热榜抓取前读取，避免使用旧签名路径。
type: fact
---
头条热榜抓取不要用带 `_signature` 的旧路径，会返回 412。可用端点：`https://www.toutiao.com/hot-event/hot-board/?origin=toutiao_pc`，实测零 header、零签名即可拿到约 50 条。