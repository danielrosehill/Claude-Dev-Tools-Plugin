---
name: templatize-workspace
description: Use when the user wants to turn a set of existing Claude Code workspace/project repos into a reusable GitHub template repo. Triggers on phrases like "templatize this workspace", "make a template from these repos", "create a claude workspace template".
---

# Templatize Claude Workspace

Given a list of existing repos that share a common Claude Code workspace pattern, extract the commonalities into a new template repo and publish it to GitHub.

## Inputs to gather

1. **Source repos** — list of paths or GitHub URLs the user considers representative of the pattern. Require at least 2; more is better.
2. **Template name** — slug for the new repo (suggest one from the pattern if the user doesn't provide).
3. **Visibility** — ask explicitly: public or private. Do not assume.
4. **Destination** — default to `~/repos/github/<name>/`. Confirm if different.

If any of these are missing, ask before proceeding.

## Procedure

1. **Survey the sources**
   - For each source repo, read: `CLAUDE.md`, `.claude/` (settings.json, agents/, commands/, skills/, hooks/), `.mcp.json`, `README.md`, top-level structure, and any `.gitignore` / tooling configs.
   - Do not read deep into application code — this skill templates the *workspace scaffolding*, not the domain logic.

2. **Identify commonalities**
   - Files that appear (byte-identical or near-identical) across most sources → include verbatim.
   - Files that appear everywhere but with project-specific content → include as a template with `{{placeholders}}` for the varying parts (project name, description, domain-specific sections).
   - Patterns that appear in only one source → exclude unless the user flags them as worth keeping.
   - Present a short summary of what will and will not be templated, and wait for confirmation before writing files.

3. **Build the template locally**
   - Create the destination directory and `git init`.
   - Write the templated files. Prefer `{{PROJECT_NAME}}`, `{{PROJECT_DESCRIPTION}}` style placeholders — keep them consistent across files.
   - Add a `README.md` explaining: what the template is for, which source repos it was derived from, what placeholders exist, and how to instantiate it (`gh repo create --template ...`).
   - Add a `.gitignore` merged from the sources.
   - If the pattern includes `.claude/settings.json` with machine-specific paths, sanitize them.

4. **Publish to GitHub**
   - Use `gh repo create <name> --<public|private> --source=. --remote=origin --push`.
   - After creation, enable template-repo mode: `gh repo edit <owner>/<name> --template=true`.
   - Report the URL.

## Safety rules

- **Always ask visibility.** Never default to public.
- **Never copy secrets.** Scan for `.env`, API keys, tokens, or hardcoded credentials before writing template files. If found in a source, omit and flag to the user.
- **Never modify the source repos.**
- Confirm the file list before writing and before pushing.
