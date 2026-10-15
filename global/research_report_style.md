---
name: research_report_style
description: "User's preferred methodology and output format for read-only code-research tasks (use when planning or executing any repo-inspection / architecture-borrowing research)"
---
- **Mode**: Read-only; no file writes, no shell, no install/clone, no config changes.
- **Public repos**: Use websearch/webfetch only; cite exact revisions (tag/commit SHA) and license.
- **Local code**: Use readFile/search only when needed.
- **Report format** (concise English): source URLs + revisions → file paths / line-level evidence → concrete integration candidates → incompatibilities → runtime acceptance tests.
- **Security**: Never use credentials or bypass access restrictions.
- **Do NOT implement** during research; return analysis only.