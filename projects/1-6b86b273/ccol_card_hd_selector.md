---
name: "ccol_card_hd_selector"
description: "改多池/周期 tab 标题 CSS 时读：误伤二级小节标题的选择器"
type: fact
---
2026-10-15 用户反馈已确认：`.c-col .card .hd{display:none}` 会把多池选股、周期深析等 tab 内部二级小节标题一起隐藏，用户看不到「① 风险池」「⑦b 板块资金」等池名。只藏 tab 卡片顶层标题：`.c-col .c-card > .hd{display:none}`。tab 按钮已有文字，藏顶层 .hd 是为了去重。