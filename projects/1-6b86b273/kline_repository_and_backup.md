---
name: "kline_repository_and_backup"
description: "选择 K 线历史数据存放位置或实时数据源时，参考已确定的仓库分工。"
type: workflow
---
K 线全历史保留在独立 GitHub 仓库 `stocks-data-kline`；重数据不放进容器持久化。GitCode 仅作为冷备，不作为算法、引擎或网站的实时数据源。