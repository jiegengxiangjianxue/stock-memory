---
name: website_data_sources_and_ts_audit
description: 网站前端依赖的 49 个 JSON 数据源清单，以及用 ts 字段做新鲜度审计的排障方法
type: fact
---
前端 SOURCES 数组定义 49 个数据文件，关键项包括 data/latest.json (主快照)、data/hot_impact.json (热点推演)、data/cycle.json (周期池)、data/scenario.json (场景)、data/market_now.json (实时行情每5分钟)、data/realtime_minute.json (分钟级每3分钟) 等。审计时读取各文件 ts 字段与当前时间对比；若出现大量 ts 整体停滞、cycle_deep.json ts 为空、cycle.json 的 tier1/tier2/pool 为 None、sector_pool.json tier1=0，说明对应模块数据未生成或未提交，需检查上游生成脚本与 nightly-backfill 产物。