---
name: json_codec_roundtrip_failure
description: DnsRuleJsonCodec loses rule ordering and some field mappings during export/import
---
**File:** app/src/main/java/com/sbai/service/DnsRuleJsonCodec.kt
**Lines:** ~30-50 (toRule/fromRule functions)
**Problem:** Rule objects serialized to JSON lose their original position in the list due to HashMap iteration order. Additionally, some sing-box specific fields (like `domain_suffix` vs `domain`) get mapped incorrectly during round-trip conversion.
**Symptom:** After export and re-import, DNS rules appear in random order and some domain-based rules stop working correctly.
**Fix:** Preserve rule index in JSON structure and add explicit field name mappings for sing-box format variants