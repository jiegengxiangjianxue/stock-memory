---
name: reverse_probe_hard_constraints
description: 逆向/接口探测任务的硬性红线：纯只读、礼貌频率、不绕过 WAF
type: identity
---
对财经接口做逆向/探测时必须遵守：1) 纯只读，仅 HTTP GET/POST，带正确 UA+Referer，间隔≥0.6s，1-2 QPS；2) 禁止安装软件、禁止破解验证码、禁止代理轮换；3) 遇到 WAF 拦截（TLS reset/567/空响应）正常，记录到 blocked 列表，不要绕过；4) 只写指定产出文件并增量落盘，不动 workspace 其他文件。