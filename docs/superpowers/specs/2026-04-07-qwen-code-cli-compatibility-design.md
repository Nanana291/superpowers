# Qwen Code CLI Compatibility Design

Add first-class Qwen Code CLI installation support to Superpowers without creating a parallel skill system.

## Goal

Make `Nanana291/superpowers` installable in Qwen Code with one native command and predictable behavior:

- `qwen extensions install Nanana291/superpowers`

The extension should expose the existing `skills/`, `commands/`, and `agents/` directories and load a Qwen-specific context file.

## Constraints

- Reuse the repo's existing skills, commands, and agents as the source of truth.
- Do not introduce a second copy of commands or skills for Qwen.
- Keep installation aligned with Qwen's native extension format from the official docs.
- Scope is limited to installability and basic discoverability, not deeper tool remapping or workflow rewrites.

## Design

### 1. Native Qwen manifest

Add `qwen-extension.json` at the repository root with:

- `name: "superpowers"`
- `version` matching `package.json`
- `contextFileName: "QWEN.md"`
- `commands: "commands"`
- `skills: "skills"`
- `agents: "agents"`

This lets Qwen discover the repo's existing assets directly, with no bootstrap script or manual symlink setup.

### 2. Qwen context file

Add `QWEN.md` at the repository root. It should mirror the minimal pattern already used by `GEMINI.md`:

- include `skills/using-superpowers/SKILL.md`
- include the Qwen-specific tool mapping reference

This keeps the extension thin and uses the existing skill system rather than inventing a separate Qwen prompt.

### 3. Install instructions

Add `.qwen/INSTALL.md` with:

- install command
- update command
- uninstall command
- verification steps
- short note that Qwen loads commands, skills, agents, and `QWEN.md` from the extension

The document should use the fork URL, not the upstream `obra/superpowers` URL.

### 4. README update

Add a `Qwen Code CLI` installation section to `README.md` with the exact install and update commands. Keep the wording parallel to the existing platform sections.

## Non-Goals

- No Qwen-specific command rewrites
- No new MCP server
- No conversion of skill content into a Qwen-only format
- No behavioral changes to the skills themselves

## File Changes

| File | Change |
|---|---|
| `qwen-extension.json` | new Qwen extension manifest |
| `QWEN.md` | new Qwen context file |
| `.qwen/INSTALL.md` | new installation guide |
| `README.md` | add Qwen install docs |

## Verification

- `qwen-extension.json` parses as valid JSON
- files referenced by the manifest exist
- `README.md` and `.qwen/INSTALL.md` both show a working install command using `Nanana291/superpowers`
