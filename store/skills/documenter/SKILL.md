---
name: documenter
description: >-
  Scaffold and maintain a repo-local, statically-hostable Markdown documentation
  system in any project using the locally-installed `documenter` CLI (commands:
  init, update, lint). Use this skill whenever the user asks to add browsable docs
  to a repository, bootstrap or set up project documentation, refresh an existing
  documenter-managed docs system, lint docs, or mentions "documenter", a docs/
  shell, docs templates, or a documentation manifest — even if they don't name the
  tool explicitly.
license: MIT
targets: [opencode, claude-code]
metadata:
  audience: developers
  domain: documentation
  platform: node
---

# documenter — repo-local docs scaffolding

## What this is

`documenter` is a locally-installed CLI (private, distributed by `git clone` +
`npm link`) that drops a self-contained Markdown documentation system into any
project and then keeps it up to date. What lands in a target project:

- `docs/index.html` + `docs/assets/` — a static, browser-viewable docs shell
  (vendored `markdown-it` / `dompurify` / `js-yaml`, no build step, no server).
- `docs/index.md` — the navigation manifest the user curates. Its frontmatter
  `title` drives the shell's sidebar heading (defaults to "Project
  Documentation"; set it up front with `init --title`).
- Three governing docs (`documentation-architecture.md`,
  `documentation-md-contract.md`, `documentation-style-guide.md`) and seven
  copyable page templates under `docs/templates/`.
- `.documenter.json` — a state file recording the SHA-256 of every file
  documenter wrote, so later updates can tell stock files from user edits.
- A `docs:lint` script added to the project's `package.json`.

The heavy dependencies and the linter live *inside* documenter — target projects
get zero devDependencies to manage.

## Before you run anything

Confirm the CLI is available: `documenter --version`. If that errors with
"command not found", it isn't installed on this machine — it's a private CLI
distributed by cloning its repo and running `npm install` then `npm link` inside
it (requires Node 20+).

documenter acts on the current directory by default. Use `--cwd <path>` to point
it at another project.

## The three commands

| Command | What it does | When to use |
|---|---|---|
| `documenter init` | Scaffolds `docs/`, writes `.documenter.json`, adds the `docs:lint` script (creating a minimal `package.json` if none exists). Skips files that already exist. Takes `--title` to set the docs site title. | Bootstrapping docs in a project for the first time. |
| `documenter update` | Refreshes the platform files (the docs shell, assets, templates, governing docs) to the CLI's current version, skipping any file the user has modified. | Pulling in documenter improvements after the CLI itself is upgraded. |
| `documenter lint` | Runs documenter's internal linter against the project's `docs/`, validating each page against the Markdown contract. | Verifying docs structure; also wired as `npm run docs:lint`. |

Shared flags: `--cwd <path>` (all commands) and `--force` (`init`/`update`).

`init` also takes `--title <text>` to set the docs site title (the sidebar
heading), e.g. `documenter init --title "My Project Docs"`. If you omit it, the
title defaults to "Project Documentation" and `init` reminds you to set it. You
can pass any text, including titles with punctuation like colons.

To change the title later, edit the `title` field in `docs/index.md`. Rerunning
`init --title` won't do it — `init` never touches an existing `docs/index.md`.
Once set, `documenter update` preserves your title and won't overwrite it.

## Why `update` is safe (the drift model)

Every managed file is hashed. On `update`, documenter compares the on-disk file
against both what it last wrote (from `.documenter.json`) and the new version it
ships, then classifies:

- **added** — new file in this CLI version → copied in.
- **current** — already matches the latest → left alone.
- **stock-old** — untouched since documenter wrote it, but a newer version
  exists → updated.
- **drifted** — the user edited it → **skipped with a warning**, never silently
  overwritten. Pass `--force` to overwrite drifted files anyway.

This is why `documenter update` is safe to run anytime: it won't clobber
hand-edited docs. It's also why `.documenter.json` should be **committed** — it's
the record that makes drift detection work across machines and over time.

## Typical workflow in a target project

1. `documenter init --title "My Project Docs"` (the title becomes the sidebar
   heading; omit `--title` to set it later in `docs/index.md` frontmatter).
2. Curate `docs/index.md` so the nav lists the pages that matter.
3. Author pages by copying from `docs/templates/`, following the rules in
   `docs/documentation-md-contract.md`.
4. `documenter lint` (or `npm run docs:lint`) to validate structure.
5. Open `docs/index.html` in a browser to preview.
6. Later, after the documenter CLI is upgraded, run `documenter update` to pull
   platform improvements without touching your content.

For *how to write* the docs, defer to the three governing docs that `init` drops
into `docs/` — they're the source of truth for the platform, the page contract,
and the writing style.

## Gotchas

- **`init` never overwrites existing files** — it skips them (and leaves them
  untracked in state). Use `--force` only when you intend to overwrite.
- **Don't hand-edit `.documenter.json`** — it's generated; editing it corrupts
  drift detection.
- **`lint` needs `docs/` to exist** — run `init` first.
- **The target project needs no dependencies installed** for `lint` to work;
  documenter resolves its own `node_modules`.
