---
name: stocks_ai_view_redlines
description: stocks-ai-view 项目的修改红线与提交约定
type: identity
---
在 `~/workspace/stocks-ai-view` 项目中，必须遵守：
- 不要执行 `git commit` / `git push`，由用户自行提交。
- 不要 `pip install` 任何库。
- 只允许修改 `scripts/collect_all_sources.py`（新增 fetcher）和必要时新建 `scripts/fetchers_*.py` 辅助模块。
- 严禁改动 `scripts/sources_registry.py` 的 SOURCES 元数据、`index.html`、任何 workflow 文件、`data/` 下任何文件。
- token/key 一律通过 `os.environ.get("XXX_KEY")` 读取，绝不硬编码。
- 容器内多数国内数据源网络不通，fetcher 能优雅失败（返回空+reason）即为合格，不得为跑通而伪造数据。