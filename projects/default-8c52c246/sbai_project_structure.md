---
name: sbai_project_structure
description: When working on sb-AI DNS/fakeIP/fallback features, read key files first.
---
sb-AI is an Android app at ~/workspace: Kotlin/Jetpack Compose, sing-box kernel. Modules: `app` (UI) and `:core` (separate process running the kernel); the cross-process boundary is a known concurrency risk.

Key service files: `app/src/main/java/com/sbai/service/` (NodeAutoDisabler, NodeDedup, NodeBatchTester, WarpClient, SubscriptionRuleMerger, PublicIpLookup).

For DNS/fakeIP/fallback work, read first:
- `app/src/main/java/com/sbai/data/Models.kt` – data models (DNS fields, config schemas)
- `app/src/main/java/com/sbai/service/SingBoxConfigGenerator.kt` – generates sing-box JSON
- `app/src/main/java/com/sbai/ui/settings/SettingsSubScreens.kt` – settings UI

Build: :app:assembleDebug (no productFlavors, arm64-v8a only); scripts/fetch-libbox.sh, .github/workflows/build.yml. `core/sb-lxf` and `scripts/build-libbox.sh` are untracked (not in git). AGENTS.md created 2026-10-11 with context-hygiene rules and verified key-file paths. bettbox/FlClash are external references only. Memory dir: `~/workspace/.aharou/memory/`.