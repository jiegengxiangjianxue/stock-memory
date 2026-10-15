---
name: user_verification_discipline
description: "Applies to any project where the user asks for analysis or comparison — enforces the user's stated preference for verified claims over speculative ones."
---
- Every architectural claim must be grounded in actually-read source code (file + line range), not assumption.
- Reference-project comparisons are only valid if source was inspected in-session; otherwise mark them *unverified*.
- Explicitly distinguish *implemented*, *incomplete*, and *unverified* statuses for each finding.
- Never claim runtime or build verification unless actually performed.
- Report access limitations (e.g., paywalled docs, private repos) explicitly rather than silently omitting them.