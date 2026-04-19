---
description: Session log to Agent-Logs + full YAML HANDOVER.md + spawn new session. Complete end-of-session sequence.
---

Full end-of-session sequence: log, handover, spawn. Steps in order:

## 1. Write session log

Write JSON log to Agent-Logs archive before creating handover.

Archive path:
- Check memory for `reference_agent_logs.md`
- Fall back to `~/repos/github/my-repos/Agent-Logs/`
- If missing, warn user, suggest `/session-transfer:setup-agent-logs`, continue with handover

If archive exists, write JSON:
- Folder: `DDMMYY/`
- File: `HHMMSS_model-name.json`
- Schema:
```json
{
  "created": "ISO 8601",
  "model": "model ID",
  "agent_type": "Claude Code session",
  "working_directory": "/abs/path",
  "repository": "name or null",
  "branch": "name or null",
  "last_commit": "hash + msg or null",
  "blocked": false,
  "user_input_needed": false,
  "debugging_required": false,
  "pending_tasks": 0,
  "goal": "one-line goal",
  "completed": [],
  "in_progress": [],
  "not_started": [],
  "blockers": [],
  "resumption_summary": "one-paragraph next steps"
}
```

Commit and push:
```bash
cd <agent-logs-path> && git add -A && git commit -m "Session log: <goal>" && git push
```

Run `python3 scripts/ingest.py` if it exists.

## 2. Create handover

Use **handover-writer** subagent to create YAML `HANDOVER.md` at repo root. Pass $ARGUMENTS as extra context if present. Verify it exists, give user one-line summary.

## 3. Spawn new session

```bash
REPO_ROOT="$(git rev-parse --show-toplevel)"
konsole --workdir "$REPO_ROOT" -e claude "/session-transfer:start-from-handover" &
```

Report to user: log archived, handover created, new session spawned and resuming.
