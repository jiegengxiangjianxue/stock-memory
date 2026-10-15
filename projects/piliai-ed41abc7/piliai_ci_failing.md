---
name: piliai_ci_failing
description: piliAI 当前 CI 构建持续失败，修代码前先确认 CI 管线状态
---
截至本次对话，main 分支 CI 构建**总是失败**，用户已手动修改过很多文件。操作前先 `gh run list` / `gh run view` 查看最近一次失败日志，定位是 Kotlin 编译、Rust 交叉编译 (aarch64)、还是 Gradle 依赖问题，再动手改。不要在未读 CI 日志的情况下盲目提交。