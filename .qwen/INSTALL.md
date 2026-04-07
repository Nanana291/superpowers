# Installing Superpowers for Qwen Code CLI

Install Superpowers as a native Qwen extension. Qwen will load the extension's `QWEN.md`, `commands/`, `skills/`, and `agents/` automatically.

## Prerequisites

- Qwen Code CLI
- Git

## Installation

Install from GitHub:

```bash
qwen extensions install https://github.com/Nanana291/superpowers
```

Short form:

```bash
qwen extensions install Nanana291/superpowers
```

Restart Qwen Code after installation.

## Verify

Start a new Qwen session and ask for something that should trigger a skill, for example:

- `help me plan this feature`
- `let's debug this issue`

You can also confirm the extension is installed:

```bash
qwen extensions list
```

## Updating

```bash
qwen extensions update superpowers
```

## Uninstalling

```bash
qwen extensions uninstall superpowers
```

## How It Works

The extension uses Qwen's native extension manifest:

- `qwen-extension.json` registers the extension
- `QWEN.md` provides persistent context
- `commands/` exposes custom slash commands
- `skills/` exposes Superpowers skills
- `agents/` exposes bundled subagents

## Getting Help

- Report issues: https://github.com/Nanana291/superpowers/issues
- Main documentation: https://github.com/Nanana291/superpowers
