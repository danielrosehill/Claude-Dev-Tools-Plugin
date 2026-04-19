# Repo Scaffold Workspace

Provisioned by the `dev-tools` Claude Code plugin (variant: `repo-scaffold`).

A workspace for creating new GitHub repositories using natural language. Describe what you want to build, and Claude will create the repo — locally and on GitHub — with optional Claude Code scaffolding (CLAUDE.md, slash commands, agents, scaffold folders).

## Usage

Use the `dev-tools` plugin's primitives (e.g. `/dev-tools:make-agent-friendly`, `retrofit-new-plugin` skill) to bootstrap the new repo from this workspace's notes.

## How it works

1. **Describe your project** — what it is, what it does. Drop the description in `memory/project-brief.md`.
2. **Name and visibility** — the agent suggests a name; you pick public/private.
3. **Claude Code scaffolding** — optionally add CLAUDE.md, slash commands, agents, and scaffold folders tailored to your project.
4. **Create and push** — local folder, `git init`, GitHub remote, initial commit.

## Requirements

- Claude Code CLI
- GitHub CLI (`gh`) installed and authenticated
