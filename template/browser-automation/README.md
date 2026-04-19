# Browser Automation Workspace

Provisioned by the `dev-tools` Claude Code plugin (variant: `browser-automation`).

A workspace for using BrowserMCP + Playwright to reverse-engineer and script browser automation tasks against specific websites.

## What it does

You give it a brief — "log into X and download my Y", "submit form Z 30 times with these inputs", "scrape this paginated table" — and it:

1. Drives a real browser (BrowserMCP) to perform the task once while you watch
2. Captures network requests, DOM structure, and auth flow
3. Picks the simplest viable automation approach (often a few HTTP calls, sometimes Playwright)
4. Writes and verifies a working script
5. Documents the brittle parts so you know what'll break first

## Pre-configured MCPs

`.mcp.json` (add your own) should ship with BrowserMCP and/or Playwright MCP.

## Folder map

```
briefings/         one .md per task — what you want done
investigations/    per-task notes — what was learned
artifacts/
  network/         HAR / request-response captures
  dom/             selector maps, DOM snapshots, accessibility trees
  screenshots/     UI captures at key flow points
scripts/           final automation scripts
context/           site-level persistent notes
outputs/           run logs, scraped data
```

## Principles

- Prefer raw HTTP over browser automation when both work
- Always investigate before scripting — the script falls out of the network tab
- Respect robots.txt, ToS, and rate limits
- Be honest about brittleness — selectors change
- Never bypass CAPTCHAs or anti-bot measures
