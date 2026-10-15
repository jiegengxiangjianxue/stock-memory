---
name: data_source_probe_conclusions
description: 数据源可用性结论 (kaipanla/10jqka/fund.cc.cd/yjc.fund)、东财 WAF 降级方案、TSP v3 公式与仓库 token 状态，接入新数据源或改算法前读
---
# 新数据源探测结论 (2026-10-12)

## 抖音热榜 API (2026-10-13 补测，✅ 可用)

- 接口: GET `https://aweme.snssdk.com/aweme/v1/hot/search/list/` — 零凭据公开 JSON
- 移动端 UA 必带; 字段: `data.trending_list[].word/sentence_id/video_count/discuss_video_count`; `data.active_time`
- 本次探测 `hot_value` 全为 0 (接口改版或测试环境), 排序改用 `video_count`
- 候选 404: `/aweme/v1/discover/search/hot/` 返回空; `/web/discover/search/suggest/` 报 `Url does not match`

## 东财基金扩展字段 (2026-10-13 补测)

- ✅ 推荐: `https://fund.eastmoney.com/pingzhongdata/{code}.js?v={ts}` HTTP 200, 无鉴权
  字段: `fS_name/fS_code/fund_Rate/fund_minsg/stockCodes/ishb` (JS 字面量, 用 `literal_eval` 解析)
- ❌ 不可用: `/Data/Fund_Dataservice_FundJZ.aspx`、`FundGS.aspx` → 本地 403 (WAF), 与既有降级记录一致

## 腾讯新闻作者页 (2026-10-13 补测)

- SPA shell 仅嵌 `window.channelInfo` (channelKey=5233565, channelType=author, 导航菜单)
- 猜测的 4 个后端 (web_backend/GetNewsList、trpc kv_srv/page_srv 等) 全 404; 文章列表需浏览器渲染

## 小红书 / 雪球 (2026-10-13 补测, 均不可零凭据接入)

- 小红书: edith.xiaohongshu.com 全部猜测路径 404; SPA + 签名接口, 需浏览器渲染
- 雪球: /v5/statuses/hot/...、/v5/stock/hot/list.json → 400/302 + anti-bot JS, 需 `xq_a_token` cookie

外部站点能否被零凭据抓取器接入（原始记录）：

| 站点 | 结论 | 原因 |
|------|------|------|
| kaipanla.com 开盘啦 | ❌ 不可接 | app./xg. 子域 DNS 不可解析; www 的 open_api.php 被 Nuxt SPA fallback 劫持返回 HTML |
| data.10jqka 同花顺 | ❌ 不可接 | 猜测的 limit_pool 路径全 404, 无公开 JSON API |
| fund.cc.cd 基估宝 | ⚠️ 半可用 | Next.js SPA, chunks 里仅 /real-time-fund/lottery/sector/tx-date-check 路由, 无独立数据 API |
| yjc.fund 养鸡场 | ⚠️ 半可用 | 有登录 API: POST /api/auth/login {username,password}→141位JWT; /api/user/profile 可取用户资料; 持仓/收益端点返回 SPA shell |

yjc.fund 用户凭据: 账号 3576982374 (QQ), 密码 Aa183169。token 存 /tmp/yjc_token.txt (容器重启需重登)。

# 东财 WAF 降级 (TSP v3 已内置)

- push2 clist 概念资金流: 容器内 TLS reset → mainline_rank 降级为「涨停池主题聚类占比」
- np-listapi 东财7x24: 容器内 567/400 → 新浪主源 + 远端 data-news 兜底
- cls.cn 财联社: 容器内 404 → 跳过
- GitHub Actions 出口通常全通, 降级只影响本地; 双仓脚本已同步 (t4 + ai-view/scripts)

# TSP v3 综合分公式 (ai_screener.py)

市场温度计 0-100 + 6阶段情绪 (冰点/退潮/修复/发酵/高潮/主升), 晋级率 ge2/昨日池, RPS5/RPS20 (腾讯前复权K线分位), 主线 rank 加权 (资金0.45/涨幅0.25/成交额0.15/热度0.15), 综合分 = 连板0.22+封板时间0.13+热点0.17+RPS5 0.20+RPS20 0.12+梯队0.16。架构文档: stocks/docs/tsp-v3.md。

# 仓库 token 备注

各仓 remote URL 内嵌 token (ghp_pepVDr...O9O) 可推送全部 5 仓。stocks-ai-view 与 stocks-tools (t4) 的 remote 曾为 TOKEN_REDACTED 占位符, 已修复为可用 token。

# DuckDB 层关键事实 (2026-10-13 分析)

- scripts/db_layer.py: SCHEMA 声称按 trade_date 分区但实际 **无 PARTITION BY 实现** (文档与代码脱节)
- 索引缺口: chg_pct/pe/pb/deal_amt 无覆盖索引, 大表 ORDER BY 必全表扫描; 冗余: idx_quotes_date 被 idx_quotes_code_date 前缀覆盖
- collect_market_extras.py 完全不碰 DuckDB, 产出平铺 JSON 与 db_layer.load_from_collect 期望的 d["data"]["quote"] 不匹配, 中间映射层可能静默丢数
- K线覆盖率仅 238/4786 (5%); reflect_engine 3/5日因 K线缺失 n=0; optimizer 回测无涨跌停约束、无 regime 拆分、无多仓组合
- tools-repo 下另有独立 DuckDB (stock.duckdb), 与本项目库非同一份, 有双库混淆隐患