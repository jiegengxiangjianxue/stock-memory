---
name: "evidence_based_pool_and_flow_audit"
description: "维护资金背离与池审计逻辑时读取此约定，确保输出可解释且不把缺失数据误判为有效信号。"
type: workflow
---
资金流与 K 线必须按交易日对齐，并输出数据覆盖范围与拒绝原因；缺失资金源明确标记为 `insufficient`，不得把单独的波动收敛判为资金背离。`pool_audit` 需依据真实产物纠正 `cycle_pool_v2` 的 `meta.ts` 与业务字段，正确对齐 risk/hot 等池，区分空池和过期，并按交易时段与非交易时段判断时效。