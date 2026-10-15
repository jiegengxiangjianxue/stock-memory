---
name: reverse_probe_hard_constraints
description: 逆向/接口探测任务的硬性红线：纯只读、礼貌频率、不绕过 WAF
type: identity
---
对数据源/财经接口做逆向/探测前必须遵守：1) 纯只读，仅 HTTP GET/POST，带正确 UA+Referer，间隔≥0.7s，1-2 QPS；2) 禁止安装软件、破解验证码、代理轮换；3) 遇到 WAF 拦截/被墙/420/418/空响应等只记录到 blocked 列表，不要绕过；4) 只写 /tmp/rev_d/result.json（先 mkdir -p），不动 ~/workspace 其他文件；每探明 2-3 个接口立即增量写盘。