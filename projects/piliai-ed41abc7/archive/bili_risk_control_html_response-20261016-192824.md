---
name: bili_risk_control_html_response
description: B站接口可能返回风控 HTML 页面，解析 JSON 前需检测避免异常
type: fact
---
调用 B站写操作等接口时，可能返回风控页面（非 JSON，以 `<` 开头）。在 JSON 解析前应先检查 `resp.trimStart().startsWith("<")`，命中时直接报错提示「B站返回风控页面（非 JSON），请稍后重试」，避免 JSONObject 抛出难懂的异常。