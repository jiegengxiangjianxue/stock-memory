---
name: open_source_adaptation_workflow
description: 借鉴外部开源算法或架构时的合规独立实现流程
type: workflow
---
在 stocks-ai-view 项目中借鉴外部仓库（如 hengruiyun/AI-Stock-Master、cloudQuant/backtrader）时，若其为 AGPL-3.0/GPL-3.0，不复制代码；仅借鉴公开的数学模型、参数定义或架构设计（如 Analyzer 插件框架），然后按本项目数据独立实现。需在代码头部注明来源链接与许可证，并明确声明独立实现、非复刻。