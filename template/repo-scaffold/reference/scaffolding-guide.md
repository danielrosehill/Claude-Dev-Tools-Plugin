# Claude Code Scaffolding Guide

Reference for what Claude Code elements to add when creating a new repository. Derived from the Claude Repo Retrofitter patterns.

## Elements

### 1. CLAUDE.md (Agent Guidance File)

The primary file Claude Code reads on every prompt. Should contain:

- Project overview and purpose
- Tech stack and key dependencies
- Build, test, and lint commands
- Code conventions and patterns
- Repo-specific constraints

**Keep it concise.** This is loaded every prompt, so only include essentials. Detailed instructions go in `context/for-agent/` or `.claude/agents/`.

### 2. Slash Commands (.claude/commands/)

Markdown files that define reusable workflows. Create 2-4 commands tailored to the repo type:

| Repo Type | Suggested Commands |
|-----------|-------------------|
| Python library | `/test`, `/lint`, `/review`, `/release` |
| Web app (frontend) | `/dev`, `/test`, `/review`, `/deploy` |
| API service | `/test`, `/review`, `/migrate`, `/deploy` |
| Data pipeline | `/run`, `/test`, `/validate`, `/review` |
| Documentation | `/build`, `/review`, `/publish` |
| CLI tool | `/test`, `/lint`, `/review`, `/release` |
| Infrastructure | `/plan`, `/apply`, `/validate`, `/review` |
| Monorepo | `/test-all`, `/review`, `/deploy`, `/sync` |

Each command file should:
- Have clear, actionable instructions
- Include parallelization hints where independent steps exist (e.g., run lint + type-check + test in parallel)
- Reference actual project files and paths

### 3. Subagents (.claude/agents/)

Specialized agents for complex repos. **Only add when clearly useful:**

- Monorepos with distinct subsystems
- Projects with separate frontend/backend
- Complex CI/CD or infrastructure
- Multi-language projects

**Skip for:** Single libraries, small repos, docs-only projects, most new repos.

### 4. Scaffold Folders

General-purpose directories for organizing project artifacts:

- `context-data/` — Background information, research, reference material
- `planning/` — Project plans, roadmaps, design documents
- `pm/` — Project management artifacts, status updates
- `from-ai/` — AI-generated outputs, suggestions, analysis
- `user-docs/` — User-facing documentation drafts

Each gets a `.gitkeep` file so git tracks the empty directory.

## Generating Contextual Content

When creating Claude scaffolding for a new repo, tailor everything to what the user actually described:

- **Don't guess tech stack** — if they said "Python CLI tool", the CLAUDE.md mentions Python, CLI patterns, argparse/click/typer, pytest. Don't add sections about deployment or frontend.
- **Don't add placeholder commands** — every slash command should serve a real workflow. If you can't think of 4, create 2.
- **Don't add agents prematurely** — a new repo rarely needs subagents. The user can add them later with the Retrofitter's `/add-agents`.
- **Match the naming** — if the repo is `my-cool-tool`, the CLAUDE.md title should be "My Cool Tool", commands should reference `my-cool-tool`.
