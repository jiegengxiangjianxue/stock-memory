---
name: "data_freshness_root_causes"
description: "数据过期五层根因（采集只覆盖盘中/周更引擎/GH Actions停跑/无新鲜度监测/弱市空tier1）与 audit_freshness.py 自查机制；审数据必须逐个算年龄"
type: fact
keywords: ["data", "freshness", "root", "causes", "数据过期五层根因", "采集只覆盖盘中", "周更引擎", "GH"]
---
# 数据过期根因与新鲜度自查机制 (2026-10-16)

用户反馈「有的功能没有数据, 有的数据没有更新, 从头到尾都显示过时数据, 热点数据也没更新过」。逐个审完 101 个 data 文件后确认五层根因。

## 根因 1: 采集窗口只覆盖盘中
原 `scheduler` / `shard-collect` / `full-pipeline` 的 cron 主要覆盖交易日盘中时段，导致盘后、晚间和凌晨约 17 小时采集停摆；`hot_news`、`news_extra`、`hot_themes` 等下游不运行，24h 新闻、实时热点和热点主题可能停在前一天盘中。实测 hot_news 停在 10-15 18:17, mainline 停在 10-15 09:02。

修: 在 `.github/workflows/scheduler.yml` 补齐盘后、晚间和凌晨的 UTC 时段，由心跳继续级联下游；新闻类任务不应只依赖开盘，也应覆盖周末。相关 workflow 的 cron 可扩到 UTC 1-12 (CST 09:02-20:57) + UTC 17 (CST 次日 01:02)，并去掉 `1-5` 工作日限制。`full-pipeline.yml` 的 24h 新闻流由 `scripts/build_news_digest.py --keep-hours 24` 生成 `news_digest.json`。

## 根因 2: 12 个引擎文件由 nightly-backfill 一周一更
`nightly-backfill` cron = `0 18 * * 6` (CST 周日 02:00), 一周一次，却负责 reflect_latest / winrate_matrix / kline_sim / walkforward / market_phase / scenario / self_reflect / rps_rotation 等。实测这些文件停在 10-14 19:44, 32.7h 未动。这些脚本 engine-chain 的 full 档其实都跑, 但 full 档只在工作日 09:05/15:30/16:40 触发。

## 根因 3: GitHub Actions schedule 对本仓静默停跑
实测 scheduler 与 shard-collect 最后运行都是 10-15T10:26Z, 之后 10 小时无任何记录, 三个 workflow state 均 active。scheduler.yml 注释已记录过「跑 9 次后停 2 小时」。cron 精细定时不可靠，需扩展时段、workflow_dispatch 兜底与自动监测。

## 根因 4: 没有人监测新鲜度 (最根本)
已加 `scripts/audit_freshness.py`:
- 扫全部 data/*.json, 按 ts/date 算年龄, 与期望频率档位比对
- 档位: realtime 0.5h / intraday 3h / daily 30h / weekly 8d / static 无要求
- 产出 data/freshness_audit.json + .md
- 首跑即发现 11 STALE + 18 NO_TS
- shard-collect 每轮采集后自查, 超阈值在 Actions 日志告警
- 前端「每日总结」tab 顶部加新鲜度横幅, 直接列出哪些文件过期、旧多久

用法: `python3 scripts/audit_freshness.py [--fail-on-stale]`

## 根因 5: 弱市下板块池显示空白
弱市下板块池可能没有 tier1，被误认为没数据。审数据必须逐个计算年龄，区分采集停摆、引擎低频更新、Actions 停跑、缺少新鲜度监测和弱市空 tier1。