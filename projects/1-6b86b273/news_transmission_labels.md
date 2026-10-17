---
name: "news_transmission_labels"
description: "改消息传导区文案或诱多/洗盘标注时先读"
type: workflow
---
stocks-ai-view 的「消息传导」按用户要求展示：好消息利好谁、坏消息利空谁。诱多/洗盘不能默认全标，只在资金证据与消息方向相反时标注（index.html 副标题：诱多/洗盘只在资金证据相反时标注）。脚本侧用 moneyflow.json 的 top_outflow/top_inflow，按 code 取 main_net_ratio；direct_bull 标的 ratio<0 才记相反证据。