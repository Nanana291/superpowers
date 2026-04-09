---
description: "Use when you need to audit durable project knowledge for contradictions, duplicates, stale references, or orphaned guidance before making changes"
---

Audit the repo's durable knowledge surfaces before making any changes.

Inspect:
- `docs/superpowers/specs/`
- `docs/plans/`
- `docs/`
- `README.md`
- `CHANGELOG.md`
- `RELEASE-NOTES.md`
- `CLAUDE.md`
- `QWEN.md`
- `AGENTS.md`

Find and report:
- duplicate guidance
- contradictory guidance
- stale references to removed commands or old behavior
- vague or overbroad instructions
- orphaned docs that no longer appear to be referenced
- important knowledge split across too many places

Return a concise report with:
- a short scan summary
- severity-ranked findings
- exact file references
- a concrete recommendation for each finding

Do not edit any files in this command.
