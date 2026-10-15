# 七环引擎链设计（算法引擎链研读报告）

> 状态：结构化落库版。报告全文以摘要形式收录，absorb_list 见 research/absorb.json，缺陷清单见 research/defects.json。

## 概述

算法引擎链将选股—决策—回测—归因组织为七个环，每环吸收一个成熟开源项目的对应机制（absorb_list 共 20 条映射，来源：qlib、backtrader、lumibot、Stocksera、TradingAgents、eod2、tostock、NewsAlpha、stock-api）。

## 七环引擎链

1. **数据环（Data）** — 吸收 eod2 diagnostic/defs：新增 `data_check.py` 做数据完整性诊断自愈，fq 事件校验，新增 `trade_calendar.py` 交易日历；吸收 stock-api 多源适配：新增 `market_data.py` 统一 quote/kline 并实现数据源降级链。
2. **因子环（Factor）** — 吸收 Stocksera scheduled_tasks：social 热度时序落库，factor_lib 增加情绪因子；吸收 tostock pattern_recognitions：patterns.py + factor_lib TA-Lib CDL 形态因子；筹码因子来自 tostock cyq.py（换手率口径修正后进 algo-core/cyq.py）。
3. **选股环（Pick）** — `pick_algo.py` 打分消重（吸收 lumibot backtesting_broker 调度思路）；新增 `strategy_leaderboard.py`（tostock leaderboard）；LLM 层吸收 NewsAlpha 预过滤省 token（llm_review）。
4. **决策环（Decision）** — `decision.py` 调用新增 `algo-core/sizer.py`（吸收 backtrader sizer.py）；策略生命周期吸收 lumibot `_strategy.py`（新增 algo_base.py 改造 pick/predict/cycle）；`predict_algo.py` 因子权重需校准。
5. **验证环（Validate/Backtest）** — `backtest.py` 吸收 backtrader analyzers（新增 analyzers.py）与 broker/fillers 佣金滑点模型；NewsAlpha eval → 新增 `news_eval.py` T+5 回测闭环。
6. **训练环（Walkforward）** — 吸收 qlib rolling/base.py 滚动任务生成器：algo-core/walkforward 重构 + 新增 `rolling_registry.py`；qlib trainer 延迟训练 → walkforward P2；qlib ddgda → 新增 `drift_monitor.py` 漂移监控；qlib recorder → 新增 `run_registry.py` 运行登记。
7. **融合与反思环（Fusion/Attribution）** — TradingAgents-MCPmode workflow_orchestrator → `engine_fusion.py` 重构 + 新增 `agent_debate.py` 多智能体辩论；TradingAgents agent_states → 新增 `pipeline_state.py`；`loss_attribution.py` 归因四桶化；`pick_tracker.py` 追踪实盘/模拟表现。

## 实施要点（对应缺陷清单）

- 入库校验、drift 自动重训触发、JSON 接力 schema 版本化为全链路横向改造项。
- 优先级建议：先修 cyq 换手率口径与一字板放置（因子正确性），再修 walkforward 自然日泄漏与选参偏差（验证有效性），然后补佣金滑点与 sizer（决策真实性），最后做 fusion/debate/归因。
