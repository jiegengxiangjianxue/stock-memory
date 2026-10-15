---
name: stock_data_live_probe_workflow
description: A股数据采集项目的域认领、节流与落盘约束
type: workflow
---
项目有域认领 + 探测节流机制。域认领表 `/root/workspace/stocks-data-live/data/research/domain_lease.json`，节流日志 `/root/workspace/stocks-data-live/data/research/probe_log.json`。已完成 14 个域和 68 个已实测源，禁止重复。限频：每源 1-2 QPS，用 `scripts/probe_throttle.py --check <id>` 去重（退出码 1 跳过）。不做 CAPTCHA/指纹/代理池。产出只写 `/root/workspace/stocks-data-live/data/research/` 下指定 JSON 与 `/tmp/lv2/`，不要动 `scripts/`。每探明 2-3 个接口必须立即落盘，防止进程被杀导致成果丢失。