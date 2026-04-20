---
name: screenshot-site
description: Capture Playwright screenshots of a website under development. Use when the user asks to screenshot the site, grab screenshots of the app/UI, snapshot pages for visual review, or track visual evolution of a site.
---

Capture screenshots of the website in the current repo using Playwright and organise them into a dated archive plus a "latest" pointer folder.

## Inputs

Accept these from the user (all optional):

- **URL(s)** — one or more URLs or paths to capture. If the user gives a path like `/about`, join it with the base URL.
- **Base URL** — defaults to `http://localhost:3000`. If that isn't responding, probe common dev ports (`3000`, `5173`, `8080`, `4321`, `5000`, `8000`) and use the first one that returns 200. If nothing is running, ask the user for the URL or whether they want you to start the dev server.
- **Viewport** — defaults to `1920x1080`. Accept shorthand like `mobile` (390x844), `tablet` (768x1024), `desktop` (1920x1080).
- **Full page** — default true. User can request viewport-only.
- **Name** — optional label for the screenshot file (e.g. `homepage`, `dashboard`). Default to a slug derived from the URL path (`/` → `home`, `/about/team` → `about-team`).

## Output layout

Write into `./screenshots/` relative to the current repo root (create if missing, and add a line to `.gitignore` if the user wants them ignored — ask once the first time you create the folder).

Structure:

```
screenshots/
├── MMDD/              # e.g. 0419 — dated archive for evolution tracking
│   ├── home.png
│   └── dashboard.png
└── latest/            # always overwritten with the most recent run
    ├── home.png
    └── dashboard.png
```

- `MMDD` uses the current local date, zero-padded (April 19 → `0419`).
- Always write each screenshot **both** to the dated folder and to `latest/` (overwriting).
- If a file with the same name already exists in the dated folder from an earlier run the same day, append a time suffix: `home-1430.png`.

## Procedure

1. **Resolve base URL.** If the user gave one, use it. Otherwise probe localhost ports (above) with a quick `curl -s -o /dev/null -w "%{http_code}" http://localhost:<port>` and use the first 2xx/3xx response. If nothing's up, ask.
2. **Resolve pages.** If the user gave specific paths, use them. Otherwise default to just `/` (homepage). If the repo appears to be a multi-page site and the user said "screenshot the site" without specifying, ask which pages.
3. **Prepare folders.** `mkdir -p screenshots/MMDD screenshots/latest`.
4. **Run Playwright.** Prefer the Playwright MCP tools if available (`mcp__plugin_playwright_playwright__browser_navigate`, `browser_resize`, `browser_take_screenshot`). Otherwise write a short Node script using `@playwright/test` or `playwright` and run it with `npx playwright` — install on demand if missing.
5. For each page: navigate, set viewport, wait for network idle, take full-page screenshot, save to both `screenshots/MMDD/<name>.png` and `screenshots/latest/<name>.png`.
6. **Report.** List what was captured with file paths so the user can open them.

## Node script template (fallback when MCP Playwright is unavailable)

```js
const { chromium } = require('playwright');
const fs = require('fs');
const path = require('path');

const BASE = process.env.BASE_URL || 'http://localhost:3000';
const PAGES = (process.env.PAGES || '/').split(',');
const VIEWPORT = (process.env.VIEWPORT || '1920x1080').split('x').map(Number);

const mmdd = (() => {
  const d = new Date();
  return String(d.getMonth() + 1).padStart(2, '0') + String(d.getDate()).padStart(2, '0');
})();

const dated = path.join('screenshots', mmdd);
const latest = path.join('screenshots', 'latest');
fs.mkdirSync(dated, { recursive: true });
fs.mkdirSync(latest, { recursive: true });

(async () => {
  const browser = await chromium.launch();
  const context = await browser.newContext({ viewport: { width: VIEWPORT[0], height: VIEWPORT[1] } });
  const page = await context.newPage();

  for (const p of PAGES) {
    const url = new URL(p, BASE).toString();
    const name = (p.replace(/^\/+|\/+$/g, '') || 'home').replace(/\//g, '-') + '.png';
    await page.goto(url, { waitUntil: 'networkidle' });
    const datedPath = path.join(dated, name);
    const latestPath = path.join(latest, name);
    await page.screenshot({ path: datedPath, fullPage: true });
    fs.copyFileSync(datedPath, latestPath);
    console.log(`Captured ${url} → ${datedPath}`);
  }

  await browser.close();
})();
```

## Notes

- Never commit screenshots automatically; leave that to the user.
- If the dev server isn't running and the user wants one started, check `package.json` for `dev`/`start` scripts and run in background — but ask first.
- For sites behind auth, ask the user how to authenticate (storage state file, login flow) before capturing protected pages.
