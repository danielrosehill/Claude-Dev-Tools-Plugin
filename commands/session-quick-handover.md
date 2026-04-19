---
description: Minimal YAML handover + spawn new session. Use /write-handover for no spawn.
---

Create a minimal YAML `HANDOVER.md` and spawn a new Claude session.

## Steps

1. Run `git status`, `git log --oneline -3`, `git diff --stat`.
2. Review conversation context for goal, progress, pitfalls.
3. Write `HANDOVER.md` using this exact format:

```yaml
# Quick Handover
meta:
  branch: current branch
  last_commit: hash msg
  status: in_progress|blocked|review

goal: one sentence
done: comma-separated items or ~
next: single most important step — file path + what to do
dont_repeat: one failed approach or ~
watch_out: one warning/gotcha or ~
uncommitted: yes/no + brief desc
```

4. Commit and push: `git add HANDOVER.md && git commit -m "Quick handover: [task]" && git push`
5. Report summary to the user.
6. Spawn new session:

```bash
REPO_ROOT="$(git rev-parse --show-toplevel)"
konsole --workdir "$REPO_ROOT" -e claude "/session-transfer:start-from-handover" &
```

Tell user a new session has been spawned.

For richer context use `/handover`. For no spawn use `/write-handover`.
