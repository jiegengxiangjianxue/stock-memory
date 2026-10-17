---
name: "eastmoney_moneyflow_waf_limit"
description: "调整主力资金实时采集任务时读取，避免重复请求触发封锁。"
type: fact
---
`.github/workflows` 中的 `moneyflow-realtime` 工作流仅允许主仓 `qwerzxcva/stocks-ai-view` 运行，镜像仓 B/C 不采集；三仓同时请求东财会使请求量增加三倍并触发 WAF。2026-10-14 实测单仓每两分钟采集已能使东财 push2 成功率降至 0%，因此不能将增加采集频率或仓库数当作稳定性修法。