---
name: "external_repo_research_convention"
description: "需要调研/借鉴外部 GitHub 仓库时的目录落位、License 判定与报告合并规范"
type: workflow
---
外部仓库借鉴调研的固定做法（以 stocks-ai-view 为例）：

- **代码落位**：用 `scripts/clone_refs.sh` 把借鉴仓库浅克隆到 `~/workspace/refs/<owner>__<repo>/`，不污染项目主目录。
- **License 先判定**：报告开头必列 License 总览表，明确哪些仓库的代码可合法复制（如 Apache-2.0、MIT 可借鉴），License 不明确的只做思路参考不抄代码。
- **报告合并去重**：`reports/` 下同类记录要合并成一份汇总（如「外部仓库调研与落地汇总.md」），按「已借鉴并落地 / 未调研缺口」分段，逐条写清借鉴了什么、落地到哪个脚本，避免重复调研。
- 调研结论落到 `docs/`（架构、数据存储、镜像节点等设计文档）与 `scripts/`（约 161 个脚本）两个位置。