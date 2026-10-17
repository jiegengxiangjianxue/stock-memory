---
name: "cloud_warehouse_authority"
description: "需要判断股票数据的权威来源或部署读取方式时查阅。 [待确认 2026-10-17，与 cloud_only_stock_data 冲突]"
type: workflow
keywords: ["cloud", "warehouse", "authority", "需要判断股票数据的权威来源或部署读取方式时查阅", "待确认", "2026-10-17", "cloud_only_stock_data", "冲突"]
pending: true
---
`stocks-ai-view/AGENTS.md` 和 `stocks-db/README.md` 明确约定云端仓库为数据来源：网站从云端仓库读取，优先 GitHub raw，失败时回退 GitCode 镜像；不要把本地 DuckDB 或本地网站当作现行权威部署。工作区中仍可见 `stocks-db/data/`，因此‘本地目录已移除’这一文档表述不能当作当前文件状态的证明。