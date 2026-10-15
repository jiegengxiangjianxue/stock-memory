---
name: stock_data_backfill_requirement
description: 项目需要补齐仓库内所有股票数据至上市至今
---
本项目要求将仓库中所有股票数据补齐，时间范围覆盖每只股票上市至今。使用不少于83个数据源进行获取。

## 2026-10-16 缺口诊断与补齐进展

**重大缺口**：universe 6029 只 vs 远端 db-stocks-a~e 实际 4084 只，**缺 1945 只 (32%)**。
缺失集中在 4 个代码段（各 shard 收录上限之后的新股/后段代码）：
`00` 644 只 (002373~003816) / `30` 453 只 (301024~302132) /
`60` 846 只 (601567~605599) / `39` 2 只 (399001~399006)。

**K线源 tencent_newfqkline 六个坑**：
1. `start` 参数被忽略，只返回 `end` 往前 640 根（`end` 真实生效）
2. 640 是硬上限（n=5000 返回 0 条）
3. code 要 `sh`/`sz` 前缀，不是东财 `1.`/`0.` secid（param error）
4. 返回是 `var={json}` 不是 JSONP
5. 个股键 `qfqday`，指数键 `day`
6. 历史深段有负价格/极低价（前复权对重组更名前老代码失真，如 601567 三星电气时代 -0.85），
   已加过滤：开高低收任一 ≤0 丢弃

**三大报表源**：THS `basic.10jqka.com.cn/api/stock/finance/{code}_{benefit,debt,cash}.json`（47/80/72 指标）。

**CI 坑**：`universe.json` 1.4MB 超 GitHub contents API 的 1MB 上限，`content` 返回空，
base64 解码得空串导致 json.loads 失败。解法：用 API 的 `download_url`（raw 端点）读。

**工具**：`scripts/backfill_missing_stocks.py` + `.github/workflows/stock-backfill.yml`
（4 个并行 job 按 segment 00/30/60/rest，直接推 5 个远端仓）。

**数据源总数已达 184 个**（远超 83 要求）。