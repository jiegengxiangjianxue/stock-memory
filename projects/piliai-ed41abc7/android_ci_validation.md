---
name: android_ci_validation
description: 修改 Android 代码后，提交前应执行构建、Lint 和回归测试门禁。
type: workflow
---
本项目的改动已通过 Android 构建、Lint（0 errors、0 warnings）以及 playback headers/session/network、PiP 参数、owned resolver、订阅搜索与链接、JSONPath、RSS、scraper、todaywatch 等回归测试。后续修改应至少复现这些 CI 验证。