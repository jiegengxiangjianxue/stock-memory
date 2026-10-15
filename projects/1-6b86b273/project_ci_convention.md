---
name: project_ci_convention
description: stocks-ai-view 项目 CI 接入方式：full-pipeline.yml 的插入位置与 git add 规则
type: workflow
---
stocks-ai-view 项目新脚本接入 CI 时，编辑 `.github/workflows/full-pipeline.yml`，在 "6f Global FX" 步骤之后插入新步骤，并注意依赖顺序。同时在 commit 步骤的 `git add` 列表中加入新生成的 json 文件。参考 `scripts/decision_engine.py` 的 `--out` argparse 写法。