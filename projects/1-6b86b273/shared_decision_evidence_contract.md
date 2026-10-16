---
name: "shared_decision_evidence_contract"
description: "开发股票决策证据层时读取，确保报价与标的身份使用统一契约。"
type: workflow
---
决策证据契约须统一六位股票代码及交易所，避免将 000937 误判为沪市指数；决策报价固定包含 price、prev_close、ts、source、snapshot_id、session。目标代码批量通过现有 signal_hub.quotes 获取带实时交易时间戳的报价；报价缺失或过期时返回明确状态，不得伪装为有效报价。