---
name: render_tencent_safe_wrap
description: 当周期深析、资金背离等懒渲染 tab 显示无数据/加载中时，先排查 renderTencent 是否未包裹
type: fact
---
在 stocks-ai-view 项目 index.html 中，renderTencent(d) 未使用 _safe 包裹时，一旦该函数内部抛错，会中断其后所有按 tab 懒渲染的逻辑（cycle_deep / flow_divergence / decision / scenario / moneyflow），表现为「周期深析停在加载中」「资金背离暂无数据」。已验证修法：将裸调用 renderTencent(d) 改为 _safe(()=>renderTencent(d),'tencent')。