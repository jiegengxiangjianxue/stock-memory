---
name: nightly_backfill_workflow
description: 夜间全量回溯工作流的触发机制、执行任务与数据产出；排查网站数据陈旧或未更新时优先检查此工作流
type: workflow
---
GitHub Actions nightly-backfill.yml 每周六 UTC 18:00 (CST 周日 02:00) 自动触发，也支持手动 workflow_dispatch。执行 backfill_predictions 全量回溯、slim_predictions 瘦身、reflect_engine、kline_sim、winrate_matrix 重算胜率、walkforward 过拟合判定，产出 data/predictions/、data/reflect_latest.json、data/kline_sim.json、data/winrate_matrix.json、data/walkforward.json 等。网站展示数据依赖该工作流更新，若大量数据文件 ts 停滞应优先检查此工作流运行状态与产物提交。