---
name: evidence_based_audit_style
description: "User's standard for code audits and recommendations: file:line evidence, bounded batches, no unverified comparisons — apply to any project"
---
For code-review / bug-audit tasks:
- Output in **English**, hard cap **500 words**.
- Cite absolute `file:line` as evidence; never speculate or rely on document claims. Do not compare against reference projects unless their code was actually inspected in-session.
- For each defect: concrete problem → user-visible symptom → minimal fix.
- Rank by severity: P0 security/data-loss, P1 logic, P2 concurrency, P3 leaks/silent-failure.
- When recommending work, return one bounded implementation batch with: affected symbols, data-preservation risks, functional acceptance checks.
- Read-only (PLAN mode): do not write files or run builds unless user explicitly lifts the restriction.