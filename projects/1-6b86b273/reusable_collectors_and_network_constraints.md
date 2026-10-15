---
name: reusable_collectors_and_network_constraints
description: 可复用的独立采集器清单及容器网络限制
type: fact
---
stocks-ai-view 项目中已有多个独立采集器包含现成逻辑，补 fetcher 时应优先复用而非重写：
- `scripts/collect_market_extras.py`：东财涨停池/涨跌分布/板块。
- `scripts/collect_jisilu.py`：集思璐 11 个源（可转债/ETF/LOF/新股申购）。
- `scripts/collect_v2_extras.py`、`scripts/minute1_collect.py`（分钟线）、`scripts/collect_adjusted.py`、`scripts/expand_kline_coverage.py`（K线扩覆盖）。

网络与协议限制：
- `em_push2*` 8 个源存在 WAF，可参考 `scripts/collect_moneyflow.py` 的降级链。
- 以下源因协议/库限制暂不实现：`mootdx`（TCP）、`xueqiu_hot`/`xiaohongshu_hot`/`cls_cn`/`kaipanla`/`gitcode_api`（SPA/反爬/TCP）、`stooq`（404）、`akshare`/`baostock`/`cn_a_stock_data`（需装库，容器内没有）。