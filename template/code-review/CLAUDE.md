# Code Review Workspace

This workspace is provisioned by the `dev-tools` plugin (variant: `code-review`). It is the data surface where QA runs, review notes, and findings accumulate. The review primitives themselves live in the plugin and are globally available:

- `/dev-tools:qa-orchestrator` — run a coordinated QA pass across the target repo
- `/dev-tools:qa-team-lead` / `/dev-tools:qa-documentation-lead` — specialised leads
- `qa-*` subagents — general QA, cleanup, docs, performance, UX, backend, SEO, deployment

## Layout

- `findings/` — one markdown file per completed review, dated.
- `checklists/` — reusable checklists for recurring review passes.
- `reports/` — exported reports for hand-off.
- `context/` — background facts about the target codebase (stack, conventions, priorities).

## Usage

1. Point this workspace at a target repository (record its path in `context/target.md`).
2. Invoke `/dev-tools:qa-orchestrator` to kick off a review.
3. Specialised subagents drop findings into `findings/` as they complete.
4. Aggregate and triage in `reports/`.
