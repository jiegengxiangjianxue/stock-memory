---
name: sharded_collection_architecture
description: 三仓分片采集策略与 GitHub Actions 配置，用于规避国内金融源对 GitHub IP 的封锁
type: workflow
---
GitHub-hosted runner 出口为 Azure 数据中心 IP，国内金融源（东财 push2/同花顺/雪球）会按 IP 计数封锁。项目采用三账号（A/B/C 仓）分片采集降低单 IP 压力：
- 分片定义见 `scripts/sources_registry.py` 的 `SHARDS`
- A 仓 (qwerzxcva): shard a + e, cron `mod5=2`
- B 仓 (镜像): shard b, cron `mod5=4`
- C 仓 (镜像): shard c, cron `mod5=0`
- d 分片需大陆出口 IP，只在 GitCode Space 跑
部署镜像仓时通过 `scripts/deploy_mirror.py` 修改 `SHARD` 和 cron。