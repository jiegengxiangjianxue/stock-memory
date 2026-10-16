---
name: market_data_sync_pipeline
description: 排查行情更新、引擎联动或网站数据延迟时先按既有链路和时延约束处理。
type: workflow
---
引擎代码唯一事实源是 `scripts/`；实时链路由 GitHub Actions 执行 `realtime-minute`（每 3 分钟）、`realtime-poll`（每 5 分钟）和 `full-pipeline`（每 5 分钟，采集→合并去重→26 个引擎→产出→push）。数据写入 GitHub 后，GitHub CDN 生效已实测需 1–5 分钟，因此不做 1 分钟级轮询。