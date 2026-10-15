---
name: wallstcn_live_api
description: 零鉴权多频道快讯 API 的可用范围、字段特性与翻页方式
type: fact
---
`api-one.wallstcn.com/apiv1/content/lives` 零鉴权可用，仅 `a-stock-channel`/`us-stock-channel`/`hk-stock-channel` 三个频道有效（`global`/`tech` 超时或空）。返回字段含 `symbols`、`related_themes`、`fund_codes`、`score`；但 `symbols` 在快讯流中实测为空，个股关联需依赖 `related_themes`。支持 `next_cursor` 翻页。