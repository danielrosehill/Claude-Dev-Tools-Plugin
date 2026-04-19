---
name: new-workspace
description: Provision a new dev-tools workspace on disk. Use when the user wants to start a new repo-scaffold, retrofitting, code-review, templatization, session-continuity, browser-automation, or agent-workspace surface. Accepts a workspace name and a variant. Scaffolds the workspace, personalises CLAUDE.md from the user's global memory, and (by default) creates a GitHub repo.
disable-model-invocation: true
allowed-tools: Bash(mkdir *), Bash(cp *), Bash(cat *), Bash(git init *), Bash(git add *), Bash(git commit *), Bash(gh repo create *), Bash(gh auth status), Bash(git push *), Read
---

# Provision Dev-Tools Workspace

Creates a new workspace for dev-tools workflows. This plugin's commands (`/dev-tools:qa-orchestrator`, `/dev-tools:make-agent-friendly`, `/dev-tools:session-handover`, etc.) and skills (`retrofit-*`, `session-*`, `templatize-workspace`, `janitor-*`) are globally available once installed — this skill only provisions the **data scaffold** (CLAUDE.md, context/, notes structure) that those primitives read from and write to.

## Arguments

`$ARGUMENTS` is parsed as:

- **First positional**: workspace name (kebab-case, used as directory and GitHub repo name). Required.
- **Second positional** (optional): target parent path. Defaults to `~/repos/github/my-repos`.
- **`--variant=<name>`** (optional): which scaffold to copy. One of `repo-scaffold`, `retrofitting`, `code-review`, `templatization`, `session-continuity`, `browser-automation`, `agent-workspace`. Default: `agent-workspace`.
- **`--local-only`** (optional): skip GitHub repo creation and push. Default: create a public GitHub repo and push.
- **`--private`** (optional): create the GitHub repo as private. Default: public.

### Examples

```
/dev-tools:new-workspace my-qa-run --variant=code-review
/dev-tools:new-workspace scrape-acme --variant=browser-automation
/dev-tools:new-workspace bulk-retrofit --variant=retrofitting --local-only
/dev-tools:new-workspace project-plan --variant=agent-workspace
```

## Procedure

### 1. Parse arguments

Extract workspace name, target parent path, variant, and flags from `$ARGUMENTS`. If workspace name is missing, ask the user for it before proceeding.

### 2. Resolve the scaffold path

The bundled scaffold lives at `${CLAUDE_SKILL_DIR}/../../template/<variant>/`. Confirm it exists. If the variant isn't one of the seven listed above, tell the user which variants are available.

### 3. Read ambient facts

Read `~/.claude/CLAUDE.md` if it exists. Extract OS, locale, timezone, and user identity facts. These will personalise the workspace's CLAUDE.md at step 6.

### 4. Create the workspace directory

```bash
mkdir -p <target-parent>/<workspace-name>
cp -r ${CLAUDE_SKILL_DIR}/../../template/<variant>/. <target-parent>/<workspace-name>/
```

Do **not** copy any `.claude/` tree. The plugin's primitives are global.

### 5. Personalise CLAUDE.md

Open the new workspace's `CLAUDE.md` and:

- Replace any placeholder identity with the facts from step 3.
- Add a short header noting the workspace name and variant.
- If ambient facts include OS/locale, embed them so downstream commands can skip re-asking.

### 6. Prompt for workspace-specific facts

Ask the user only for facts this plugin can't infer:

- **repo-scaffold**: the intended new repo's name/description.
- **retrofitting**: path to the directory of repos to retrofit.
- **code-review**: path(s) to the target repository being reviewed.
- **templatization**: source repos to analyse.
- **session-continuity**: nothing usually — it's per-session.
- **browser-automation**: the target site / task brief.
- **agent-workspace**: a one-line project brief.

### 7. Initialise git and (optionally) publish

```bash
cd <target-parent>/<workspace-name>
git init
git add .
git commit -m "Initial workspace from dev-tools plugin"
```

Unless `--local-only` is set:

```bash
gh repo create <workspace-name> --<public|private> --source=. --push
```

Use `--public` by default, `--private` if flag was passed.

### 8. Print next steps

Tell the user:

- Workspace path.
- Variant chosen.
- Which plugin primitives apply (e.g. `/dev-tools:qa-orchestrator` for `code-review`, `retrofit-scan` skill for `retrofitting`, `/dev-tools:session-write-handover` for `session-continuity`).
- Reminder that the workspace is **data** — they can delete/move it freely without losing the plugin's commands.

## Notes

- The scaffold path must be resolved via `${CLAUDE_SKILL_DIR}/../../template/` (not `${CLAUDE_PLUGIN_ROOT}` — that variable isn't exported in skill bash injection, only in hooks/MCP).
- Never copy `.claude/commands/`, `.claude/agents/`, or `.claude/skills/` into the new workspace.
- Don't hard-code any personal paths or identifiers here — everything comes from user memory or prompts.
