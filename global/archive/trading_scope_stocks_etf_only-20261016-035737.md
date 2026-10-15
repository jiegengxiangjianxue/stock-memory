---
name: trading_scope_stocks_etf_only
description: 用户只交易股票和 ETF，不要推荐转债/期权/期货/融资融券类技能
---
用户明确表示「只玩股票和 ETF」，不涉及可转债、期权、期货、融资融券。

含义：
- 推荐/安装股票技能时跳过转债类（如 qmt-bridge-convertible，已按此原则删除，备份在 /tmp/qmt-bridge-convertible-backup）
- 期权/期权链/希腊字母类技能（ibkr-options-assistant、options-flow-analyzer、trade-options 等）不推荐
- qmt-bridge 合集里只考虑股票与 ETF 子技能：qmt-bridge-market-watch、qmt-bridge-technical-signal、qmt-bridge-sector-theme、qmt-bridge-event-calendar、qmt-bridge-trading、qmt-bridge-etf 等
- 分析输出聚焦 A股个股 + ETF；港股/美股数据可用但非主体