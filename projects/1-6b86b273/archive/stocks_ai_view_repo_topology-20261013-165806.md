---
name: stocks_ai_view_repo_topology
description: stocks-ai-view 项目的仓库拓扑、关键路径与 CI 调度参数（在该目录下工作时查阅）
---
- 工作目录: /root/workspace/stocks-ai-view/
- 三个仓库协作关系:
  - stocks-ai-view (当前仓库, 含 server.py / index.html / .github/workflows/)
  - stocks-tools → checkout 到 /tmp/stocks-tools (提供 tools/fetch_all_sources_v2.py, tools/run_all_pools.py)
  - stocks-data-live → checkout 到 /tmp/stocks-data-live (数据目录)
- 数据根目录: STOCKS_DATA="/root/workspace/stocks-data-live/data"
- Flask API 端口: 8080 (server.py)
- CI cron: '*/30 2,3,4,5,6,7,8 * * 1-5' (CST 10:00–16:30, 每 30 min, 工作日), 同时支持 workflow_dispatch
- CI 流水线顺序: checkout 三仓库 → pip install numpy pandas flask → fetch_all_sources_v2.py → export_pools.py → run_all_pools.py → git add/commit/push
- l2_config.json 仅本地保存, 不推 GitHub