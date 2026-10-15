---
name: plan_mode_constraints
description: When the user activates PLAN mode, read this to know what is forbidden
---
PLAN mode = strictly read-only. Forbidden: executing commands, cloning, downloading to disk, edits, installs, config changes, commits, pushes. Only read-only web inspection is allowed. Implementation/push requests noted during PLAN are **not** authorized until the user explicitly lifts the mode. Never claim a build was executed.