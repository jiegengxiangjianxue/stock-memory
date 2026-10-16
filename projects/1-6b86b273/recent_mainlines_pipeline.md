---
name: recent_mainlines_pipeline
description: 修改主线数据后，需在采集工作流生成近30日主线快照并纳入发布链。
type: workflow
---
在 `.github/workflows/shard-collect.yml` 中使用 `python3 scripts/recent_mainlines.py --days 30 --out data/recent_mainlines.json` 生成近 30 日主线数据；在 `.github/workflows/engine-chain.yml` 的发布文件列表和 `--protect` 列表中同时加入 `data/recent_mainlines.json`，避免生成后未发布或被清理。