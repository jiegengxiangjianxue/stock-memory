---
name: github_actions_azure_egress
description: 何时该读它：当需要从 GitHub Actions 访问外部 API 或评估数据源可达性时
type: fact
---
项目 stocks-ai-view 的 GitHub Actions 出口在海外 Azure。涉及国内商业 API（如 Tushare、聚宽、同花顺等）调用时，需注意可能存在网络延迟、地域访问限制或 IP 风控；必要时优先选择海外可直达的数据源，或评估代理方案。