---
name: "cloud_retire_local_db_site"
description: "改数据层或网站前先读：本地库和本地站已退役，不要再依赖本地 data/ 或 DuckDB [待确认 2026-10-17，与 cloud_only_deployment_pending_4ce0aebe1a59 冲突]"
type: identity
pending: true
---
AGENTS.md（2026-10-14）规定：本地数据库和本地网站都已退役。站点从云端部署，直接读云端仓库。禁止本地 `data/` 目录，禁止本地 DuckDB。