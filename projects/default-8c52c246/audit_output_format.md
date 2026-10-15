---
name: audit_output_format
description: When delivering a remote repo audit, read this for the required deliverables
---
Mandatory fields: (1) inspected commit SHA; (2) source permalinks with line ranges; (3) actual failing run ID / step name / raw error text (distinguish log-confirmed failures from source-level risks); (4) one bounded repair batch listing affected config keys/functions, verification commands, and rollback considerations. If repo or logs are inaccessible, state precisely what could and could not be verified.