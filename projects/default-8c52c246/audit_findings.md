---
name: audit_findings
description: Audit results for SingBoxConfigGenerator and ConfigMerger against sb-lxf kernel v1.15
---
**Severity: Critical (Kernel rejects startup)**

**1. ConfigMerger.preserve_unknown_fields (app/src/main/java/com/sbai/service/ConfigMerger.kt:~140)**
- **Problem**: When merging imported user JSON, the merger preserves unknown top-level fields if `preserve_unknown` is enabled. The sb-lxf (sing-box 1.15) kernel rejects JSON objects containing unrecognized top-level keys with `invalid config` error.
- **Fix**: Disable `preserve_unknown` for top-level config merges, or whitelist only known sing-box keys before preserving.

**2. Deprecated `rule_set_tag` usage (app/src/main/java/com/sbai/service/SingBoxConfigGenerator.kt:~200)**
- **Problem**: Generator may emit `rule_set_tag` in rule objects. In sing-box 1.15, this field was removed; use `rule_set` instead.
- **Fix**: Replace `rule_set_tag` with `rule_set` in rule JSON generation.

**Severity: Medium (Field silently ignored)**

**3. DNS fake-ip fields placement (SingBoxConfigGenerator.kt:~85)**
- **Problem**: `fake-ip` block placed at top level instead of under `dns.servers[*].fake_ip`. Kernel silently ignores top-level `fake-ip`.
- **Fix**: Move `fake-ip` into the appropriate DNS server object.

**4. Outbound `detour` string vs struct (Models.kt:~50)**
- **Problem**: `detour` specified as plain string in some outbound definitions. sing-box 1.15 expects `detour` to be a structured ref or omitted. String value may be ignored rather than causing error.
- **Fix**: Ensure `detour` values reference valid inbound tags or use proper struct format.

**Severity: Low (Validation gap)**

**5. DNS rule actions not validated (ConfigMerger.kt:~90)**
- **Problem**: No validation that DNS rule `actions` are valid sing-box 1.15 actions (e.g., `answer`, `client_subnet`, `ip_cidr`). Invalid actions silently dropped by kernel.
- **Fix**: Add allowlist validation for DNS action types before serialization.