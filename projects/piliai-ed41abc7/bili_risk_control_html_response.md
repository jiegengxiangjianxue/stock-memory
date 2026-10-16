---
name: "bili_risk_control_html_response"
description: "B站接口可能返回风控 HTML 页面，解析 JSON 前需检测避免异常"
type: fact
keywords: ["bili", "risk", "control", "html", "response", "B站接口可能返回风控", "HTML", "页面"]
---
B站接口（包括写操作）可能返回风控 HTML 页面而非 JSON，状态码可能为 HTTP 200 或 412，通常以 < 开头。解析 JSON 前应先读取文本并检查 trimStart().startsWith("<")；命中时返回可读失败结果或抛出可识别错误（如“B站返回风控页面（非 JSON），请稍后重试”），交由上层降级处理，避免 decodeFromString/JSONObject 因首字符 < 抛出异常而绕过仅按业务 code 判断的降级链。