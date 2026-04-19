# Agent Instructions

This repository follows the **General Agent Workspace Template** — a folder-based human/agent collaboration layout. Always prefer writing to the structured folders below over ad-hoc file creation.

> **Ignore `.template-meta/`** unless the user is explicitly working on the template itself. That folder is about the template, not about any project built with it.

**Foundational principle.** Humans and agents have separate, well-defined workspaces — but the *point* of the separation is to streamline working together, not to build walls. Every folder below belongs to one side or the other; the contract is what makes handoffs cheap.

## STATUS.md (read this first)

`STATUS.md` at the repo root is the single source of truth for "where are we right now". It lists: active plans, open blockers, pending prompts, last log entry, last session. **Read it at the start of every session.** Refresh it with `/status:refresh` after meaningful work or before ending a session. It is auto-generated from the folders below — never hand-edit.

## Folder contract

| Path | Purpose | Who writes |
|---|---|---|
| `inputs/prompts/pending/` | Prompts/tasks awaiting execution | User |
| `inputs/prompts/edits/` | Reusable edit-round prompts (guide iterative revisions) | User |
| `inputs/prompts/completed/` | Processed prompts | Agent (moves them) |
| `inputs/audio/` | Raw voice notes to transcribe | User |
| `context/to-add/` | Raw context pending ingestion | User |
| `context/store/` | Chunked, organized persistent background context (RAG-style) | Agent (or user) |
| `context/api-docs/` | API references for the agent | User |
| `context/for-agent/` | Long-form SOPs / instructions the user writes for the agent | User |
| `outputs/runs/` | Initial run artifacts | Agent |
| `outputs/combined/` | Aggregated repeat runs | Agent |
| `outputs/pdf/` | Typst-rendered PDFs | Agent |
| `management/project-brief/` | Overarching project brief, goal, stack choices | Agent + user |
| `management/plans/{draft,active,completed,archived}/` | Discrete work plans (UUID-named, sorted by state) | Agent |
| `management/blockers/` | Documented blockers (UUID-named) | Agent |
| `management/debug-recipes/` | Reusable debug recipes (UUID-named, tagged) | Agent |
| `management/sessions/` | Session summaries (timestamp-named) | Agent |
| `management/feature-requests/` | Feature requests (UUID-named) | Agent |
| `management/stack/` | Current stack documentation (`current.md`) | Agent |
| `management/agent-logs/` | Progress logs | Agent |
| `management/handovers/` | Archived session handover docs | Agent |
| `management/decisions/` | Decision records | Agent |
| `docs/for-user/` | Docs the agent writes for the user | Agent |
| `docs/for-world/` | Public-facing drafts | Agent |
| `code/<project>/` | Source code (MUST be nested ≥1 level) | Agent + user |

> **Note:** `management/project-brief/` was previously named `management/planning/`. If you find a `planning/` folder in an older workspace, move its contents to `project-brief/`.

**Where does feedback go?** User feedback and corrections that should inform future work → `management/` (specifically a decision, session note, or agent-log entry — whichever fits). Do *not* put transient feedback in `context/`. Only durable, reusable background facts belong in `context/store/`.

## Code nesting rule (CRITICAL)

For code projects, **all source code must live inside `code/<project-name>/`** — never loose at the repo root and never loose directly inside `code/`. This keeps the organisational framework (inputs/context/outputs/management/docs) cleanly separated from the codebase itself, so the framework can wrap multiple sub-projects and the agent never confuses framework files with source files.

If you find source code violating this rule, propose moving it via `/organise-repo` before continuing work.

## Plans

Any non-trivial piece of work gets a plan file under `management/plans/`. Rules:

- **Filename** must start with a random UUID: `<uuid>--<slug>.md`
- **Frontmatter** must include `id`, `title`, `state`, `created`
- **State is the folder** — plans live in exactly one of `draft/`, `active/`, `completed/`, `archived/`. Move the file to transition state; keep the frontmatter `state:` field in sync.
- Use `/plan:new` to create and `/plan:update` to transition.

## Context vs docs (important)

These two trees look similar — they are not:

- **`context/`** = ongoing *background* the agent pulls on when reasoning. Think RAG corpus: chunked facts, SOPs, API refs, retrievable user instructions. The agent reads from here constantly. **`context/for-agent/`** is where the user drops long-form written instructions for the agent.
- **`docs/`** = discrete, authored *documents* with a named audience. `docs/for-user/` = agent-authored deliverables aimed at the user. `docs/for-world/` = public-facing drafts. Not retrieval material.

Rule of thumb: if you'd want it vectorised and semantically searched → `context/`. If it's a document someone will *read top-to-bottom* → `docs/`.

## Handovers

Handovers have **one specific purpose**: bridging a discontinuity — a session break (context full, restart, crash) or a switch between agents / code editors (Claude Code → Cursor → another Claude session). They are not general status updates, not session summaries, and not progress logs. If you want "where are we right now", that's `STATUS.md`. If you want "what happened in this session", that's `management/sessions/`. Only reach for `/handover:*` when the work is about to be picked up by a different agent or a fresh session.

Session handover is delegated to the `dev-tools` plugin's session-continuity primitives:

- `/dev-tools:session-write-handover` — write `HANDOVER.md` at the repo root
- `/dev-tools:session-start-from-handover` — resume from a prior `HANDOVER.md`
- `/dev-tools:session-handover` — end-of-session menu (log + write + spawn)

**Workflow in this workspace:**

1. Ending a session → run `/dev-tools:session-handover`.
2. Archive the resulting `HANDOVER.md` to `management/handovers/YYYY-MM-DD-HHMM.md` before the next session consumes it.
3. Resume with `/dev-tools:session-start-from-handover`.

The root `HANDOVER.md` is the live handoff file; `management/handovers/` is the permanent archive.

## Templates

Reusable document templates live in `templates/`. Slash commands read these and populate them — do not write structured docs freehand, always start from the template.

| Template | Command | Destination |
|---|---|---|
| `templates/blocker.md` | `/doc:blocker` | `management/blockers/` |
| `templates/decision.md` | `/doc:decision` | `management/decisions/` |
| `templates/stack.md` | `/doc:stack` | `management/stack/current.md` |
| `templates/debug-recipe.md` | `/doc:debug` | `management/debug-recipes/` |
| `templates/session.md` | `/doc:session` | `management/sessions/` |
| `templates/feature-request.md` | `/doc:feature` | `management/feature-requests/` |
| `templates/edit-round.md` | `/doc:edit-round` | `management/plans/draft/` |
| `templates/status.md` | `/status:refresh` | `STATUS.md` (skeleton only; hydrated from folder state, never hand-edited) |

When populating a template from conversation context, fill in everything you already know — only ask the user for genuine gaps.

## Context retrieval

Before answering non-trivial requests, consult `context/store/` for persistent user/project context.

**Pinecone MCP contract.** If a Pinecone MCP is available, treat it as the *primary* vector store: query it first for relevant chunks on non-trivial requests, and when saving new durable context, upsert to Pinecone *and* mirror a plain-markdown copy to `context/store/` so the workspace remains usable offline and the raw text stays version-controlled. If Pinecone is **not** available, skip all vector operations silently and read/write `context/store/` directly — no other behavior changes. Never gate functionality on Pinecone being present.

## Optional MCP integrations

- **Pinecone MCP** — extended vector context store (see § Context retrieval above for the exact contract)
- **Gemini transcription MCP** — for `/ctx:process-audio`
- **GitHub Gist MCP** — for publishing `docs/for-world/` drafts
- **Typst** (local binary) — for `/run:to-pdf`

If an MCP is not available, fall back gracefully to local-only behavior and tell the user what was skipped.

## Output runs

When producing output artifacts, place them under `outputs/runs/<slug>/` where `<slug>` is either a date (`DDMMYY`) or a subject name — the user chooses at `/run:new` time.

**Collision rule.** If `outputs/runs/<slug>/` already exists when `/run:new` is invoked:

1. Append a numeric suffix: `<slug>-2`, then `<slug>-3`, etc. Never overwrite an existing run folder.
2. When the same *subject* has multiple numbered runs, `/run:combine` aggregates them into `outputs/combined/<slug>.md`.
3. Dates (`DDMMYY`) collide intentionally when a second run happens the same day — the `-2` suffix is the correct resolution, not a prompt to the user.

## Logging

Four time-ordered surfaces exist. Don't duplicate across them — pick the right one:

| Write to… | …when |
|---|---|
| `management/agent-logs/` | Meaningful work just happened. One file per day, appended. "What did we do today." |
| `management/sessions/` | A session is ending and you want a summary of *that session specifically*. |
| `management/decisions/` | A non-obvious choice was made and future-you needs the *why*. `NNNN-title.md`. |
| `HANDOVER.md` (root) | A different agent/editor or a fresh session is about to pick up the work. Transient. |

`STATUS.md` is separate — it's a *snapshot*, not a log, and is regenerated, not appended to.
