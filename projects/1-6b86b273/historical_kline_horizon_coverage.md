---
name: "historical_kline_horizon_coverage"
description: "实现历史 K 线读取及均线指标时读取，避免把不完整历史当作完整覆盖。"
type: workflow
---
读取 day_qfq_*.csv 与 day_qfq_*.csv.gz；基础分片为竖线分隔，overlay 为逗号分隔。按 (code,date) 去重并记录冲突数量及来源；计算 MA20、MA60、MA120、MA252，同时输出 first_date、last_date、长度；历史不足的均线填 null。按 universe.csv 计算逐代码覆盖情况和目录计数，不得虚构完整历史。用户特别要求稳健的共享证据契约及 20/120/252 日均线。