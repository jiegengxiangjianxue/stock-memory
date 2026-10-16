---
name: "direct_play_query_route"
description: "新增直链播放导航时应使用 query 参数传 URL，避免路径参数被 Navigation 解码后破坏路由匹配。"
type: workflow
---
直链播放路由使用 `directplay?url={url}&title={title}&cover={cover}`，通过 `Uri.encode` 后放入 query 参数；不要把完整 URL 放进路径分段，即使编码 `/`，Navigation 匹配和解码过程仍可能将其恢复为分隔符，导致点击后无法跳转。