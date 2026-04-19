<!--
STATUS.md — auto-generated snapshot. Do not hand-edit.
This file (templates/status.md) is the SKELETON that /status:refresh
hydrates from the current state of management/ and inputs/.
It is not the live STATUS.md — the live file lives at the repo root.
Edit this template only to change the shape of future snapshots;
never edit the generated root STATUS.md by hand.
-->

# Status

_Last refreshed: {{timestamp}}_

## Active plans
{{list of files in management/plans/active/ — title + 1-line hook + path}}

## Open blockers
{{list of files in management/blockers/ — title + path; empty → "None."}}

## Pending prompts
{{count + titles of files in inputs/prompts/pending/; empty → "None."}}

## Last session
{{newest file in management/sessions/ — title + date + path}}

## Last agent log entry
{{last line of newest file in management/agent-logs/}}

## Last decision
{{newest file in management/decisions/ — title + path}}

## Current stack
{{link to management/stack/current.md if present}}
