# Memory Audit and Instruction Sync Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add two maintenance commands, `memory-audit` and `sync-instructions`, that help keep durable knowledge and host-facing instructions aligned.

**Architecture:** Implement the feature as two new markdown command files under `commands/`. `memory-audit` stays read-only and reports drift, duplicates, and stale guidance. `sync-instructions` runs audit-first, shows the proposed edits, and only then applies the smallest necessary doc changes.

**Tech Stack:** Markdown, YAML frontmatter, git

**Spec:** `docs/superpowers/specs/2026-04-09-memory-audit-instruction-sync-design.md`

---

## File Structure

| File | Responsibility | Action |
|---|---|---|
| `commands/memory-audit.md` | Read-only maintenance audit command | Create |
| `commands/sync-instructions.md` | Audit-first instruction sync command | Create |

---

### Task 1: Add the read-only maintenance audit command

**Files:**
- Create: `commands/memory-audit.md`

- [ ] **Step 1: Write the command file**

Write `commands/memory-audit.md` with:

```md
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
```

- [ ] **Step 2: Verify the command is self-contained**

Check that the file:
- has valid YAML frontmatter
- states that it is read-only
- names the inspection surfaces explicitly
- tells the agent what the report must contain

Run:

```bash
sed -n '1,220p' commands/memory-audit.md
```

Expected: the full command text above, with no placeholders and no write instructions.

---

### Task 2: Add the audit-first instruction sync command

**Files:**
- Create: `commands/sync-instructions.md`

- [ ] **Step 1: Write the command file**

Write `commands/sync-instructions.md` with:

```md
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
```

- [ ] **Step 2: Verify the command is approval-gated**

Check that the file:
- lists all instruction surfaces from the spec
- requires an audit pass first
- requires the proposed edits to be shown before patching
- limits writes to the smallest necessary edits

Run:

```bash
sed -n '1,220p' commands/sync-instructions.md
```

Expected: the full command text above, with explicit approval gating and no hidden write path.

---

### Task 3: Validate and commit the command pair

**Files:**
- Test: `commands/memory-audit.md`
- Test: `commands/sync-instructions.md`

- [ ] **Step 1: Verify both command files exist**

Run:

```bash
test -f commands/memory-audit.md && test -f commands/sync-instructions.md
```

Expected: exit code `0`.

- [ ] **Step 2: Review the diff**

Run:

```bash
git diff -- commands/memory-audit.md commands/sync-instructions.md docs/superpowers/plans/2026-04-09-memory-audit-instruction-sync.md docs/superpowers/specs/2026-04-09-memory-audit-instruction-sync-design.md
```

Expected: only the new plan and the two new command files are present in the diff for this feature.

- [ ] **Step 3: Commit the work**

```bash
git add commands/memory-audit.md commands/sync-instructions.md docs/superpowers/plans/2026-04-09-memory-audit-instruction-sync.md
git commit -m "feat: add memory audit and instruction sync commands"
```

