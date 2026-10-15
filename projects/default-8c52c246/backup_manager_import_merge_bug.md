---
name: backup_manager_import_merge_bug
description: BackupManager import overwrites user config with imported data instead of merging
---
**File:** app/src/main/java/com/sbai/service/BackupManager.kt
**Lines:** ~85-95 (importConfigMerge function)
**Problem:** When importing a backup, the code performs a shallow merge where imported values overwrite existing config fields without checking if the imported value is null/default. The merge direction is wrong - it should prefer user's current settings over imported defaults for conflict resolution.
**Symptom:** User's customized DNS rules or routing policies get overwritten by imported backup's simpler defaults, causing silent configuration degradation.
**Fix:** Reverse merge priority: `existingField ?: importedField` instead of `importedField ?: existingField`