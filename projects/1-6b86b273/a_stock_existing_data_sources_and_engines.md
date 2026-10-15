---
name: a_stock_existing_data_sources_and_engines
description: A股项目已实现的数据源、引擎与网站形态，避免重复建设
type: fact
---
项目已有 161 个 Python 脚本在 GitHub Actions 云端运行，网站为 GitHub Pages 静态站 + JSON。已实现数据源：腾讯、东财、同花顺、新浪、Newsnow、集思录、KlineShare。已有引擎：engine_v5（17 技术指标共振）、十维因子、消息面三引擎、周期、海外关联、打板统计、涨停归因、权重校准、K线模拟回测、walkforward、形态识别（patterns.py）。