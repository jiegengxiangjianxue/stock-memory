---
name: market_phase_source
description: 判断牛/熊/震荡市场环境的字段来源
---
市场状态（牛/熊/震荡）从 `market_now.json` 或 `latest.json` 的 `market_phase` 字段读取。reflect_engine.py 输出胜率时须按此字段区分市场环境。