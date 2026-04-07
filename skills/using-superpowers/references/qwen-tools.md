# Qwen Code Tool Mapping

Skills use Claude Code tool names. When you encounter these in a skill, use your platform equivalent:

| Skill references | Qwen Code equivalent |
|-----------------|----------------------|
| `Task` tool (dispatch subagent) | Qwen subagents |
| Multiple `Task` calls (parallel) | Multiple Qwen subagents when appropriate |
| `TodoWrite` (task tracking) | Qwen planning/task tools |
| `Skill` tool (invoke a skill) | Qwen skills load natively from the extension |
| `Read`, `Write`, `Edit` (files) | Use Qwen's native file tools |
| `Bash` (run commands) | Use Qwen's native shell tools |
| Slash commands from `commands/` | Qwen custom commands |

## Native extension layout

Qwen Code loads extension assets from `qwen-extension.json`:

- `contextFileName` loads `QWEN.md`
- `commands` exposes custom slash commands
- `skills` exposes skills through `/skills`
- `agents` exposes extension subagents

If the extension is installed, you do not need a separate bootstrap step.

## Subagents

Qwen Code has native subagent support. When a skill says to dispatch a subagent:

1. Prefer the extension agent if one exists in `agents/`
2. Otherwise create or use a suitable Qwen subagent for the task
3. Keep the delegated task narrow and concrete

## Commands

Qwen custom commands are Markdown files under `commands/`. Nested directories map to namespaced commands:

- `commands/brainstorm.md` -> `/brainstorm`
- `commands/fs/grep.md` -> `/fs:grep`

Extension commands are available after restart once the extension is installed.
