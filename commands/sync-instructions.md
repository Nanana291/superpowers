---
description: "Use when you need to compare and repair instruction drift across host-facing docs and repo guidance with explicit approval"
---

Sync instruction surfaces with an audit-first flow.

Compare:
- `CLAUDE.md`
- `QWEN.md`
- `AGENTS.md`
- `README.md`
- `docs/README.codex.md`
- `docs/README.opencode.md`
- `.codex/INSTALL.md`
- `.qwen/INSTALL.md`

Look for:
- inconsistent install and update steps
- mismatched terminology for skills, commands, and host behavior
- stale links or stale command names
- platform-specific caveats present in one surface but missing in another
- instruction wording that has diverged from the repo's current behavior

Process:
1. Run the audit pass first.
2. Show the user the proposed edits before changing anything.
3. Apply only the smallest edits needed to restore alignment.
4. Report the final state after sync.

Default to read-only until the user approves the patch.
