# Session Continuity Workspace

This workspace is provisioned by the `dev-tools` plugin (variant: `session-continuity`). It stores handover documents, session logs, and cross-session context so a fresh Claude Code instance can resume work without losing state.

## Primitives

- `/dev-tools:session-handover` — end-of-session menu
- `/dev-tools:session-write-handover` / `/dev-tools:session-quick-handover` — YAML handover files
- `/dev-tools:session-log-and-handover` — log + handover + spawn new session
- `/dev-tools:session-start-from-handover` — resume from a prior HANDOVER.md
- `session-*` skills — mirror-to-private, fork-new-direction, spawn-docs-repo, etc.

## Layout

- `handovers/` — timestamped YAML handover files.
- `logs/` — full session transcripts archived here when using `/log-and-handover`.
- `context/` — long-lived facts that should follow every handover.

## Usage

1. At the end of a session, run `/dev-tools:session-write-handover` or `/dev-tools:session-quick-handover`.
2. Start the next session with `/dev-tools:session-start-from-handover`.
