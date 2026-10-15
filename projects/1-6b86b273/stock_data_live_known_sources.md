---
name: stock_data_live_known_sources
description: A股各数据源已验证可用性与已知坑，避免重复测试
type: fact
---
已验证可用：东财 push2his clist (total=5923)；新浪 vip getKLineData (datalen=6000 拿全史)；腾讯 fqkline；东财 datacenter 6 报表；集思录 10 接口（需匿名 session + Referer + XHR）；雪球 stock.xueqiu.com/v5/quote.json（需预热拿 xq_a_token）。已知坑：push2his kline rc=102 已废；腾讯 fqkline qfq 有负价 bug，hfq 正常；新浪 MoneyFlow 有子代理报告失效。