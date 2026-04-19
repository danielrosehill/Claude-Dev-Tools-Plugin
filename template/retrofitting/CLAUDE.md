# Retrofitting Workspace

This workspace is provisioned by the `dev-tools` plugin (variant: `retrofitting`). Use it to systematically retrofit existing repositories with Claude Code scaffolding — CLAUDE.md, subagents, slash commands, skills, settings, and MCP config.

## Primitives

- `/dev-tools:make-agent-friendly` — prepare a human-developed codebase for agentic development (end-to-end pass).
- `retrofit-scan` skill — scan a directory of repos for missing scaffolding.
- `retrofit-retrofit` / `retrofit-retrofit-this` skills — bulk or single-repo retrofit.
- `retrofit-interactive` / `retrofit-auto` skills — with or without user approval gates.
- `retrofit-add-agents` / `retrofit-add-commands` / `retrofit-new-plugin` — targeted additions.

See `foundational.md` for the principles these skills apply.

## Layout

- `targets/` — list of repositories queued for retrofit.
- `scan-reports/` — output from `retrofit-scan`.
- `logs/` — per-retrofit change logs.
- `context/` — conventions, defaults, and reusable snippets to inject.

## Usage

1. Seed `targets/targets.md` with repos to retrofit.
2. Run `retrofit-scan` to see what's missing.
3. Run `retrofit-interactive` (prompt-gated) or `retrofit-auto` (hands-off) to apply.
