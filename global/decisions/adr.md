# ADR
1. 2026-10-15 数据分片采用跨账号 db-daily-0..5 (避开单仓/单号配额), symbol 序号取模
2. 2026-10-15 日K 主力源=新浪(单请求~3700日), 腾讯分段补1999前; 前复权 qfq
3. 2026-10-15 存储 parquet zstd 按年分区 (沿用 db-warehouse data_warehouse.py schema)
4. 2026-10-16 云端回补以 Actions 为主力(约2天补完), 本机 collector_v4 并行兜底
