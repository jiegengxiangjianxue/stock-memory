---
name: "increment_recent_klines_before_review"
description: "本地 K 线滞后会导致快照、复盘和回测缺少最新数据，应先增量补录最近 K 线。"
type: fact
---
已确认的问题是本地 bulk K 线最后日期落后于实时快照，导致复盘或回测缺少最新数据，`reflect_engine` 无法验证。修法是从行情源拉取最近 N 根 K 线，仅追加本地不存在的日期，不修改已有历史数据。