---
name: "weight_calibrator_v3_floor"
description: "重跑权重校准或看到权重全零时读"
type: fact
---
weight_calibrator_v2 会把权重全部归零。v3（scripts/weight_calibrator_v3.py）改为分层校准，相对权重限制在 0.1–0.5，产出 data/weight_calibration.json，供 engine_v5 / multidim 消费。命令：python3 weight_calibrator_v3.py [--min-n 5000] [--dry-run]。2026-10-17 实跑未归零；bear 样本仅 12.4%（n=35），看空胜率 0% 区间宽，不能当强结论。