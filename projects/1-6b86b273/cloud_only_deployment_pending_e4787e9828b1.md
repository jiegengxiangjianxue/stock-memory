---
name: "cloud_only_deployment_pending_e4787e9828b1"
description: "涉及 stocks-ai-view 的数据读取或部署时，遵循云端仓库架构。 [待确认 2026-10-17，与 cloud_only_deployment 冲突]"
type: workflow
pending: true
---
项目约定：本地数据库和本地网站已退役，部署使用云端环境，网站数据直接从云端数据仓库读取；不要把本地 `data/` 或 DuckDB 作为正式数据源。