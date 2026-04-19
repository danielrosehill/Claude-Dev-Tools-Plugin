---
description: Full handover sequence — write YAML HANDOVER.md then spawn new Claude session to resume
---

Full handover sequence. Steps in order:

1. **Create handover**: Use **handover-writer** subagent to investigate repo and create YAML `HANDOVER.md` at repo root. Pass $ARGUMENTS as extra context if present.

2. **Verify**: Confirm HANDOVER.md exists. Give user a one-line summary.

3. **Spawn new session**:
```bash
REPO_ROOT="$(git rev-parse --show-toplevel)"
konsole --workdir "$REPO_ROOT" -e claude "/session-transfer:start-from-handover" &
```

4. **Report**: Tell user a new Claude session has been spawned and is resuming from the handover.
