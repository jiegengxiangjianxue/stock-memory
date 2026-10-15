---
name: clash_yaml_parser_security
description: "ClashYamlParser doesn't sanitize server addresses, enabling SSRF via crafted subscription"
---
**File:** app/src/main/java/com/sbai/service/ClashYamlParser.kt
**Lines:** ~45-60 (parseServers function)
**Problem:** Server addresses from Clash YAML are used directly in URL construction without validation. Malformed YAML containing `127.0.0.1:6666` or internal network addresses bypasses the format check.
**Symptom:** App may attempt connections to internal services, leaking subscription data to local networks or triggering SSRF vulnerabilities.
**Fix:** Add strict regex validation: `^([a-zA-Z0-9\-\.]+)\:\d{1,5}$` and reject localhost/internal IP ranges before processing