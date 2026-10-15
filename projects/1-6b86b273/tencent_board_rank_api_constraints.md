---
name: tencent_board_rank_api_constraints
description: 接入或复核腾讯 board_rank 榜单接口时读取，确认 URL、返回规模与 sort_type 合法值。
type: fact
---
接口：`https://proxy.finance.qq.com/cgi/cgi-bin/rank/hs/getBoardRankList`。实测返回 A 股榜单，`total=5576`，单页 `n=200`；`sort_type` 白名单只有 `price`、`turnover`、`volume`、`pn`、`speed`。传 `zdf`、`hsl`、`zllr`、`ltsz` 等会被服务端拒绝，返回 `code=1619377101`，`msg=参数错误:sort_type error`。