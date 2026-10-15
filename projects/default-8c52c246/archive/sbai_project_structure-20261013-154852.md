---
name: sbai_project_structure
description: When working on sb-AI DNS/fakeIP/fallback features, read key files first.
---
sb-AI is an Android app at ~/workspace using sing-box (SingBoxConfigGenerator.kt). Key files:
- `app/src/main/java/com/sbai/data/Models.kt` – data models (DNS fields, config schemas)
- `app/src/main/java/com/sbai/service/SingBoxConfigGenerator.kt` – generates sing-box JSON from model
- `app/src/main/java/com/sbai/ui/settings/SettingsSubScreens.kt` – settings UI
- `scripts/fetch-libbox.sh` and `.github/workflows/build.yml` – build pipeline
- `core/sb-lxf` and `scripts/build-libbox.sh` are **untracked** (not in git)
AGENTS.md originally absent; created 2026-10-11 with context-hygiene rules, build command :app:assembleDebug (no productFlavors, arm64-v8a only), and verified key-file paths. bettbox/FlClash are external references for DNS/fakeIP/fallback, not in this workspace. Memory dir: `~/workspace/.aharou/memory/`.