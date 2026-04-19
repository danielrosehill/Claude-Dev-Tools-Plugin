---
description: Resume work from a previous agent's YAML HANDOVER.md
---

Session resume requested. Steps in order:

1. **Find** `HANDOVER.md` at repo root. If missing, tell user no handover to resume from. Stop.

2. **Read** entire HANDOVER.md. Parse the YAML. Internalize: goal, progress, completed/in-progress/remaining, failed approaches (do NOT repeat), blockers, build/test status, resume steps.

3. **Check state drift** — run `git status` and `git log --oneline -3`, compare against handover metadata:
   - Same branch?
   - Commits since handover?
   - Uncommitted changes not in handover?
   
   If significant drift, warn user with specifics. Ask whether to proceed with handover context or get updated direction.

4. **Delete** HANDOVER.md. Use `git rm HANDOVER.md` if tracked, else `rm`. Do not commit deletion — next commit should be meaningful work.

5. **Report** to user: 3-5 bullets summarizing previous agent's work, where they left off, what you'll do next.

6. **Begin work** following resume instructions from handover. Incorporate $ARGUMENTS as additional direction if present.
