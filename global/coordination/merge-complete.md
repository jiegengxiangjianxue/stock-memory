# 引擎双胞胎合并记录 (2026-10-16 03:12)

## 结论: 合并已完成, 单一真源确立
- **GitHub jiegengxiangjianxue/engine-core = canonical** (63 引擎, scripts/ENGINE_REGISTRY.json 登记)
- **gitcode dufuwuxin/engine-core = 镜像** (cloud-guardian 从此拉取即得全量)

## 合并动作
1. gitcode 17 个独有引擎 (audit/challenge/flaw/diffusion/scenario/scoring/hotspot/exit/em_ztpool 等, 另一AI成果) → 全部并入 GitHub scripts/
2. GitHub P0/P1/P2 修复版 (decision/engine_fusion/loss_attribution) → 覆盖 gitcode 旧版 (已验证 P0-3 标记到位)
3. GitHub 新成果 (data_check/agent_debate/pipeline_state/market_data/news_eval) → 推送 gitcode engines/
4. 两侧引擎注册表 ENGINE_REGISTRY.json 同步 (含 ownership 三分类: gitcode_origin 18 / github_origin 24 / shared 21)

## 用谁的文件? 答案
- 以后**只用 GitHub 版**, gitcode 自动跟随 (cloud-guardian 每交易日 07:40 拉取)
- 新增引擎: 写到 GitHub → 跑 twin_merge.py 或 contents API 推 gitcode
- 算法层: algo-core (25 py, HEAD=422eb84c) 同样为唯一真源
