---
name: verify_after_each_change
description: 每次代码改动后必须执行的验证流程
---
在 ~/workspace/stocks-ai-view 项目中，每步代码改动后须立即运行对应验证脚本（如 `python3 scripts/reflect_engine.py --horizons 1,3,5,10 --out /tmp/reflect_test.json`）确认无 crash，通过后再进入下一步。全部改完执行 `git add + commit`，message 格式参考：`feat: <简述>`。