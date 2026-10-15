---
name: eastmoney_price_unit_centi
description: 东财接口价格字段的单位换算与阈值判定
type: fact
---
东财接口价格单位为「厘」，如 116050 表示 116.05 元；判定规则为正常股价 <1000 元，故 pval>1000 时才除以 1000。