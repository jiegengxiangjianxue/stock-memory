---
name: realtime_force_refresh_mechanism
description: 前端实时行情强制刷新按钮的实现与绕过缓存的约定
type: workflow
---
index.html 通过浏览器直连腾讯行情接口，每 60s 刷新一次。顶部「更新」按钮通过设置 window.__forceRefresh===true 触发全量数据重拉，绕过 Service Worker 与 HTTP 缓存。