---
name: global_panel_dataflow
description: 全球→A股传导引擎的数据路径、K线构建与输出规范
type: workflow
---
`engines/global_panel.py` 读取 `data/global_kline.json`（由 `collect_global_kline.py` 每日增量构建，腾讯 fqkline 单次上限640根），输出 `data/global/latest.json`。计算 A50期货、KWEB、VIX、离岸人民币、SOXX、纳指等全球→A股传导信号（`global_score`/`global_mood`/`global_mom`），供网站与复盘使用。