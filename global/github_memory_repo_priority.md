---
name: "github_memory_repo_priority"
description: "整理、合并或优化记忆（以及排查数据缺失）时，优先查阅 GitHub 上的记忆库"
type: workflow
---
用户在要求整理记忆时明确说「多看看 GitHub 的记忆库」。

执行要点：
- 做记忆的合并、去重、清理、优化前，先查 GitHub 上的记忆库（远端仓库），把它作为优先信息源。
- 不要只翻本地记忆目录（如 ~/.aharou 下的 global / projects 结构），远端记忆库与本地要对照着看。
- 遇到「网站没有数据」这类数据问题，同样优先去 GitHub 记忆库与相关仓库找线索。