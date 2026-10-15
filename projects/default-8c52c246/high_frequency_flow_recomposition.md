---
name: high_frequency_flow_recomposition
description: Compose 顶层 collect 高频 StateFlow 导致全量重组的根因与修法
type: fact
---
HomeScreen（约1600行）顶层 collect SbCommandClient.status（1秒发射一次的高频流）会让整个页面每秒全量重组，是首页卡顿的主因。修法：顶层不订阅该高频流；status 仅用于非实时场景（toggleVpn 日志、启动中/错误文案），直接读瞬时值 SbAiVpnService.status.value；实时流量交由 HomeTrafficCard 内部独立 collect。