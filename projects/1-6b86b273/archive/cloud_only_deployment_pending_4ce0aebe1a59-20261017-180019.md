---
name: "cloud_only_deployment_pending_4ce0aebe1a59"
description: "进行数据读取或部署时应遵循项目的云端架构，避免依赖已废弃的本地数据库和网站。 [待确认 2026-10-17，与 cloud_only_deployment 冲突]"
type: workflow
pending: true
---
项目采用仅云端部署：本地数据库和本地网站均已废弃，项目不应依赖本地 `data/` 目录或本地 DuckDB；网站从云端部署，并直接读取云端数据仓库。