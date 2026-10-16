---
name: overseas_linkage_engine
description: 实现或扩展海外关联性分析前读取：overseas_linkage.py 的字段口径、接入位置、实测相关/贝塔结论
type: fact
---
实现或扩展海外关联性分析前读取：`scripts/overseas_linkage.py` 的字段口径、接入位置、实测相关/贝塔结论。

# 海外关联性框架（2026-10-15 建）
量化 A 股标的受海外影响的程度，覆盖股票↔板块/大盘/全球股市、板块↔大盘、大盘↔全球股市。`overseas_linkage.py` 纯本地计算，读取 `data/global_kline.json` 与 `data/index_kline.json`，按共同交易日输出 `corr`、`beta`、`r2`、`best_lag`、`overnight_corr`、`overnight_beta`、`n`，最少 20 个样本；产出 `data/overseas_linkage.json`，供前端「海外关联」tab（`data-ci="8"`）及 multidim 的「海外面」维度（权重 0.07）使用，由 9 维扩为 10 维。

实测结论（50 对，2026-10-15）：沪深300←恒生科技 corr 0.47/beta 0.35；创业板指←纳斯达克 corr 0.33/beta 0.70/隔夜 0.22；科创50←纳斯达克 corr 0.30/beta 0.83；上证指数←恒生科技 corr 0.44/beta 0.25。高相关既是择时信号也是风险传导通道，不是「越高越好」。

接入：`.github/workflows/engine-chain.yml` tier 2 必须在 `multidim_engine` 之前运行，并���入 `git_push_retry` 文件与 protect 列表；light tier 不加，关联性按日更新即可。

修改海外关联模块时，读取 `window._data.overseas_linkage` 与 `window._data.overseas_impact`，按 `benefit`、`harm`、`mixed` 展示传导候选，并明确标注结果基于相关性、贝塔、滞后和隔夜传导，**不是因果结论**。无关联数据时显示暂无数据，并注明数据由 `overseas_linkage.py` 生成、依赖 `global_kline.json` 和 `index_kline.json`。