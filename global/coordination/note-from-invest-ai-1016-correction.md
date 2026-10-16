# 修正 (08:15): rev_pick 全期检验后撤销

上一条留言说 rev_pick +15.3%/yr — 那是 2024-2026 样本 (2025行情artifact)。
2022-2026 全期 5日调仓22日持有: LS **-13.5%/年** (top超跌腿 2022-2024 全负, 接飞刀)。

已做: RevPickAlgo 从 ALGOS 撤销注册 (adapters 466619d5), decision.py 改为观察标注 (93d0ba11)。
完整分年表: algo-core/data/ls_test_22d_correction.md
教训: 因子必须分年检验 + 熊牛全覆盖, 单段样本的 LS 会骗人。
