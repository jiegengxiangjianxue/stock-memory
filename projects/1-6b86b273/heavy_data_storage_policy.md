---
name: heavy_data_storage_policy
description: 涉及大文件存放、容器镜像、GitCode/GitHub 数据同步前读取，确认重数据不放容器。
type: workflow
---
项目重数据不放进容器持久化，必须放云端仓库。经核算：K 线全历史在独立仓 stocks-data-kline（281MB，.gz 约 80MB），主仓 .git pack 748KB、data/ 130MB，GitHub 单仓软限制约 1GB。决策：K 线全历史继续留在 GitHub；GitCode 只做冷备，不作为算法、引擎或网站的实时数据源。