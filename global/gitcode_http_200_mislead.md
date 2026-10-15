---
name: gitcode_http_200_mislead
description: 在 GitCode/AtomGit 上勿仅用 curl HTTP 200 判断页面可访问，未登录或无权限页也可能返回 200
type: fact
---
GitCode/AtomGit 对未登录或无权限页面可能返回 HTTP 200，例如 `https://gitcode.com/-/user_settings/personal_access_tokens` 用 curl 检测为 200，但浏览器实际显示"页面找不到或无权限"。验证页面真实可访问性时必须用浏览器加载并检查页面文本，不能只依赖 HTTP 状态码。