---
name: trend_strength_consistency_robust_fix
description: trend_strength.py 中 consistency 维度在 μ≈0 时失效的修复方案
type: fact
---
scripts/trend_strength.py 的 RTSI consistency 原用 1-σ/μ，收益率均值 μ≈0 时导致该维恒为 0（实测 200 只全零，25% 权重白丢）。已验证修复：改用 0.6*方向一致性（与趋势同向的天数占比）+ 0.4*波动收缩，修复后 consistency 分布在 18.9~54.7，为 0 的数量归零。