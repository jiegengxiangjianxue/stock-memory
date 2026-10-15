---
name: multi_bot_push_overwrite_chain
description: 多 bot 并发推 main 导致数据不更新的 6 层根因与修法，含 git_push_retry.py 的设计与验证方法教训
---
# 多 bot 并发推 main 的覆盖链（2026-10-14 全链路修复）

仓库有 6 个 bot 同时推 main（pipeline-bot/hot-bot/brief-bot/pool-bot/minute-bot/cloud-bot），
导致数据「工作流全 success 但线上不更新」。共 6 层原因，全部已修：

1. **cron 静默停跑**：80+ cron 触发点/小时，GitHub scheduler 丢触发。
   修：`scheduler.yml` 单 cron 点 + workflow_dispatch 级联唤醒 6 个下游。
2. **硬编码本地路径**：`realtime_minute.py` 的 `AI_VIEW` 默认 `/root/workspace/...`，
   runner 上不存在 → 什么都没写 → commit 步骤比对旧文件「无变化」→假 success。
3. **东财软限流**：返回 200 但 diff 空，或直接断开连接。QPS=10 必踩。
   修：QPS=2 + pz=200 + 单页重试 + push2delay→push2his→新浪三级降级。
4. **旧副本覆盖新版**：各 run checkout 时间点不同。
   修：push 前 `git checkout origin/main -- <paths>` 取回远端最新版。
5. **git commit 提交整个 index**：`git add` 是增量但 `git commit` 提交全量，
   checkout 写进 index 的路径被一并带走。
   修：`git commit -- <paths>` 限定范围；未生成的路径要过滤（pathspec 报错）。
6. **protect 保住未改动的旧副本**：protect 文件若与 run 基线（reset 前的 HEAD）
   逐字节相同，说明本轮没改过，不该当成果保护。
   修：`base_ref` 比对（必须在 `git reset --soft` 之前捕获 HEAD）；
   另：rebuild-pools 的 `--merge` 必须先取远端最新 latest.json 再合并。

关键工具：`scripts/git_push_retry.py`
（fetch → 捕获 base_ref → reset --soft → refresh_from_remote → add → commit -- → push，
5 次退避重试 5/10/20/40s）。

## 验证方法教训

- `bash -n` **查不出** bash 续行断裂（`--user: command not found`），
  必须用「只接受已知 flag 的假命令」实跑才能抓到。YAML 合法同样不代表 run 块正确。
- 检查数据新鲜度要按「哪个工作流负责哪个文件」分别验，不能只看总数。
- 容器出口到东财的限流是**状态性**的：刚做完压力测试就会被完全断连，
  此时新浪兜底仍可用（5573 只 > 3000 健康门槛）。

## 测试

`/tmp/test_protect.py`、`test_protect_dir.py`、`test_protect_stale.py`、
`test_commit_only.py`、`test_commit_filter.py` —— 5 个隔离 git 仓测试，全 PASS。
新增逻辑时必须配对应的隔离仓测试，只跑 `bash -n` 不够。