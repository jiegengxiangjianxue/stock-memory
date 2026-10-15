---
name: a_share_pipeline_perf_baseline
description: A股采集与引擎链的实测耗时基线，用于判断压缩更新周期时各阶段是否有余量
type: fact
---
- 单轮全市场采集：16s（东财 push2delay 正常时）至 80s（东财被墙走新浪降级时）。
- 轻量引擎链（engine_v5 + decision + today_picks + ai_pick_v4）本地实测 65s。
- engine_v5 全量 3997 只 61s。
- 上述数据为已确认事实，用于评估 2 分钟周期方案时是否需要拆分并行或接受降级窗口。