---
name: stocks_data_live_network_facts
description: stocks-data-live 项目容器环境限制与已验证股票数据源可用性清单
type: fact
---
容器为 Alpine aarch64，有 node v22.22.1，无 npm/npx。已验证可用：腾讯 qt.gtimg.cn 实时行情、web.ifzq.gtimg.cn 日K全史、新浪 hq.sinajs.cn、东财 push2his.eastmoney.com K线、datacenter-web 报表、同花顺 10jqka、集思录、TradingView scanner。已验证受限：push2.eastmoney.com 全域被 WAF TLS reset；push2delay.eastmoney.com 间歇可用（逐笔成交 3 秒快照、历史资金流 kline 已验证）；雪球需 xq_a_token cookie；巨潮/深交所/上交所 API 不可用。GitHub Actions 出口全网畅通。数据源普查文件：/root/workspace/stocks-data-live/data/research/data_source_catalog_20261013.json。