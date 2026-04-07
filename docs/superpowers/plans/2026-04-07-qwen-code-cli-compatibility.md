# Qwen Code CLI Compatibility Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a native Qwen Code CLI installation path for Superpowers using `qwen-extension.json`, `QWEN.md`, and `.qwen/INSTALL.md`.

**Architecture:** Use Qwen's native extension manifest to point at the existing `commands/`, `skills/`, and `agents/` directories. Add a minimal `QWEN.md` context file and document the install flow in both `.qwen/INSTALL.md` and `README.md`.

**Tech Stack:** Markdown, JSON

**Spec:** `docs/superpowers/specs/2026-04-07-qwen-code-cli-compatibility-design.md`

---

## File Structure

| File | Responsibility | Action |
|---|---|---|
| `qwen-extension.json` | Qwen extension manifest | Create |
| `QWEN.md` | Qwen extension context | Create |
| `.qwen/INSTALL.md` | Qwen install guide | Create |
| `README.md` | Cross-platform install docs | Modify |

---

### Task 1: Add Qwen extension metadata

**Files:**
- Create: `qwen-extension.json`
- Create: `QWEN.md`

- [ ] **Step 1: Create the manifest**

Write `qwen-extension.json` with:

```json
{
  "name": "superpowers",
  "description": "Core skills library: TDD, debugging, collaboration patterns, and proven techniques",
  "version": "5.0.7",
  "contextFileName": "QWEN.md",
  "commands": "commands",
  "skills": "skills",
  "agents": "agents"
}
```

- [ ] **Step 2: Create the Qwen context file**

Write `QWEN.md` with:

```md
@./skills/using-superpowers/SKILL.md
@./skills/using-superpowers/references/qwen-tools.md
```

- [ ] **Step 3: Verify referenced paths**

Confirm that:
- `commands/` exists
- `skills/` exists
- `agents/` exists
- `skills/using-superpowers/SKILL.md` exists
- `skills/using-superpowers/references/qwen-tools.md` exists

---

### Task 2: Add install instructions

**Files:**
- Create: `.qwen/INSTALL.md`
- Modify: `README.md`

- [ ] **Step 1: Create `.qwen/INSTALL.md`**

Document:
- install: `qwen extensions install https://github.com/Nanana291/superpowers`
- shorthand install: `qwen extensions install Nanana291/superpowers`
- update: `qwen extensions update superpowers`
- uninstall: `qwen extensions uninstall superpowers`
- verify: start a new Qwen session and trigger a skill

- [ ] **Step 2: Update `README.md`**

Add a `Qwen Code CLI` section in Installation with:

```bash
qwen extensions install https://github.com/Nanana291/superpowers
```

And an update snippet:

```bash
qwen extensions update superpowers
```

- [ ] **Step 3: Verify consistency**

Check that:
- repo URL matches the fork in both docs
- extension name matches `qwen-extension.json`
- wording does not claim unsupported behavior

---

### Task 3: Validate outputs

**Files:**
- Test: `qwen-extension.json`
- Test: `.qwen/INSTALL.md`
- Test: `README.md`

- [ ] **Step 1: Validate JSON**

Run:

```bash
node -e "JSON.parse(require('fs').readFileSync('qwen-extension.json','utf8')); console.log('ok')"
```

Expected: `ok`

- [ ] **Step 2: Check file references**

Run:

```bash
test -f QWEN.md && test -f .qwen/INSTALL.md && test -d commands && test -d skills && test -d agents
```

Expected: exit code `0`

- [ ] **Step 3: Review diff**

Run:

```bash
git diff -- qwen-extension.json QWEN.md .qwen/INSTALL.md README.md docs/superpowers/specs/2026-04-07-qwen-code-cli-compatibility-design.md docs/superpowers/plans/2026-04-07-qwen-code-cli-compatibility.md
```

Expected: only the planned Qwen compatibility additions
