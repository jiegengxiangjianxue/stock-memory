---
name: "stage_aware_quote_validation"
description: "实现午盘和收盘复盘时应遵循的真实交易日、行情覆盖与阶段口径"
type: workflow
---
午盘必须验证同一交易日的 `premarket_picks` 且生成时间早于 09:15；对全部 A/B 候选代码批量调用 `signal_hub.quotes`（一次调用），兼容带前缀和裸代码，记录价格、行情时间、来源和状态；无行情必须标记 `no_quote`，不能保留为 `KEEP` 或 `no_data`，且不能把收盘后的 `market_now` 当作早盘行情。收盘复盘以同日 `premarket_picks` 和 `midday_picks` 为主输入，对候选并集批量取行情一次，分别生成阶段摘要和缺失覆盖率；不支持的阶段输出 `insufficient`，不能编造胜率，也不能把旧 review 标成 fresh。