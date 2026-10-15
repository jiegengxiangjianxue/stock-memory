---
name: duckdb_layer_facts
description: stocks-ai-view 的 DuckDB 库现状与 db_layer.py 索引/并发/双库结论，再做 DB 优化或查询前读
---
# DuckDB 库层已核实事实（2026-10-13 分析）

- 库文件：`~/workspace/stocks-data-live/stocks.duckdb`，5.8M，DuckDB 1.5.6。
- 数据量极少（新库）：quotes 6018 行（仅 2026-10-13 一天）、events 213 行（仅 10-12）、kline_daily 0 行、pools 0 行、margin 30 行、sources_log 3 行。
- `db_layer.py --bench` 在 6K 行下全部 1~8ms，无生产规模参考价值。
- 索引（duckdb_indexes 实测存在 8 个）：idx_quotes_date(trade_date) 被 idx_quotes_code_date(code,trade_date) 前缀覆盖，属冗余写放大；排序列（chg_pct/pe/pb/deal_amt）无覆盖索引。
- 分区脱节：db_layer.py 顶部注释宣称 quotes/events/pools 按日期分区，但 SCHEMA 实际无任何 PARTITION BY。
- 并发：connect() 每次新建连接，无连接池/锁/WAL，多 --load 写者靠文件锁串行。
- collect_market_extras.py 是零依赖 HTTP 采集器，完全不碰 DuckDB（grep 无 SQL）；其平铺输出结构与 load_from_collect 期望的 d[data][quote/event] 不匹配，中间映射层未核实。
- 双库隐患：tools-repo 下另有 /home/agentuser/stockdb/stock.duckdb（minute 分钟线库），与本项目库不是同一份。
- 完整报告：/tmp/db_optimization_report.md（/tmp 易失，重要结论以本记忆为准）。

# 扩展与操作原则

- 加法式原则：修改 db_layer.py 或扩展表/索引时，只增表/列/索引，不删已有结构。
- 双库问题（tools-repo stock.duckdb）：只报告，不修。
- 已知 gap：K 线表覆盖率 238/4786（约 5%）。

# 知识星球 MCP (2026-10-13 接入)

- 端点 https://mcp.zsxq.com/topic/mcp?api_key=<key>, Aharou 已配项目级 MCP 名 zsxq (下次会话生效)
- 21 个工具 (call_zsxq_api/get_group_topics/get_hashtag_topics/search_topics/create_topic...)
- 账号 桔梗想见雪 user_id=18142221114442; 星球「行业咨询智库」group_id=51111514812824
- **卡点**: 星球未开通 Skill 权限 → 主题内容全被拒 ("该星球暂未开通 Skill 权限"), 需联系星主
- 可用: get_self_info / get_user_groups / get_group_hashtags (100个标签, 金融相关21个, 总量15000+篇)
- 已备好 scripts/zsxq_collector.py (--check/--collect), 权限一通即可采集; docs/MCP_ZSXQ.md 有完整方案

# GitHub 私有仓情报 (PAT 权限内 28 仓, 2026-10-13)

- **jiegengxiangjianxue1/stockdb** (私有76MB): stock.duckdb daily 207万行/3175只/2024-01~2026-10-09 + minute + picks.duckdb对账。已用 scripts/integrate_stockdb.py 导出1233只新股(79.5万行)到 bulk, 覆盖51%→75%
- **K线覆盖真相 (2026-10-14 实测, 推翻了上面的75%)**: bulk 实际只有 **3997只**、**129万行**、66.4MB; 每只中位数仅 **121根**(约半年); **2544只(64%)最早日期在2026年**; 覆盖不足90%的有 2947只(74%); 缺口约 **37万根**。之前的643快照/6万reflect样本/46 walkforward folds 全部基于这个短历史, 所有胜率结论都隐含此前提
- **jiegengxiangjianxue1/stocks-algo** (私有486MB): 算法库。实测统计: 晋级率首板10.6%/2板24.2%/3板45.2%/4板49.6%/5板+63.9%; 次日均收益0.42/1.18/2.19/1.95/1.92%; B规则(240日位<35%+量比0.8-2+涨0-5%)5日胜率55.5% vs 基准48.8%。已移植 ai_pick_v4.py
- 其他私有: jiegengxiangjianxue1/stocks, dufuwuxin/engine-core, stocks-ops, d2-social, d3-global, jiegengxiangjianxue3/optimization-lab
- **安全教训 (踩过3次!)**: token 绝不能写进代码! GitHub secret scanning 会拦 push (2026-10-14 deploy_mirror.py 又踩一次)。一律 os.environ.get, 部署脚本靠 export 传入

# GitCode 平台事实 (2026-10-13 实测)

- 仓库: dufuwuxin/stock-system (公开), token A_Ud4NyTDAsxcBJSqDyrJ2me (用户 dufuwuxin, 邮箱 3576982374@qq.com)
- **有**: Git 推送, Contents API (/api/v5/repos/{o}/{r}/contents/{path}, 返回 base64), /api/v5/user, /user/repos
- **无**: Actions API (404), Pages API (404), rate_limit (404), ai/quota (404)
- **AI 额度可用**: POST https://gitcode.com/api/v5/chat/completions, Bearer token, model=deepseek-ai/DeepSeek-V4-Flash (备 Qwen/Qwen3-30B-A3B-Instruct-2507)。已实测生成487字复盘
- **云容器**: 华为云容器版, 需网页授权一次 (gitcode.com/dufuwuxin/stock-system → 云开发), 可 crontab 常驻, 详见仓内 CLOUD_SETUP.md
- 同步工具: scripts/sync_gitcode.py --status/--pull-engines/--push-data; Actions 用 secrets.GC_TOKEN
- 双向已打通: GitHub scripts/(94个) ↔ GitCode engines/+algorithms/(35个); data/ 镜像 39 个 json

# GitCode Space 集群与数据集 (2026-10-14 建成)

# GitCode Space 集群与数据集 (2026-10-14 建成)

**技术指标的 alpha 在「回避侧」而非「选多侧」**
- 看空侧显著有效 (Wilson CI下限>50%): KDJ 73.6% / HullMA 77.0% / A/D Line 74.8% / Pivot 72.8% / MACD 67.7% / SuperTrend 67.2% (n=61~243)
- 看多侧 0 个指标有效 (17个里最高 ElderForce 64.3 也未过 CI 显著)
- 唯一看多侧区分维度: 资金面 (TOP票均分70)
→ 策略: 技术共振应降级为「回避过滤器」(指标翻空就排除), 而非「买入信号」
→ 这解释了为什么 1日看多胜率只有 40-43% (低于随机): 用同向指标做多本质是反向的

# GitCode Space 集群与数据集 (2026-10-14 建成)


# GitCode Space 集群与数据集 (2026-10-14 建成)

- 组织: jiegengxiangjianxue (id 14856065, type=Group)
- Space 创建 API: `POST /api/v5/orgs/{org}/repos` + `repository_type=space` + `license_template`(必填); **`/user/repos` 的 owner 参数无效, 只会建到个人空间**
- Space 网页入口: https://ai.gitcode.com/space/create (不在组织页新建仓库里, 用户找不到的原因)
- 已建 5 仓: stocks-ai-space(Space) + 4 个 dataset (kline-daily/news-archive/valuation-daily/prediction-log)
- 数据集: `repository_type=dataset`, 自带 Git LFS 规则, **零核时仅存储**
- 核时: 按 CPU 核×小时; 组织与个人额度独立统计; 2核常驻单组织需1440/月(超1000), 需 3 组织分摊
- Space 休眠: 默认 30 分钟无 HTTP 访问; 解法 = GitHub Actions 每 20 分钟 curl /health 保活
- 数据流: GitHub Actions 采集 → gitcode-sync 推 Space → Space git pull 跑引擎 → analysis_out/ → space-pull 拉回覆盖 GitHub data/
- 已推日K parquet 24MB (207万行/3175只) 到 kline-daily

# 采集侧数据质量问题 (2026-10-14 全链实跑发现, 未修)

- `all_sources.json` quote detail[54].price = 116050 (东财涨停池 p 单位是厘)。源码 collect_market_extras.fetch_pool 阈值已从 100 改 1000, **但旧数据文件仍在库里**, 下次采集刷新才会好
- `realtime_minute.json` quotes[0].chg_pct = 223.06%。源码已加 ±35% clamp, 同样是旧数据残留
- 教训: 修源码不等于修数据; 数据文件要靠重新采集或写一次性补丁刷新
- `loss_attribution` 样本不足是因为 picks.duckdb 缺失 (jiegengxiangjianxue1/stockdb 私有仓有), 非 bug

# 复盘闭环与 K 线覆盖 (2026-10-13)

- **预测持久化**：data/predictions/engine_v5_YYYY-MM-DD.json，CI full-pipeline 每周期存一份；reflect_engine `--pred-dir data/predictions` 读全部历史快照做前向验证。3/5/10 日胜率样本随时间逐日累积（<30 标 insufficient）。
- **kline_sim.py**：`--source predictions` 读历史预测回测；当天信号无法回测（前视防护，属正常）。
- **expand_kline_coverage.py**：腾讯+东财双源 8 并发扩 K 线，覆盖从 4.5%→51%（2737/5343），增量写 day_qfq_expand.csv，与 kline_sim 的 load_klines 格式兼容。
- **CI 闭环**：full-pipeline 5b 存预测快照 → 6 复盘 → 6b kline_sim+winrate_matrix → commit 到 GitHub；前端 fetchJSON 可从 raw.githubusercontent.com 兜底读。