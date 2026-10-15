---
name: overseas_linkage_engine
description: 实现或扩展海外关联性分析前读取：overseas_linkage.py 的字段口径、接入位置、实测相关/贝塔结论
type: fact
---
# 海外关联性框架 (2026-10-15 建)

## 用户诉求
不只是展示「海外涨跌」, 而是量化 **A股标的受海外影响的程度**:
股票↔板块、股票↔大盘、股票↔全球股市、板块↔大盘、大盘↔全球股市
(大A 与 韩股/美股/日股/欧股)。用户明确说「这是让你构建海外实时数据的原因,
但现在还没建起来」。

## 实现
`scripts/overseas_linkage.py` — 纯本地计算, 不发请求。
输入: `data/global_kline.json` (symbols[sym].bars=[[date,o,c,h,l,v]])
      `data/index_kline.json` (indices[sym]=[{date,close}])
对每对 (target, driver) 按共同交易日对齐后输出:
  corr    日收益皮尔逊相关
  beta    target = alpha + beta*driver 的回归斜率 (弹性)
  r2      回归解释力
  best_lag 使 |corr| 最大的滞后天数 (-2..+2; +1 = driver 涨一天后 target 才跟)
  overnight_corr / overnight_beta  driver 当日涨跌 → target 次日涨跌 (跨市场传导)
  n       共同样本数 (最少 20, 不足跳过)

产出 `data/overseas_linkage.json`, 前端「海外关联」tab (data-ci="8") 展示,
multidim 的「海外面」维度 (权重 0.07) 消费它 — 由 9 维扩为 10 维。

## 实测结论 (50 对, 2026-10-15)
- 沪深300 ← 恒生科技 corr 0.47 / beta 0.35 (最强)
- 创业板指 ← 纳斯达克 corr 0.33 / beta 0.70 / 隔夜 0.22 (科技成长跟美股最紧)
- 科创50  ← 纳斯达克 corr 0.30 / beta 0.83 (弹性最高, 跌时也被放大)
- 上证指数 ← 恒生科技 corr 0.44 / beta 0.25

## 接入位置
- `.github/workflows/engine-chain.yml` tier 2, **必须在 multidim_engine 之前跑**
  (multidim 的「海外面」读它), 并加入 git_push_retry 的文件与 protect 列表
- light tier 不加: 关联性是慢变数据, 日频足够

## 显示约定
每行给双向解读: 高相关既是择时信号也是风险传导通道, 不是「越高越好」。