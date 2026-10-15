# 股票体系记忆仓库 (stock-memory)

供 AI 会话加载的结构化记忆。全部为 JSON, 机器可读。

## 结构
- `entities/` — 账号、token 属主、仓库清单 (不含明文token)
- `data-catalog/` — 数据集目录: 覆盖范围、schema、分片规则
- `decisions/` — 架构决策记录 (ADR)
- `lessons/` — 踩坑记录 (必须遵守)
- `profiles/` — 用户偏好与任务状态

## 使用
分析 AI 先读 `entities/repos.json` + `data-catalog/datasets.json`, 再按需读其他。
