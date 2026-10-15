---
name: global_markets_data_sources
description: 实现或修复全球股市/韩股行情、竞价抢筹榜前读取：东财无KOSPI、新浪znb_KOSPI可用、fbt字段省略前导零、采集时段缺口
type: fact
---
# 韩股/全球行情数据源事实 (2026-10-15 实测)

## 韩股 KOSPI
- **东财没有 KOSPI 的 secid**。`100.KOSPI` 查了直接返回空（接口不报错，静默丢弃该 secid，只返回同批其它指数）。
- 东财搜索接口能搜到正确代码：`100.KS11`（韩国KOSPI）、`100.KOSPI200`（韩国KOSPI200），QuoteID 形式。但 `push2 ulist.np` 用这两个 secid 实测连不上（RemoteDisconnected，疑似该域名对我方 IP 限流）。
- **可用源：新浪 `hq.sinajs.cn/list=znb_KOSPI`**（需带 `Referer: https://finance.sina.com.cn/`，否则 403）。返回 GBK，格式：
  `var hq_str_znb_KOSPI="首尔综合指数,5587.4700,-421.34,-7.01,2:27 AM,..."`
  字段：0=名称 1=现价 2=涨跌额 3=涨跌幅
- 腾讯 `sqt.gtimg.cn` 不覆盖日韩欧（只支持 A股/港股/美股），`s_krKS11` 无效。

## 其它全球指数
- 腾讯 sqt 可用：A股(`s_sh000001` 等)、港股(`s_hkHSI`/`s_hkHSCEI`/`s_hkHSTECH`)、美股(`s_usDJI`/`s_usIXIC`/`s_usSPX`)。
- 东财 `push2 ulist.np` 可用：`100.N225`(日经)、`100.FTSE`/`100.DAX`/`100.CAC40`(欧股)。
- `scripts/collect_global_markets.py` 三源合并：腾讯优先 → 东财补 → 新浪覆盖韩股（已实现）。

## 涨停池首封时间字段 (做竞价抢筹榜的依据)
`collect_market_extras.py` 的 `fetch_pool` 里，东财字段 `fbt` 是首封时间，**会省略前导零**：
`92500` = 09:25:00（5 位），也可能给满 6 位。解析必须 `t.zfill(6)` 再切片。
09:25 整封板 = 集合竞价阶段资金直接抢筹，据此推导「竞价抢筹榜」，无需额外接口（`build_jingjia()`，已实现）。

## 采集时段覆盖缺口 (已修)
`shard-collect.yml` 的 cron 只跑 UTC 01:00-08:55 = CST 09:00-16:55，恰好只覆盖 A 股时段。
于是欧美盘中/收盘、亚洲早盘这三个窗口里 `global_markets.json` 一直是旧快照，「全球股市」tab 数据停十几小时。
2026-10-15 新增 `.github/workflows/global-markets-realtime.yml`，cron `0 8,9,10,14,15,16,17,18 * * 1-5` 补这些时段。