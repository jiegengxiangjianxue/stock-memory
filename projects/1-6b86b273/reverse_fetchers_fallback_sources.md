---
name: reverse_fetchers_fallback_sources
description: 逆向或替代路径获取的数据源列表，用于原接口失效时切换
type: fact
---
`scripts/fetchers_reverse.py` 存放原 fetchers_extra 中没有、通过逆向或替代路径找到的数据源。已验证可用：
1. **sohu_hisHq**: 搜狐历史日K，作为东财 WAF 备援。URL: `https://q.stock.sohu.com/hisHq?code=cn_600519&start=..&end=..`。返回 JSON 数组，字段含 `[date, open, close, chg, chg%, low, high, vol, amount]`。注意：部分股票 503 限流，港股/美股 code 格式不同暂不支持。
2. **sina_industry**: 新浪行业板块，替代全 404 的同花顺 ths_industry。URL: `https://vip.stock.finance.sina.com.cn/q/view/newSinaHy.php`。返回 JS 赋值语句，需正则解析，含行业名/公司数/涨跌幅。
3. **eastmoney_news**: 东财 7x24 快讯，已从 fetchers_extra 迁来。