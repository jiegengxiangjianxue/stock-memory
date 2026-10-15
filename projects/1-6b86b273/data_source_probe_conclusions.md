---
name: data_source_probe_conclusions
description: 数据源可用性结论 (kaipanla/10jqka/fund.cc.cd/yjc.fund)、东财 WAF 降级方案、TSP v3 公式与仓库 token 状态，接入新数据源或改算法前读
---
# 数据源接入矩阵与探测结论 (2026-10-12/13)

## 零凭据可用
- 腾讯前复权 K 线（RPS 在用）
- 新浪 7x24 新闻主源
- 东财 pingzhongdata 基金：GET `fund.eastmoney.com/pingzhongdata/{code}.js?v={ts}` 200，无鉴权 JS 字面量，`literal_eval` 解析；字段 fS_name/fS_code/fund_Rate/fund_minsg/stockCodes/ishb
- 抖音热榜 `aweme.snssdk.com/aweme/v1/hot/search/list/`：移动端 UA，按 `video_count` 排序；`hot_value` 全 0；候选 404 已排除

## 需 cookie/浏览器渲染
- 小红书（SPA+签名）、雪球（需 `xq_a_token`）、腾讯新闻作者页（SPA 仅嵌 channelInfo，后端全 404）、东财 FundJZ.aspx/FundGS.aspx（本地 403 WAF）、开盘啦（app/xg 子域 DNS 不可解析，www open_api.php 被 Nuxt SPA 劫持）、同花顺 data.10jqka（无公开 JSON API）、基估宝/养鸡场（SPA 壳，养鸡场持仓端点回壳）

## 东财 WAF 降级链
push2 clist → 本地 TLS reset → `mainline_rank` 涨停池主题聚类占比兜底；7x24 容器 567/400 → 新浪主源 + 远端 data-news 兜底；cls.cn 404 跳过。GitHub Actions 出口全通，降级仅影响本地。

## 双库隐患
tools-repo 下另有独立 `stock.duckdb`，与 stocks-ai-view 的 DuckDB 层并存，注意勿混用。

# 2026-10-13 晚 新增/修正

## 新可用源（已实测 200）
- `em_push2his_clist`：push2his.eastmoney.com/api/qt/clist/get — **push2 WAF 绕行方案**（push2 域 TLS reset，push2his 同路径可用，total=5923 全 A 股）；坑：`pz` 上限 100，必须分页循环
- `em_push2his_etf_onsite`：同上 `fs=b:MK0021` — 场内基金/ETF 1357 只
- `em_fund_lsjz`：api.fund.eastmoney.com/f10/lsjz — **Referer 静默鉴权**：不带 Referer 返回 200+ErrCode=-999+空数据（比 403 危险，必须校验 ErrCode）
- `jisilu_dividend_rate`：jisilu.cn/data/stock/dividend_rate_list/ — pb/pe_temperature 历史分位 + 5年均值
- `sina_vip_kline`：vip.stock.finance.sina.com.cn — 多周期 + 直返 MA5/MA10，K 线交叉校验源
- 集思录 10 接口（2026 新版无后缀路径，老 `*_list_ajax.php` 全 404）：`/data/lof/index_lof_list|stock_lof_list/`、`/data/etf/etf_list/`（费率/申赎单位独家）、`/data/cbnew/cb_list_new|adjust_list|delisted/`、`/data/new_stock/apply|hkipo|sub|super/`。需匿名 session（首页拿 kbzw__Session）+ Referer + X-Requested-With
- baostock、akshare 1.19.1 容器已装；同花顺 ths90 财务摘要（RPT 字段名：`WEIGHTAVG_ROE`/`BASIC_EPS`/`MGJYXJJE`，混有新三板需过滤）

## 已失效
- **`em_push2his` kline 端点 rc=102**（19:04 实测 200，21:xx 复测失效）→ K 线一律用 `tencent_kline`(fqkline) 或 `sina_vip_kline`；clist 仍可用勿混淆

## 腾讯指数翻页坑
`kline/kline` 的 `end` 参数必须 `YYYY-MM-DD`（`20200101` 返回空），且返回数组**升序**——最早日期在 `seg[0][0]` 不是 `seg[-1][0]`。修后拿到 8567 根全史

# 数据准确性核查结论 (2026-10-13，用户反馈板块/涨停数据错)

准确（已交叉验证）：上证 K 线与腾讯逐根吻合；涨跌分布 2831/2384/149 一致；tc 权威计数 涨停67/跌停8/炸板19。

问题（已修）：
1. `latest.json` 是 6.1h 前旧快照 → 加 `stale` 标记
2. `market_now.json` 涨停 89 vs tc 67 口径不一致 → 统一为 tc，宽口径存 `*_wide`
3. `latest.json` advance/decline 偏差 188/166（盘中未刷新）
4. `sector_pool.json` `tier3_count=68` 但 `tier3=[]` 计数脱节 → 已修

工具：`scripts/data_audit.py`（--check 时效+字段校验 / --dedup-write / --stamp-write provenance）、`scripts/fix_market_now_caliber.py`。

教训：**页面消费的 JSON 必须带 stale 标记 + provenance（source/confidence/snapshot_ts）**，否则用户看到旧数据无法察觉。

# 分钟级实时方案结论 (2026-10-14)

## 限流实测（30 分钟压测，结论：可行）
- `push2delay.eastmoney.com` 60s 频率、60 页/轮 × 30 轮 = 1800 页请求，**仅 1 次瞬时失败 (0.06%)**，每轮 15-24 秒
- 子代理推的「19 分钟触发限流」**没发生**（它基于腾讯 qt.gtimg.cn 旧结论，源不同）
- 结论：限流不是瓶颈；真正延迟在 `git push → GitHub CDN`(1-5 分钟)

## Codespaces 不能长期常驻（官方文档）
- idle timeout 最长 240 分钟（4h），默认 30 分钟；「空闲」按终端/VS Code 活动判定
- 免费额度 120 core-h/月（Free）/ 180（Pro），2 核机器实际 60h/月
- 交易时段常驻需 ~121h/月 → **超免费额度 2 倍**，超额 $0.18/core-h 且需绑卡

## 最终架构（免费优先）
- **GitHub Actions** `realtime-minute.yml`：每 3 分钟单轮采集（stdlib only，无 pip install，~90s/轮），528 分钟/月额度内
- **GitCode Space**（1000 核时/月，独立额度）跑引擎重活 → `space-pull.yml` 拉回 GitHub data/
- **GitCode Notebook**（2h 上限 + 不持久化）只做一次性批量回补（复权补录）
- 展示层 GitHub Pages 读 raw（`data_origin_patch.js` GitHub 优先 → gitcode 镜像 → 本地兜底）

## 关键文件
- `scripts/realtime_minute.py`：常驻/单轮采集器（5923 只/轮，字段已折算）
- `scripts/probe_rate_limit.py`：限流实测工具
- `scripts/deploy_cloud.sh`：云部署（systemd + 双 remote）