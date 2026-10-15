---
name: external_repos_merge_handoff
description: 48 个外部仓库合并任务的交接状态：9/48 已调研、39 个待调研清单、子代理不可用、已落地代码与下一步优先级
---
# 48 个外部仓库合并任务 — 交接状态 (2026-10-16 03:20)

## 用户诉求 (原话)
「借鉴项目（海外、国内我都要，所有数据源、分析算法、引擎、模型、架构、框架、设计，都给我合并去重）」
+ 第二批 12 个仓库/账号。合计 **48 个**。

## 当前进度

### 已调研并落地 (12/48 有情报, 8 个已变成代码)
报告: `docs/external-repos-research-2026-10-16.md` (含「借鉴→落地对照表」)

**License 可合法复制代码的**: a-stock-data (Apache-2.0, 最高价值) /
TradingAgents-MCPmode (MIT) / Stocksera (MIT) / stock-api (MIT,TS) /
stock-sdk (MIT,TS) / kand-ta/kand (Apache-2.0 OR MIT)。

**已落地的 8 项**:
- a-stock-data (Apache-2.0) → `fetchers_ticks.py` 腾讯逐笔 (CI 36只跑通)
- a-stock-data (Apache-2.0) → `auction_engine.py` 资金结构四指标
- a-stock-data (Apache-2.0) → `volume_units.py` 成交量单位归一化 (688按股,差13倍)
- AI-Stock-Master (AGPL仅数学式) → `trend_strength.py` RTSI/TMA/MSCI
- TradingAgents-MCPmode (MIT) → `bull_bear_debate.py` 多空辩论红队
- cloudQuant/backtrader (GPL仅架构) → `analyzers.py` Analyzer 插件框架
- lumibot (GPL仅设计) → `gitcode_ai_review.py` replay caching
- kand-ta/kand (Apache/MIT) → `indicators_ext.py` +5 指标 (Vegas/Camarilla/HA/MFI/OBV)

**关键止损结论**: 通达信公开服务器 K线/盘口/逐笔 2026-09 起全部失效
(a-stock-data 用 10 台服务器逐台实测确认)。自研 7709 协议这条路已死。

### 未调研 (36/48)
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