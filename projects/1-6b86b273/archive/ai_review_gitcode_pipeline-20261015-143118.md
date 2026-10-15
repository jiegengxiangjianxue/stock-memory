---
name: ai_review_gitcode_pipeline
description: 实现或修改 AI 复盘前读取：GitCode AI 端点/模型、接入 workflow 的位置、11 类输入、数据读取坑与 prompt 纪律
type: fact
---
# AI 复盘 (GitCode 免费 AI) 接入方式

## 脚本与端点
`scripts/gitcode_ai_review.py`
- 端点 `https://gitcode.com/api/v5/chat/completions`, Bearer token
- 模型: `deepseek-ai/DeepSeek-V4-Flash` (主) / `Qwen/Qwen3-30B-A3B-Instruct-2507` (备, 主失败自动切)
- token 从环境变量 `GITCODE_TOKEN` 读, **不硬编码**
- 产出 `data/ai_review.json` + `data/ai_review.md` (md 含数据上下文 collapsible)

## 接入位置
`.github/workflows/engine-chain.yml` 的 **tier 8 最后一步** (close_review /
premarket / midday 都跑完之后), 这样十维/海外关联/推股/收盘复盘全部就绪。
env 需配 `GITCODE_TOKEN: ${{ secrets.GITCODE_TOKEN }}` (顶层 + job 级都加了)。
**需要在 GitHub 仓库 Settings → Secrets 里添加 GITCODE_TOKEN, 否则脚本自动跳过。**
推送列表要含 `data/ai_review.json data/ai_review.md` + 对应 `--protect`。

## 喂给 LLM 的 11 类输入 (全部本地 JSON, 不发额外请求)
盘面(market_now) / 十维评分含海外面(multidim) / 决策引擎(decision) /
海外关联性 corr+beta+滞后+隔夜(overseas_linkage) / 全球六大市场(global_markets) /
板块资金(sector_moneyflow) / 今日推股闸门+ABC级+入场止损目标(premarket_picks) /
收盘复盘(close_review) / 历史胜率带CI(reflect_latest) / 指标有效性排名(reflect_latest.analysis.ranked) /
引擎自我反思(self_reflect) / 24h资讯(news_digest)

## 两个数据读取坑 (已修)
1. `reflect_latest.json` 的 `horizons` 只是持有天数列表 `[1,3,5,10]`, **不是胜率数据**。
   真实胜率在 `analysis.overall = {"1":{bull_win_rate, bull_win_rate_ci, avg_ret_pct, n}, "3":{...}}`。
   指标有效性在 `analysis.ranked` (list of {indicator, quality, bull_win_rate, ...})。
2. 推股表部分记录 `name` 为空, 用 `search_index.json` 兜底时必须**先试 sz 再试 sh**:
   `000937` 在 sz 是冀中能源(股价 4.45), 在 sh 是指数「800公用」(2560.53),
   顺序反了会把个股显示成指数名。valuation 取值同理 (sh/sz 前缀决定含义)。

## system prompt 的硬性纪律 (用户要求)
- 每个判断必须引用给定数据, 没提的写「数据未覆盖」
- 情绪面/高低位**必须双向解读** (低不一定是坏=可能是冰点反转, 高不一定是好=可能是派发区), 禁止单向断言高分即优
- 海外关联必须用 corr/beta/滞后/隔夜四个量说话, 并指出高 beta 下跌时同样被放大
- 复盘昨日推股逐条对照 verdict, 不回避亏损
- 历史胜率低于随机(50%)时, 明确指出模型信号失效、建议降低权重
- 弱市 (gate=defensive) 仍要给候选名单, 但标注逆势风险与轻仓要求
- 结构固定 7 段: ①盘面 ②主线题材 ③海外传导 ④昨日推股验证 ⑤明日关注与风险 ⑥策略建议 ⑦算法自我反思; ≤700 字

## 前端
`renderAiReview()` 返回 HTML 字符串 (不是写 DOM), 由 renderDailySummary 的⑦节消费;
`md2html()` 处理 Markdown 的 ## 标题 / **粗体** / 列表。显示模型名 + 上下文字数。