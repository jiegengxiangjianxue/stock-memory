---
name: prediction_bundle_cycle_input
description: 生成预测包时会读取周期分析数据。
type: workflow
---
`scripts/prediction_bundle.py` 的主流程会加载 `data/cycle_deep.json`，并据其 `status` 或 `composite` 字段参与预测包处理；修改预测包逻辑时应保留该输入的读取与兼容缺失文件的行为。