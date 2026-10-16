# fwd_ret 标签必须用 lead() 不是 lag()

2026-10-16 发现: factor_matrix 的 fwd_ret_1d 误用 lag() 导致标签=当日收益, 与特征 ret_1d 相关 1.0, ML AUC=1.0 假象。

规则: 任何 fwd_* 前瞻标签用 lead(); 训练前必查 corr(特征, 标签), 若 >0.1 或 AUC>0.95 立即排查泄漏。
修正: factors_wide.fwd_ret_1d 已用 lead() 重算 (corr 0.015, 干净 AUC 0.516)。
