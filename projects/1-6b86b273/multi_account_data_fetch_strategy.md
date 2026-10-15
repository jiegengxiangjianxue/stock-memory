---
name: multi_account_data_fetch_strategy
description: 使用多平台多账号并行获取数据但避免重复抓取相同数据
type: workflow
---
为应对单平台GitHub运行限制，采用两个平台（GitHub + GitCode）、三个GitHub账号和一个GitCode账号并行获取数据。关键约束：各账号/平台不要同时获取相同的数据，以分散请求压力避免触发限制。