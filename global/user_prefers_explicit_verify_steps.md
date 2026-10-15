---
name: user_prefers_explicit_verify_steps
description: 给用户出方案时确认是否附验证命令
---
用户习惯在任务描述中明确写出每步的验证命令（如 `python3 scripts/kline_sim.py --out data/kline_sim.json --days 1,3,5`、`python3 -c "import duckdb; ..."`），要求最后 `git add + commit`。回复时保留这些验证步骤，不要省略。