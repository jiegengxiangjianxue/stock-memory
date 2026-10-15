---
name: a_stock_project_cloud_constraints
description: A股数据分析项目的硬性运行约束，评估任何方案前必读
type: workflow
---
项目部署于 GitHub Actions / GitCode Space 云端，不能跑常驻服务，不能暴露 token，要求零运维，优先纯 Python。重量级微服务、需长驻进程/密钥托管/Docker 常驻容器的架构均不适用；可借鉴的思想必须轻量化到无服务器批处理模式。