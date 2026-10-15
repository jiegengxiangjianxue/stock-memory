---
name: multi_account_schedule
description: 三账号仓错峰调度实现2分钟级数据粒度的架构、cron配置、部署命令与token纪律
type: identity
---
# 三账号仓交叉调度 (2026-10-14 建成)

## 目的
GitHub Actions `schedule` cron **最短间隔 5 分钟**（官方硬限制，单仓无法 2 分钟）。
三仓错峰合成 2 分钟粒度。

## 仓与 cron
| 标签 | 仓 | cron 分钟位 | mod5 |
|---|---|---|---|
| A | qwerzxcva/stocks-ai-view | 2,7,12,...,57 | 2 |
| B | jiegengxiangjianxue/stocks-ai-view-b | 4,9,14,...,59 | 4 |
| C | dufuwuxin/stocks-ai-view-c | 0,5,10,...,55 | 0 |

任意 2 分钟窗口必有一个仓刚跑完 → 前端读三个 raw 取 ts 最新。

## 账号
- A 的 token 登录名实际是 `dufuwuxin`（qwerzxcva 是组织名）
- B = jiegengxiangjianxue (zoumou159357@gmail.com)
- C = dufuwuxin (741059079@qq.com)
- token 一律 export MIRROR_TOKEN_B / MIRROR_TOKEN_C，**绝不写进代码**（secret scanning 会拦）

## 部署
`export MIRROR_TOKEN_B=ghp_xxx && python3 scripts/deploy_mirror.py --label B`
- 复制代码（排除 data/ 与 .git），改 cron 错峰
- 保留 schedule 的只有 3 个采集 workflow：full-pipeline / realtime-minute / realtime-poll
- 其余 12 个禁用 schedule（AI/复盘/归档/回溯），避免三仓重复烧 LLM 额度、重复归档、commit 冲突
- B/C 只是数据源节点，**不要在 B/C 上手改代码**，会被下次部署覆盖

## 前端
`gh_sources.js` 的 `REPOS` 配了 A/B/C；`index.html` 的 `fetchJSON` 接入多仓降级：
配多仓时取 ts 最新，单仓时走原同源路径（无回归）。
实测 `fetchMulti('data/market_now.json')` → source=A 取到最新。

## 附带解决的隐患
1. **公开仓 schedule 60 天无活动会被 GitHub 自动禁用** — 三仓各有真实提交，互为热源
2. 单点故障：A 挂了 B/C 继续供数

## 已停用
- GitCode Space（30 分钟休眠 + 需人工网页激活，无法自动开关）
- space-pull.yml、full-pipeline 的保活步骤、gitcode-sync 的 Step C 版本戳
- GitCode 只留 4 个 dataset 仓（零核时纯存储）做归档