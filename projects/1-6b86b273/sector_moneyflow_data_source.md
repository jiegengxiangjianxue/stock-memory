---
name: sector_moneyflow_data_source
description: 板块主力净额数据源选型与腾讯接口的字段限制
type: fact
---
腾讯行业板块排行 `mktHs/rank` 只返回涨跌幅（`bd_zdf`/`bd_zdf5`/`bd_zdf20`），不含主力净额。板块资金流需单独走东财 `push2.eastmoney.com/api/qt/clist/get`，用 `m=90`（行业板块）+ `fid=f_flow`（按主力净额倒序）拉取；字段映射为 `f_flow`→主力净额、`f_come`→净占比等。