---
name: no_hardcoded_secrets
description: 项目中 token/密钥必须从环境变量读取，禁止硬编码
type: workflow
---
在 stocks-ai-view 项目中，所有脚本和代码涉及 token、密钥等敏感信息时，必须使用 `os.environ.get()` 从环境变量获取，严禁在源码中硬编码。