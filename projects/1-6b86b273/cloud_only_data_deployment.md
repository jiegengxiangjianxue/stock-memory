---
name: "cloud_only_data_deployment"
description: "涉及数据读取、部署或前端验证时，先遵循项目的云端架构约定。 [待确认 2026-10-17，与 cloud_only_stock_data 冲突]"
type: workflow
pending: true
---
项目自 2026-10-14 起采用 cloud-only 部署：本地数据库、本地网站和本地 DuckDB 已退役，前端数据直接读取云仓库；不要把本地 `data/` 或本地数据库视为生产数据源。依据：`AGENTS.md`。