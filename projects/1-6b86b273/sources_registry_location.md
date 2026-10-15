---
name: sources_registry_location
description: 项目数据源注册表路径与现有规模，调研数据源时需先读取以避免重复
type: workflow
---
stocks-ai-view 项目的数据源注册表以 scripts/sources_registry.py 为唯一事实来源（对应 data/sources_registry.json，当前 83 个源）；调研或新增接口前，需先 grep ~/workspace/stocks-ai-view/scripts/sources_registry.py 或运行 --list 确认 id 不存在以去重。同一厂商的新接口可视为新源，完全重复的排除。采集器只读注册表、不硬编码 URL；新源要加记录并写 fetch 函数，status 以 probe 实测为准。已知勿重源包括：腾讯 qt.gtimg.cn/q=、腾讯 web.ifzq.gtimg.cn fqkline/分时/分笔、新浪 vip.stock.finance.sina.com.cn、新浪 money.finance.sina.com.cn、新浪 hq.sinajs.cn、东财 push2*/datacenter、集思录、akshare、tushare、tradingview、yahoofinance、coingecko、toutiao、douyin、xueqiu_anon_token。