---
name: volume_unit_share_vs_lot_trap
description: A股成交量单位陷阱：科创板按股、其余按手，实测相差13倍；跨股/跨板块比较成交量前必须用 scripts/volume_units.py 归一化
type: fact
---
# A股成交量单位陷阱 (2026-10-16 实测确认)

## 陷阱
**科创板 (688/689) 的成交量按「股」, 其余板块按「手」**(1手=100股)。

来源: 借鉴 simonlin1212/a-stock-data (Apache-2.0) V3.10.1 的更正——上游原文
「此前文档全写成手, 照旧文档换算科创板会大 100 倍」。

本项目 bulk K 线实测 (3057 只最后一行):
- sh688 平均 volume = 3,669,953 (75 只)
- sh60x 平均 volume =   275,119 (1361 只)
- sz00x 平均 volume =   349,646 (1217 只)
→ 科创板原始值是主板的 **13 倍**。

## 已落地
`scripts/volume_units.py` — `to_lots(code, v)` / `to_shares(code, v)` /
`is_share_unit(code)` / `amount_of(code, price, v)`。
归一化后 sh688 = 36,700 手, 与主板同量级。

## 什么场景会错 (必须归一化)
- 全市场成交量/成交额 TOP N 排名 → 科创板上榜率被放大 13 倍
- 板块内成交量排名、换手率横向比较
- 任何 `volume * price` 近似成交额
- 把 volume 当成交股数传给下游 (如资金流强度 = 净额/volume)

## 什么场景不会错 (单位在运算中抵消)
- 量比 vol_ratio = 当日量 / 前5日均量 → 同股, 安全
- VWMA / Elder Force / A/D Line → 同股加权, 安全
- 单股时序放量/缩量判断 → 同股, 安全

已核查: ban_prob 的 vol_ratio、indicators_ext 的 vwma/elder_force/ad_line
都是同股运算, **不受影响, 无需修改**。

## 另一个已核查项: 北交所老号段
上游警告 43x/83x/87x 已迁 920xxx 且**老码不报错**(东财研报静默返回0篇,
腾讯行情返回定格僵尸报价, 价差 17%~100%+)。
本项目 bulk 实测: old BJ codes = 0 只, new BJ codes = 0 只 → **无污染, 无需处理**。