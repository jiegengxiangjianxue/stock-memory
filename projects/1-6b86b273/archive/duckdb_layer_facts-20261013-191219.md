---
name: duckdb_layer_facts
description: stocks-ai-view 的 DuckDB 库现状与 db_layer.py 索引/并发/双库结论，再做 DB 优化或查询前读
---
# DuckDB 库层已核实事实（2026-10-13 分析）

- 库文件：`~/workspace/stocks-data-live/stocks.duckdb`，5.8M，DuckDB 1.5.6。
- 数据量极少（新库）：quotes 6018 行（仅 2026-10-13 一天）、events 213 行（仅 10-12）、kline_daily 0 行、pools 0 行、margin 30 行、sources_log 3 行。
- `db_layer.py --bench` 在 6K 行下全部 1~8ms，无生产规模参考价值。
- 索引（duckdb_indexes 实测存在 8 个）：idx_quotes_date(trade_date) 被 idx_quotes_code_date(code,trade_date) 前缀覆盖，属冗余写放大；排序列（chg_pct/pe/pb/deal_amt）无覆盖索引。
- 分区脱节：db_layer.py 顶部注释宣称 quotes/events/pools 按日期分区，但 SCHEMA 实际无任何 PARTITION BY。
- 并发：connect() 每次新建连接，无连接池/锁/WAL，多 --load 写者靠文件锁串行。
- collect_market_extras.py 是零依赖 HTTP 采集器，完全不碰 DuckDB（grep 无 SQL）；其平铺输出结构与 load_from_collect 期望的 d[data][quote/event] 不匹配，中间映射层未核实。
- 双库隐患：tools-repo 下另有 /home/agentuser/stockdb/stock.duckdb（minute 分钟线库），与本项目库不是同一份。
- 完整报告：/tmp/db_optimization_report.md（/tmp 易失，重要结论以本记忆为准）。

# 复盘闭环与 K 线覆盖 (2026-10-13)

- **预测持久化**：data/predictions/engine_v5_YYYY-MM-DD.json，CI full-pipeline 每周期存一份；reflect_engine `--pred-dir data/predictions` 读全部历史快照做前向验证。3/5/10 日胜率样本随时间逐日累积（<30 标 insufficient）。
- **kline_sim.py**：`--source predictions` 读历史预测回测；当天信号无法回测（前视防护，属正常）。
- **expand_kline_coverage.py**：腾讯+东财双源 8 并发扩 K 线，覆盖从 4.5%→51%（2737/5343），增量写 day_qfq_expand.csv，与 kline_sim 的 load_klines 格式兼容。
- **CI 闭环**：full-pipeline 5b 存预测快照 → 6 复盘 → 6b kline_sim+winrate_matrix → commit 到 GitHub；前端 fetchJSON 可从 raw.githubusercontent.com 兜底读。