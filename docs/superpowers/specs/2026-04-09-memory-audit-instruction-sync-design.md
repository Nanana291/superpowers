# Memory Audit and Instruction Sync Commands

Add two maintenance entrypoints that help keep Superpowers' durable knowledge and host instructions aligned without turning the commands layer into a dump of ad hoc workflows.

## Problem

Today the `commands/` directory only contains deprecation stubs that redirect users to skills. That is fine for migration, but it leaves two recurring maintenance jobs without a strong, discoverable home:

1. Checking durable project knowledge for contradictions, duplicates, stale references, and orphaned guidance.
2. Keeping host-facing instruction surfaces in sync when the repo changes.

Those tasks are currently handled manually, which makes drift easy to miss and makes maintenance feel scattered across many files.

## Goals

- Provide one command for read-only knowledge auditing.
- Provide one command for instruction synchronization with an audit-first flow.
- Keep both commands core-safe, dependency-free, and narrow in scope.
- Make the output actionable enough that a human can fix problems quickly.
- Preserve the repo's existing philosophy: inspect first, edit only after confirmation.

## Non-Goals

- No new training pipeline.
- No automatic memory promotion into external systems.
- No broad rewrite of skills, prompts, or repository philosophy.
- No hidden writes during audit mode.

## Proposed Command Pair

### `memory-audit`

Purpose: scan the repo's durable knowledge surfaces and report quality issues.

Surfaces to inspect:

- `docs/superpowers/specs/`
- `docs/plans/`
- `docs/`
- `README.md`
- `CHANGELOG.md`
- `RELEASE-NOTES.md`
- `CLAUDE.md`
- `QWEN.md`
- `AGENTS.md`

Findings to report:

- duplicate guidance
- contradictory guidance
- stale references to removed commands or old behavior
- vague or overbroad instructions
- orphaned docs that no longer appear to be referenced
- important knowledge that is split across too many places

Output format:

- short summary of what was scanned
- severity-ranked findings
- exact file references
- concrete recommendation for each finding

This command writes nothing.

### `sync-instructions`

Purpose: compare instruction surfaces and repair drift with explicit approval.

Surfaces to compare:

- `CLAUDE.md`
- `QWEN.md`
- `AGENTS.md`
- `README.md`
- `docs/README.codex.md`
- `docs/README.opencode.md`
- `.codex/INSTALL.md`
- `.qwen/INSTALL.md`

Drift to detect:

- inconsistent install and update steps
- mismatched terminology for skills, commands, and host behavior
- stale links or stale command names
- platform-specific caveats present in one surface but missing in another
- instruction wording that has diverged from the repo's current behavior

Behavior:

1. Run an audit pass first.
2. Show the user the proposed edits before changing anything.
3. Apply only the smallest edits needed to restore alignment.
4. Report the final state after sync.

Default behavior is read-only until the user approves the patch.

## Design Rationale

The pair splits maintenance into two clear responsibilities:

- `memory-audit` is the diagnostic tool.
- `sync-instructions` is the repair tool.

That separation keeps the audit command trustworthy and keeps the sync command from becoming a vague "fix everything" bucket.

If a future use case needs write-heavy maintenance, it should extend `sync-instructions` rather than add a third overlapping command.

## Implementation Shape

- Add each command as a markdown file under `commands/`.
- Keep the command copy short, explicit, and action-oriented.
- If shared scanning rules are needed, factor them into a small reference doc instead of duplicating logic in multiple commands.
- Do not introduce new dependencies or runtime hooks for this work.

## Validation

- Confirm both commands are discoverable by the supported harnesses.
- Confirm `memory-audit` performs a read-only scan and never edits files.
- Confirm `sync-instructions` reports proposed edits before writing anything.
- Validate the commands against at least one deliberate drift example.

