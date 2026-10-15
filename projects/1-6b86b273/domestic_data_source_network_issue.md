---
name: domestic_data_source_network_issue
description: 容器内国内数据源网络不通时的fetcher处理原则
type: fact
---
`~/workspace/stocks-ai-view` 项目的容器内多数国内数据源网络不通。编写 fetcher 时应优雅失败（返回空+reason），不得为跑通而伪造数据。