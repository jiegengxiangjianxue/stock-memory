---
name: data_freshness_root_causes
description: 数据过期五层根因（采集只覆盖盘中/周更引擎/GH Actions停跑/无新鲜度监测/弱市空tier1）与 audit_freshness.py 自查机制；审数据必须逐个算年龄
type: fact
---
# 数据过期根因与新鲜度自查机制 (2026-10-16)

用户反馈「有的功能没有数据, 有的数据没有更新, 从头到尾都显示过时数据, 热点数据也没更新过」。
逐个审完 101 个 data 文件后确认五层根因。

## 根因 1: 采集窗口只覆盖盘中
`scheduler` / `shard-collect` / `full-pipeline` 三个 workflow 的 cron 原本都是
`2,7,...57 1,2,3,5,6,7 * * 1-5` = **只覆盖 CST 09:02-15:57**。
`ai-update` (跑 latest/hot_news) 一天 4 次且全在盘中。
→ 盘后 15:57 到次日 09:02 共 **17 小时采集停摆**, 深夜/凌晨/周末看网站
必然是一整天前的数据。实测 hot_news 停在 10-15 18:17, mainline 停在 10-15 09:02。

修: cron 扩到 UTC 1-12 (CST 09:02-20:57) + UTC 17 (CST 次日01:02),
并去掉 `1-5` 工作日限制 (新闻类不依赖开盘, 周末也该刷)。

## 根因 2: 12 个引擎文件由 nightly-backfill 一周一更
`nightly-backfill` cron = `0 18 * * 6` (CST 周日 02:00), **一周一次**,
却负责 reflect_latest / winrate_matrix / kline_sim / walkforward /
market_phase / scenario / self_reflect / rps_rotation 等。
实测这些文件停在 10-14 19:44, 32.7h 未动。
这些脚本 engine-chain 的 full 档其实都跑, 但 full 档只在工作日
09:05/15:30/16:40 触发。

## 根因 3: GitHub Actions schedule 对本仓静默停跑
实测 scheduler 与 shard-collect 最后运行都是 10-15T10:26Z, 之后 10 小时
无任何记录, 三个 workflow state 均 active。scheduler.yml 注释已记录过
(「跑 9 次后停 2 小时」)。→ **cron 精细定时不可靠**, 需扩展时段 +
workflow_dispatch 兜底 + 自动监测。

## 根因 4: 没有人监测新鲜度 (最根本)
已加 `scripts/audit_freshness.py`:
- 扫全部 data/*.json, 按 ts/date 算年龄, 与期望频率档位比对
- 档位: realtime 0.5h / intraday 3h / daily 30h / weekly 8d / static 无要求
- 产出 data/freshness_audit.json + .md
- 首跑即发现 11 STALE + 18 NO_TS
- shard-collect 每轮采集后自查, 超阈值在 Actions 日志告警
- 前端「每日总结」tab 顶部加新鲜度横幅, 直接列出哪些文件过期、旧多久

用法: `python3 scripts/audit_freshness.py [--fail-on-stale]`

## 根因 5: 弱市下板块池显示空白 (被误认为「没数据」)
10-15 大盘涨429/跌5108 (上涨占比7.7%), 100 个板块里**没有一个**当日涨≥2%
且5日>0 → sector_pool.tier1 空数组, 前端只读 tier1||tier2 所以显示空白。
修: 前端逐级回退 tier1→tier2→tier3 并标注「当日无T1」;
另修 rebuild_pools.py 疏漏: tier3_count=41 有值但 tier3 数组从未输出。

## 教训
**不要只看文件列表前几个就下结论「数据很新」**。101 个文件里有 11 个过期、
18 个无时间戳, 分布在全时间轴 (0.2h 到 90h)。审数据必须逐个算年龄并对照
期望频率, 否则就是把「刚更新的那几个」当成全部。