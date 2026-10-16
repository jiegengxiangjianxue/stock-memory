---
name: "cloud_only_deployment_pending_22052de3a9fc"
description: "调整 stocks-ai-view 的部署、存储或数据读取方式时阅读。 [待确认 2026-10-16，与 cloud_only_deployment 冲突]"
type: identity
pending: true
---
## 仅云端部署
stocks-ai-view 自 2026-10-14 起废弃本地数据库与本地网站。网站从云端部署，直接读取云端数据仓库。
- 不使用本地 `data/` 目录。
- 不使用本地 DuckDB。
- 修改部署或读取逻辑时，保持云端架构，不恢复本地数据库或本地网站。