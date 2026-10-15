---
name: evidence_based_audit_style
description: "User's standard for code audits and recommendations: file:line evidence, bounded batches, no unverified comparisons — apply to any project"
---
- Always cite **absolute `file:line`** as evidence; never rely on document claims.
- Do **not** compare against reference projects unless their code was actually inspected in-session.
- When recommending work, return **one bounded implementation batch** with:
  1. Affected symbols (functions/types).
  2. Data-preservation risks.
  3. Functional acceptance checks.
- Default to PLAN mode (read-only) unless user explicitly lifts the restriction.