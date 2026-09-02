# ecosystem-docs

The single source of truth for the design docs shared across every project in this
`base-scaffold`/`APP-DESIGN.md` ecosystem, plus the org-wide app registry (`APPS.md`,
`BASE-DESIGN.md` §11.3).

## Why this exists

`APP-DESIGN.md`, `BASE-DESIGN.md`, `INTEGRATION-GUIDE.md`, `CLAUDE-CODE-GUIDE-APP.md`, and
`CLAUDE-CODE-GUIDE-BASE.md` used to be copy-pasted, byte-identical, into every project's own
`docs/` — `appkit`, `base-scaffold`, `hjtdev-django-cleanup`, and every app package after them.
Editing one meant remembering to hand-copy the same edit into every other clone, or watching them
silently drift. This repo is the one place those five files live; every project links to them
instead of holding its own copy.

Each project keeps its own genuinely project-local docs in its own `docs/` — `CONTRACT.md`,
`SECURITY-CHECKLIST.md`, a project's own `CLAUDE-CODE-GUIDE-APP-<name>.md`, and so on. Only the
five files above move here.

## Setup — one time, per machine

Clone this repo as a **sibling** of every project that uses it — the symlinks below are relative
and assume that layout:

```
~/Projects/ecosystem-docs/     <- this repo
~/Projects/appkit/
~/Projects/base-scaffold/
~/Projects/your-app-here/
```

```bash
cd ~/Projects
git clone https://github.com/HjtDev/ecosystem-docs.git
```

Then, in any project that wants the shared docs synced:

```bash
cd ~/Projects/your-project
make docs-link
```

This creates five relative symlinks under `docs/` — `docs/APP-DESIGN.md ->
../../ecosystem-docs/APP-DESIGN.md`, and so on — replacing whatever local copies were tracked
there. Run it again any time; it's idempotent. See a project's own `Makefile` for the exact
target, and its `CLAUDE.md` for the "edit here, never locally" convention this depends on.

## Editing

Edit the file here, once. Every project with `make docs-link` already run sees the change
immediately — no re-copy, no PR fan-out. If you're an agent working inside one of the linked
projects and about to edit `docs/APP-DESIGN.md` (or any of the other four), stop: you're editing
this repo's file through the symlink. Make the edit here instead, in a checkout of
`ecosystem-docs` itself, or the change is easy to lose track of.

## Adopting this pattern in your own ecosystem

Nothing here is specific to `HjtDev`'s projects beyond `APPS.md`'s own entries. Clone this repo,
replace `APPS.md`'s rows with your own app packages, and point your own projects' `docs-link`
targets at your clone instead.

## Contents

| File | What it is |
|---|---|
| `APPS.md` | The app-package registry — `BASE-DESIGN.md` §11.3 |
| `APP-DESIGN.md` | How a single versioned app package is built |
| `BASE-DESIGN.md` | How the host monorepo scaffold is built |
| `INTEGRATION-GUIDE.md` | How an app package installs into a host — the bridge between the two above |
| `CLAUDE-CODE-GUIDE-APP.md` | Generic build-order guide for a new app package, phase by phase |
| `CLAUDE-CODE-GUIDE-BASE.md` | Generic build-order guide for the base scaffold itself |
