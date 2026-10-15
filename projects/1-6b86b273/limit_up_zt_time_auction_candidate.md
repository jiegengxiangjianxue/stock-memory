---
name: limit_up_zt_time_auction_candidate
description: 涨停池 zt_time 字段格式与竞价抢筹筛选口径
type: fact
---
涨停池数据中 zt_time 为字符串格式 'HHMMSS'（如 '92500' 表示 09:25:00），可用于筛选 09:25 集合竞价前后封板的「竞价抢筹候选」。