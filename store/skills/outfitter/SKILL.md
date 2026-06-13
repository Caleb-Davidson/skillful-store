---
name: outfitter
description: Scaffold a new Tauri + React + Vite game project — 2D (Phaser 3) or 3D (Three.js) — with the locally-installed `outfitter` CLI. Use this whenever the user wants to start, bootstrap, scaffold, create, or spin up a new game project, game prototype, or fresh Tauri/Phaser/Three.js app, even if they don't say "outfitter" by name. Always drive it non-interactively with `--yes` and flags. For new projects only — it does not modify existing ones.
license: MIT
targets: [opencode, claude-code]
metadata:
  audience: developers
  domain: game-dev
  platform: node
---

# outfitter — scaffold a new game project

`outfitter` is a command-line tool that creates a new Tauri 2 + React 19 +
Vite + TypeScript game project from a fixed set of conventions. Every project
it generates ships the same opinionated baseline: strict TypeScript, an
ESLint + Stylelint + PurgeCSS lint stack run from a Husky pre-commit hook,
NSIS-only Windows packaging, a 1920×1080 window, itch.io-safe asset loading,
and a `docs/` system managed by the `documenter` CLI.

Each project is one of two styles:
- **2D** — Phaser 3 (scenes, sprites, Arcade physics).
- **3D** — Three.js (scene graph, fixed-timestep simulation).

Reach for this skill when someone wants to *start* a new game or prototype.
It does not modify, migrate, or add to existing projects.

## Before you run it

`outfitter` is a private CLI installed per-machine (it is not published to npm).
Two things must be true, and it's worth checking both up front:

1. **`outfitter` is on PATH** — verify with `outfitter --version`.
2. **`documenter` is on PATH** — verify with `documenter --version`. outfitter
   shells out to `documenter init` and hard-fails at startup if it's missing.
   This is a deliberate, non-skippable requirement.

If either is missing, the user installs it by cloning the relevant repo and
running `npm install && npm link` inside it (ask the user where the repos live
if you need to). Don't try to work around a missing tool — stop and report it.

## How to run it (always non-interactively)

`outfitter init` is the only command that scaffolds. By default it asks
interactive questions — **but as an agent you cannot answer prompts, so always
pass `--yes` with the answers as flags.** That is exactly what the flag
interface is for: running start-to-finish with no human at the keyboard.

The project is created at `<current-working-directory>/<name>`, so run from the
directory where the new project folder should live. The target directory must
not already exist.

In `--yes` mode only `--name` and `--style` are required; everything else has a
sensible default.

```sh
# Minimal:
outfitter init --yes --name my-game --style 2d

# Full (one line; the agent adapts to the shell):
outfitter init --yes --name spire --style 3d --description "A roguelike prototype." --narrative --remote http://gitea.example/me/spire --identifier com.example.spire
```

### Flags

| Flag | Meaning |
|---|---|
| `--name <name>` | **Required.** Project + directory name. Lowercase letters, digits, and dashes only; must start with a letter or digit. |
| `--style <2d\|3d>` | **Required.** `2d` = Phaser 3, `3d` = Three.js. |
| `--description <text>` | Goes into `package.json` and the generated `AGENTS.md`. Default: "A small game prototype." |
| `--narrative` / `--no-narrative` | `--narrative` adds a `plot/` story skeleton and wires it into `opencode.json`. Default: off. |
| `--remote <url>` | Git remote to add as `origin` and push the first commit to (Gitea, GitHub — anything `git push` understands). Default: no remote, no push. |
| `--identifier <id>` | Tauri bundle identifier. Default: `com.outfitter.<name>` (dashes removed). |
| `-y, --yes` | Non-interactive mode. Required for agent use. |
| `-h, --help` | Print the full flag list. |

If unsure the flag set is current, run `outfitter init --help`.

## What to confirm before scaffolding

Scaffolding is mildly consequential — it creates a directory, runs `npm
install`, makes a git commit, and (with `--remote`) pushes. So:

1. Confirm the **name** and **style** with the user; both are required and not
   trivially changed afterward.
2. Treat it as a **narrative** project only if the user says so — default is no.
3. Pass `--remote` only with a URL the user actually provides. Pushing is
   outward-facing; never invent a remote.

Then run the single `outfitter init --yes …` command. It performs several slow
steps in sequence (template copy → `documenter init` → `npm install` → git), so
expect it to take a minute or more — let it finish rather than interrupting.

## After it finishes

outfitter prints next steps and the generated project is self-contained (it no
longer depends on outfitter). Typical follow-ups, from inside the new project:

```sh
cd <name>
npm run dev          # web preview (Vite)
npm run tauri:dev    # desktop (Tauri)
```

If a `--remote` push fails, outfitter reports it as a note but leaves the local
repo committed and intact — the user can add the remote and push later.

## Gotchas

- **Never omit `--yes` in an agent context** — the command will hang on the
  first prompt.
- **The target directory must not exist.** If it does, outfitter exits with an
  error; choose a different name or run from a different directory.
- **New projects only.** There is no update / refresh / add-to-existing mode.
- **The name pattern is strict:** lowercase letters, digits, and dashes,
  starting with a letter or digit — `My_Game` and `MyGame` are rejected.
