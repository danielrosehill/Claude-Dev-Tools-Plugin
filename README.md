# dev-tools

Claude Code plugin: dev-tools workflow — scaffold-repo, multi-agent QA review, templatize.

> Session-handover commands and the `session-handover-writer` agent moved out of dev-tools in 1.2.0. They now live in the [`claude-hopper`](https://github.com/danielrosehill/Claude-Hopper) plugin.

Consolidates six former plugins — `repo-retrofitter`, `make-agent-friendly`, `qa-team`, `claude-templatizer`, `claude-janitor`, `session-transfer` — and eight template repos into one cluster plugin.

Part of the [danielrosehill Claude Code marketplace](https://github.com/danielrosehill/Claude-Code-Plugins).

## What you get

### Commands (namespaced by source)

**Retrofitting / agent-friendliness:**
- `/dev-tools:make-agent-friendly` — prepare a human-developed codebase for agentic development.

**QA / code review (`qa-*`):**
- `/dev-tools:qa-orchestrator` — run a coordinated multi-agent QA pass.
- `/dev-tools:qa-team-lead` — QA team lead role.
- `/dev-tools:qa-documentation-lead` — docs-focused QA lead.

### Skills

**Retrofitting (`retrofit-*`):**
- `retrofit-scan`, `retrofit-retrofit`, `retrofit-retrofit-this`, `retrofit-auto`, `retrofit-interactive`, `retrofit-add-agents`, `retrofit-add-commands`, `retrofit-new-plugin`.

**Templatization:**
- `templatize-workspace` — extract shared structure from existing repos into a template.

**Janitor / cleanup (`janitor-*`):**
- `janitor-remove-script-clutter`, `janitor-doc-cleaning`, `janitor-code-cleanup`, `janitor-organisation`, `janitor-separation`, `janitor-remove-doc-sprawl`.

**Session (`session-*`):**
- `session-handover`, `session-write-handover`, `session-quick-handover`, `session-log-and-handover`, `session-start-from-handover`, `session-clipboard-handover`, `session-fork-new-direction`, `session-mirror-to-private`, `session-new-claude-at`, `session-new-claude-here`, `session-repo-from-thread`, `session-setup-agent-logs`, `session-spawn-docs-repo`, `session-spawn-planning-repo`, `session-terminal-here`, `session-junction-relay`.

### Agents (25, all `qa-*`)

- `qa-*` — 25 specialised QA subagents (general QA, cleanup, documentation, backend (API/db/MCP), performance, UX, SEO, deployment, code-non-code separation, etc.).

### Provisioning skill

- `/dev-tools:new-workspace <name> [--variant=<variant>] [--local-only] [--private]`

Variants: `repo-scaffold`, `retrofitting`, `code-review`, `templatization`, `browser-automation`, `agent-workspace`.

Scaffolds a new workspace (CLAUDE.md + variant-specific structure), personalises it from `~/.claude/CLAUDE.md`, and (by default) creates a public GitHub repo for it.

## Pattern

Primitives live in the plugin → globally available from any cwd.
Workspace scaffolds are provisioned as **data** → no `.claude/` tree inside provisioned workspaces.
Plugin updates never touch your workspace data.

See [PLAN.md in Claude-Workspace-Reshaping-190426](https://github.com/danielrosehill/Claude-Workspace-Reshaping-190426) for the full pattern spec this plugin follows.

## Variants

- `repo-scaffold` — creating new GitHub repositories with optional Claude scaffolding.
- `retrofitting` — bulk-retrofitting existing repos with Claude Code scaffolding.
- `code-review` — holding QA findings, checklists, and reports.
- `templatization` — extracting shared structure from multiple repos into a reusable template.
- `browser-automation` — reverse-engineering and scripting browser automation tasks.
- `agent-workspace` — general-purpose structured Claude Code workspace.

## Install

Via the danielrosehill marketplace:

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install dev-tools
```

## License

MIT.
