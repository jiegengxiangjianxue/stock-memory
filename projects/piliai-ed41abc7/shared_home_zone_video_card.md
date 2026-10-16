---
name: "shared_home_zone_video_card"
description: "修改首页或分区页的视频网格卡片时，先确认共用组件的影响范围。"
type: fact
---
项目代码注释明确：网格视频卡已从 `HomeScreen.VideoCardItem` 提取为首页与分区页共用组件；分区页原有的 `ZoneCard` 是旧版副本。调整卡片视觉时应检查首页和分区页的效果，避免只改其中一页。