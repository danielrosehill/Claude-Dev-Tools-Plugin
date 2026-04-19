# {{PROJECT_NAME}}

{{DESCRIPTION}}

This is a Claude Code workspace for reverse-engineering and scripting browser automation tasks. Given a task briefing — "log into X and download all my Y", "scrape Z table on every page", "submit form W with N variations" — the workspace drives an investigation through BrowserMCP (UI exploration, network capture, DOM mapping) and produces a working script.

## How it works

The workflow is a four-phase loop:

1. **Brief** — capture the task: site, action, success criteria, auth model, anti-bot signals
2. **Investigate** — drive a real browser via BrowserMCP, probe the UI, snapshot DOM, capture network requests, map auth + state, identify the simplest possible automation surface
3. **Design** — pick the right approach (raw HTTP > headless DOM scripting > full browser automation), justify it, write the script
4. **Verify** — run end-to-end, capture results, document edge cases and brittleness, hand back a working artefact

## Folder structure

| Folder | Purpose |
|--------|---------|
| `briefings/` | One file per task — the user's brief, success criteria, constraints |
| `investigations/` | Per-task investigation notes — what was tried, what worked, what the auth/state model looks like |
| `artifacts/network/` | Captured network requests during investigation (HAR files, request/response pairs) |
| `artifacts/dom/` | DOM snapshots, selector maps, accessibility tree dumps |
| `artifacts/screenshots/` | UI screenshots at key flow points |
| `scripts/` | Final automation scripts (Python `requests`, Playwright, raw curl, etc.) |
| `context/` | Site-level context that persists across tasks (known quirks, auth flows, rate limits, robots.txt status) |
| `outputs/` | Run logs, scraped data, final deliverables |

## MCPs

- **BrowserMCP** (default) — drives a real browser the user controls, ideal for sites needing the user's auth/cookies
- **Playwright MCP** (default) — headless automation when the user doesn't need to be in the loop

Both are pre-configured in `.mcp.json`. Approve them on first run.

## Working principles

- **Minimum viable surface.** Always prefer the simplest approach that works. Order: raw HTTP/curl → `requests` with cookies → headless Playwright → full browser automation with mouse/keyboard. Don't reach for browser automation when an undocumented JSON API is right there in the network tab.
- **Investigate before scripting.** Open the site in BrowserMCP, perform the task manually, watch the network tab. The script almost always falls out of what you observe.
- **Capture evidence.** Every investigation writes a HAR or representative request/response to `artifacts/network/` and key DOM snapshots to `artifacts/dom/`. The agent that comes after you should be able to reconstruct your reasoning.
- **Respect the site.** Check robots.txt and ToS before scripting. Document what's allowed/disallowed in `context/site-<name>.md`. Never bypass anti-bot measures (CAPTCHAs, rate limits) — work within them or stop.
- **Auth is not state.** Logged-in cookies are short-lived and account-bound. Document the auth model explicitly; don't bake credentials into scripts.
- **Brittleness > magic.** A script that breaks loudly when a selector changes is better than one that "handles" it silently. Document selectors as fragile-by-default and note what to watch.

## Commands

- `/onboard` — first-run setup: orient on what kind of automation work this workspace is for, install MCP confirmation, set defaults
- `/new-task` — capture a new task brief into `briefings/`, scaffold its investigation folder
- `/investigate` — run the structured investigation flow against a brief (UI exploration → network capture → DOM mapping)
- `/design-script` — given a completed investigation, design and write the automation script
- `/verify-script` — run the script end-to-end, capture results, document edge cases
- `/site-notes` — capture or update persistent site-level notes in `context/`

## Agents

- `task-investigator` — drives BrowserMCP through the investigation phase: explores UI, captures network, maps DOM, assesses anti-bot posture
- `automation-architect` — given an investigation, picks the simplest viable approach and writes the script
- `verifier` — runs the script, captures evidence of success/failure, documents edge cases
