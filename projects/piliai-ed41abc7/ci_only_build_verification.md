---
name: ci_only_build_verification
description: 当需要在 piliai 项目中验证 Android/Kotlin/Rust 编译时
type: workflow
---
本地 Gradle 环境无法启动，不能进行本地构建验证。所有编译验证必须依赖 CI（GitHub Actions），当前基线要求 CI 全绿（APK 约 26.06 MB + rust-lib 0.52 MB）。