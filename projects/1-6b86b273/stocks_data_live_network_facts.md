---
name: stocks_data_live_network_facts
description: stocks-data-live 项目容器环境限制与已验证股票数据源可用性清单
type: fact
---
Alpine aarch64 容器，node v22.22.1，无 npm/npx，无法安装 REA 等 npm 包。股票数据源：83 条注册于 `/root/workspace/stocks-ai-view/scripts/sources_registry.py`（57 可用）；普查报告 `/root/workspace/stocks-data-live/data/research/data_source_report_20261013.md`。已知可用：腾讯 qt.gtimg.cn 实时行情、web.ifzq.gtimg.cn 日K全史；新浪 hq.sinajs.cn、vip.stock.finance.sina.com.cn；东财 push2his.eastmoney.com K线、push2delay.eastmoney.com 间歇可用（逐笔成交 3 秒快照、历史资金流 kline 已验证）、datacenter-web 报表、push2ex；同花顺 10jqka；集思录；TradingView scanner；雪球 stock.xueqiu.com（需 xq_a_token cookie）。已知被封/不可用：容器内 push2.eastmoney.com 全域 WAF TLS reset；巨潮/深交所/上交所 API；小红书/腾讯新闻作者页 SPA。GitHub Actions 出口全网畅通。