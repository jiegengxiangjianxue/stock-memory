---
name: fetcher_implementation_conventions
description: collect_all_sources.py 中 fetcher 的标准实现规范
type: workflow
---
在 `scripts/collect_all_sources.py` 中新增 fetcher 时，必须严格遵循以下约定：
- 函数签名：`def f_xxx(ctx) -> (records, meta)`，模仿现有 fetcher（如 `tencent_quote` / `eastmoney_fund_rank` / `sina_news`）的风格，不自创风格。
- 单源失败不抛异常，必须返回 `([], {"reason": "..."})`，保证单点失败不影响整体采集。
- 所有 HTTP 请求超时统一 ≤8s。
- 多源降级：主源失败自动尝试备用源，参考 registry 中的 `degrade_to` 字段。
- 返回值主键约定：quote 组用 `code`，event 组用 `code|theme|date`，fund 组用 `code`，news 组用 `title`，用于合并去重。
- 将新 fetcher 注册到 `FETCHERS` 字典的正确 group（quote/event/sector/news/fund）。
- 验证命令：`python3 scripts/collect_all_sources.py --out /tmp/test.json --groups <group> --workers 4`，最终跑 `--shard a` / `--shard c` 验证。