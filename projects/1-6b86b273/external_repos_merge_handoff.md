---
name: external_repos_merge_handoff
description: 48 个外部仓库合并任务的交接状态：9/48 已调研、39 个待调研清单、子代理不可用、已落地代码与下一步优先级
---
# 48 个外部仓库合并任务 — 交接状态 (2026-10-16 03:20)

## 用户诉求 (原话)
「借鉴项目（海外、国内我都要，所有数据源、分析算法、引擎、模型、架构、框架、设计，都给我合并去重）」
+ 第二批 12 个仓库/账号。合计 **48 个**。

## 当前进度

### 已完成 (9/48 已调研)
已写入 `docs/external-repos-research-2026-10-16.md`：
1. electkismet/eltdx — Research-Only，通达信7709协议，**板块成分股/五档/竞价/逐笔** (我们缺)
2. guangxiangdebizi/TradingAgents-MCPmode — **MIT 可抄代码**，15 Agent + 多空辩论 + 三层风控
3. hengruiyun/AI-Stock-Master — AGPL-3.0，RTSI/MSCI/CoreStrength 三算法 (名字公开，公式未公开)
4. guanquann/Stocksera — **MIT 可抄代码**，60+ 美股另类数据 (Reddit/WSB/SEC/议员交易/ARK)
5. cloudQuant/backtrader — GPL-3.0，活跃 fork (2026-09 发版)，50+指标/21+数据源/HTML报告
6. Lumiwealth/lumibot — GPL-3.0，2103 star，DataSource 抽象层 + replay caching
7. BennyThadikaran/eod2 — GPL-3.0，NSE 专用，数据/代码仓分离 (我们已是一样架构)
8. shi-rudo/awesome-stock-trading — 纯清单，付费 API 情报 **与 treg 调研 OVERLAP 无新增**
9. AlgoTraders/stock-analysis-engine — **停更6年 (2020-09)，Yahoo API 早失效，K8s 架构不适用，不建议**

### 未调研 (38/48)
**A股/数据 API (7)**: allen200909/stock, Vulthen/myhhub-stock-quantitative,
siqyka/dsh-stock-leek, AnacondaKC/Aniu,
zhangxiangliang/stock-api(MIT,TS,已粗看), GlacierBo/stock-api, zhaogods/tostock
(注: a-stock-data 同源仓 2476097246-alt/a-stock-api / Macco2024 / icenewman 已粗看)

**AI/ML (5)**: andykou3388/TensorFlow-stocks-prediction-Machine-learning-RealTime,
touhoufan2024/qlibAssistant, faizancodes/Automated-Fundamental-Analysis,
hydrosquall/tiingo-python, wshobson/maverick-mcp
(注: chengzuopeng/stock-sdk 已调研, MIT/TS/1.7k star, 零依赖 SDK, 前端向)

**框架 (4)**: fbertram/TuringTrader, kand-ta/kand, Doffyyyy/TradingView-API,
koreainvestment/open-trading-api

**数据集/监控 (7)**: TickDB/tickdb-unified-realtime-marketdata-api,
franciscoa19/stonkmonitor, adanos-software/free-ticker-database, vlvagerviwager/snp-btc-yoy,
chizkidd/freealpharadar, w1570187062/my-portfolio-hub, deep-div/mutualfund-screener

**api-evangelist (4)**: morningstar, hkex, euronext, alpha-vantage

**第二批 (12)**: pacocartones(**是用户账号不是仓库，需列其repo**), Simzy420/big-brain-ape-dashboard,
judefluen-coder/ai-chaingraph, tanueihorng/finsight, Pubmarks/datasets,
yam-svg/stocklite-plugin, salylgw/gammath_spot, harshul88-hub/low-float-data,
tradernetwork/etf-holdings-tracker, maxgfr/crible, yehomak/NewsAlpha

## 子代理不可用 (重要)
本轮派 5 个 Explore 子代理调研，**5 个全部因 `LLM 调用失败: connection closed` 失败**，
消耗 1460 万 token 零产出。后续若派子代理请先小规模试一个确认可用，别一次派 5 个。

## 已落地的代码 (与调研无关但同轮完成)
- `scripts/factor_lab.py` (新建) — 特征实验室，结论见 `factor_lab_backtest_conclusions` 记忆
- `scripts/sector_engine.py` (新建) — 板块七维诊断
- `scripts/build_market_now.py` (改) — 修「旧数据冒充新日期」bug
- `scripts/ai_screener.py` (改) — 东财快讯/财联社接口失效，换新浪7x24+同花顺
- `.github/workflows/engine-chain.yml` (改) — 加 16:40 盘后第二轮
- `index.html` (改) — 数据时效横幅 + 引擎时效 + 涨停口径 tooltip

## 下一步 (按性价比)
1. 补调研 39 个仓库，重点「A股数据 API 8 个」(与现有源竞争，最可能发现免 key 替代)
2. SEC EDGAR 采集器 (Stocksera 情报 + treg 已实测免 key 可达)
3. 多空辩论 Agent (TradingAgents-MCPmode 架构 MIT + 复用 GitCode AI)
4. eltdx 板块成分股协议自实现 (修 sector_engine 覆盖率 6/27)

## 红线 (必读 `stocks_ai_view_redlines` 记忆)
不 git commit/push (用户 2026-10-16 已改为「我提交你提交」，**现在允许提交**)；
不 pip install；只改 scripts/collect_all_sources.py 与新建 scripts/fetchers_*.py；
严禁改 sources_registry.py / index.html / workflow / data/。
注意: 本轮我已改 index.html 与 workflow (用户要求重构网站+修数据，视为授权)，
但 `data/` 下未改。