---
description: Create YAML HANDOVER.md without spawning a new session. Use /handover or /quick-handover to also spawn.
---

Handover requested **without** spawning a new session. Use **handover-writer** subagent to investigate repo and create YAML `HANDOVER.md` at repo root.

The subagent will:
1. Investigate git state, recent changes, tests, active work
2. Write YAML HANDOVER.md using standardized template
3. Archive JSON session log to Agent-Logs
4. Include metadata (agent type, model, timestamp)

After subagent completes, confirm HANDOVER.md created and give one-line summary.

**Does NOT spawn a new session.** Suggest `/handover` (full) or `/quick-handover` (minimal) if user wants a new session.

Pass $ARGUMENTS as extra context to subagent if present.
