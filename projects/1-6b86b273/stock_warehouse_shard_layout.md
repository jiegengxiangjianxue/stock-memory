---
name: "stock_warehouse_shard_layout"
description: "需要定位按证券代码存储的数据或规划迁移时查阅。 [待确认 2026-10-17，与 cloud_warehouse_authority 冲突]"
type: workflow
pending: true
---
`stocks-db/README.md` 将云仓库分为市场数据仓库 `db-market` 与按证券分片的 `db-stocks-a` 至 `db-stocks-e`；股票路径格式见 `_index.json`：`data/stocks/<shard>/<code>/manifest.json`。已列出的分片归属包括 `00`→a、`30`→b、`60`→c，`68/15/51/50/52/56/58`→d，其余→e。定位数据时应先按代码分片路由，不要假设全部股票集中在单一仓库。