# Quick Repo Spin-Up Bash Alias

A one-command workflow to create a new GitHub repo at `~/repos/github`, clone it, `cd` in, and launch Claude Code.

## Install

Append to `~/.bash_aliases`:

```bash
# nr = new repo
# usage: nr <name> [pub|priv]   (default: priv)
# - normalizes <name> to This-Case-Structure
# - creates on GitHub via gh, clones to ~/repos/github, cds in, launches claude
nr() {
  if [ -z "$1" ]; then
    echo "usage: nr <name> [pub|priv]" >&2
    return 1
  fi
  local name
  name=$(echo "$1" | sed -E 's/[ _]+/-/g' \
    | awk -F- '{for(i=1;i<=NF;i++)$i=toupper(substr($i,1,1))substr($i,2)}1' OFS=-)
  local vis=${2:-priv}
  if [ "$vis" = "pub" ]; then vis=public; else vis=private; fi
  local base="${REPOS_GITHUB_DIR:-$HOME/repos/github}"
  cd "$base" && \
    gh repo create "$name" --"$vis" --clone && \
    cd "$name" && \
    claude
}
```

Reload: `source ~/.bash_aliases` (or open a new shell).

## Usage

```bash
nr my-new-tool          # -> My-New-Tool (private)
nr "my new tool" pub    # -> My-New-Tool (public)
nr data_pipeline priv   # -> Data-Pipeline (private)
```

## What it does

1. Normalizes the name: spaces/underscores → hyphens, Title-Cases each segment (`This-Case-Structure`).
2. Defaults visibility to **private**; pass `pub` for public.
3. `gh repo create --clone` creates on GitHub and clones into `~/repos/github/<Name>`.
4. `cd`s into the new repo.
5. Launches `claude` (Claude Code) in the fresh working directory.

## Requirements

- `gh` CLI authenticated (`gh auth status`)
- `claude` on `PATH`
- `~/.bash_aliases` sourced by `~/.bashrc` (Ubuntu default)
