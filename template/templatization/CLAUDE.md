# Templatization Workspace

This workspace is provisioned by the `dev-tools` plugin (variant: `templatization`). Use it to extract commonalities from a set of existing repositories and produce a reusable GitHub template repo.

## Primitive

- `/dev-tools:templatize-workspace` — analyses source repos, extracts shared structure, and emits a template.

## Layout

- `sources/` — list or clones of the source repositories to compare.
- `analysis/` — diff output, shared-file manifests, variant notes.
- `template-draft/` — the emerging template scaffold (published to GitHub when ready).
- `context/` — naming conventions, license choice, target audience.

## Usage

1. List source repos in `sources/sources.md` (URLs or local paths).
2. Invoke `/dev-tools:templatize-workspace` — it populates `analysis/` and drafts `template-draft/`.
3. Review, adjust, then push `template-draft/` as a new GitHub template repo.
