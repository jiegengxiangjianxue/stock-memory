---
name: sbai_dns_research_caveats
description: Before trusting any DNS/fakeIP/fallback claims, verify against original sources; this memory file is unverified.
---
The note `~/workspace/.aharou/memory/research_task_sb-ai_dns_tun_20261011.md` is an **incomplete speculative draft** – its repo guesses, DNS schemas, filter-mode names, and claimed equivalents are **not verified evidence**. Always cross-check against original upstream sources (bettbox repo, FlClash repo, sing-box-lx docs) at immutable revisions.

Required terminology distinctions in any DNS work on sb-AI:
- **Server-error failover** (try next DNS on timeout/refused) ≠ **answer-filter fallback** (e.g. NXDOMAIN or empty response triggers alternate resolver).
- **Real DNS** (actual A/AAAA/AAAA lookup) ≠ **fakeIP** (synthetic in-range IP, no real resolution).
- **Persisted configuration** (written to disk / SharedPreferences) ≠ **runtime application** (applied to the running sing-box kernel without restart).

Do not claim a feature works without a passing test.