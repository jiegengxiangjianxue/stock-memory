---
name: cloud_only_deployment
description: stocks-ai-view 自 2026-10-14 起纯云端部署，本地无 data/ 与 DuckDB，读取走 GitHub。
type: workflow
---
stocks-ai-view 项目已于 2026-10-14 退役本地数据库与本地网站，站点部署在云端并直接从云端仓库读取数据。所有读取操作均通过 GitHub 完成，本地不存在 `data/` 目录，也不使用本地 DuckDB。任何本地脚本或调试都应面向云端仓库，不要尝试在本地建库或落盘。