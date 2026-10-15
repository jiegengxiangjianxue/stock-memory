---
name: stocks_ai_view_repo_topology
description: stocks-ai-view 项目的仓库拓扑、关键路径与 CI 调度参数（在该目录下工作时查阅）
---
stocks-ai-view 项目结构、仓库拓扑与 CI 参数
- 工作目录: /root/workspace/stocks-ai-view/
- 内部布局: 脚本在 scripts/，数据/输出在 data/（multi_v4.json, ai_picks.json, kline_sim.json, winrate_matrix.json, reflect_latest.json）
- 三仓库协作: stocks-ai-view(server.py, index.html, .github/workflows/, scripts/, data/)、stocks-tools→/tmp/stocks-tools(fetch_all_sources_v2.py, run_all_pools.py)、stocks-data-live→/tmp/stocks-data-live
- 数据根目录: STOCKS_DATA="/root/workspace/stocks-data-live/data"
- K线 bulk 数据: ~/workspace/stocks-data-kline/data/bulk/
- 数据库: DuckDB，表 quotes(code, trade_date, chg_pct, pe, pb, ...)、events(type, trade_date, deal_amt, ...)
- 市场阶段(regime): 从 data/reflect_latest.json 读取
- Flask API 端口: 8080
- CI cron: '*/30 2,3,4,5,6,7,8 * * 1-5'（CST 10:00–16:30，工作日），支持 workflow_dispatch
- CI 流水线: checkout 三仓库 → pip install numpy pandas flask → fetch_all_sources_v2.py → export_pools.py → run_all_pools.py → git add/commit/push
- l2_config.json 仅本地保存，不推 GitHub