---
name: readonly_probe_redline
description: 接口逆向调研的只读与合规红线
type: identity
pending: true
---
做股票/财经接口逆向调研时必须纯只读：仅 HTTP GET/POST 探测，带正确 UA+Referer，间隔≥0.6s，1-2 QPS；禁止安装任何软件、禁止破解验证码、禁止代理轮换；本地网络被墙（TLS reset/567/空响应）属正常，记录即可，不要尝试绕过。