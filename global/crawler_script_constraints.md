---
name: crawler_script_constraints
description: 写爬虫/数据采集 Python 脚本时查阅通用技术约束
---
用户明确要求此类脚本满足：
- **零依赖**：只用 Python 标准库（`urllib.request`、`json`、`time`、`sys`），不引入 requests 等第三方包。
- **UA 模拟浏览器**：请求头带 `User-Agent`（如 `Mozilla/5.0 ...`）。
- **限流**：每个目标（股票/吧）之间 `time.sleep(0.5)`。
- **故障隔离**：单个目标抓取失败用 `try/except` 捕获并记录，不中断其余目标。
- 命令行入口形如 `python3 script.py 600519,000858,300750 [输出目录]`，逗号分隔多个目标。