---
name: render_pools_data_ready_guard
description: 前端 renderPools 渲染前的数据就绪守卫逻辑
type: fact
---
renderPools() 在数据未就绪(!SR || !Object.keys(g).length)时必须提前 return，避免渲染出「暂无数据」占位；且 plRendered 一旦置位后数据到达不会触发重画，应保持在加载态，由 render() 末尾按 plRendered 补画。