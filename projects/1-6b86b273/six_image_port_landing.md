---
name: "six_image_port_landing"
description: "六张参考图的借鉴落位清单：哪些已落地、哪些查重跳过、哪些暂缓；避免后续会话重复借鉴或覆盖口径"
type: workflow
---
# 六张参考图借鉴落位 (2026-10-17, commit 92e1aea)

六张图的功能已合并去重落地（用户要求不含财务分析、不含自定义布局）：

已落地：
- 关键价位引擎 `scripts/key_levels.py`：候选股 Pivot R1/P/S1 + 布林(20,2) + Keltner 短/中/长 + ATR(14) 通道 + Donchian20 + Vegas 隧道 + 10/30日 MA 偏离全市场百分位排名 + 60日新高/新低广度（同一次 K 线遍历）；全部复用 indicators_ext，零新指标代码。输出 data/key_levels.json。
- 点位提醒：realtime_minute.py 每轮对 key_levels 枢轴 R1/S1（自动）+ data/price_alerts.json（手工）做真穿越检测（对上轮快照），同日同 key 去重，事件写 data/market/alert_events.json。
- 区间高点追踪 `scripts/pick_high_track.py`：选出后 7 交易日区间最高点（借鉴「AI 选股历史」），与收盘胜率互补；ai_picks 快照优先于 engine_v5 快照（新口径不混测）；build_ai_picks 起每天归档 data/predictions/ai_picks_YYYY-MM-DD.json。
- 指数站上 MA5/20/60：build_market_now.py 从 index_kline 计算，写入 market_now.market.indices_ma。
- hot_impact：每只个股判断带规则生成的证伪条件 invalidation；每个主题带扩散路径 path（热点→涨停龙头→动量扩散）。
- 前端：关键价位面板（#levelsBox）、点位提醒/指数MA/60日广度（盘面摘要）、区间高点（收盘复盘后）、hot_impact 证伪条件列+扩散路径行。

查重后跳过（已有）：板块异动时间线(tape_structure)、预计全天成交额(amount_curve)、封板率(zt_rate)、5分钟量能榜(speed_leaders)、竞价分析(auction_engine)、每日报告时间线(morning/afternoon/close review + daily_brief)。

暂缓：竞价快照回放（需分钟级竞价 tick 持久化）、语音播报、模拟盘（全局 skill 已有）、B 站视频资讯源。

24295.jpg 是 B 站视频信息流截图（分区/排行），与股票功能无关，未借鉴。