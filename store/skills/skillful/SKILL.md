---
name: skillful
description: >-
  Manage AI coding-tool config — agents, commands, skills, providers, MCP
  servers, configs — and their git sources via the locally-installed `skillful`
  CLI's non-interactive, JSON-only command surface (OpenCode, Claude Code,
  Codex). Use whenever the user wants to install, remove, list, or inspect these
  items, or add / update / check a source — e.g. "add the pdf skill", "install an
  agent into this repo", "set up an MCP", "what skills are available" — even if
  they don't say "skillful" by name. Use this instead of skillful's interactive TUI.
---

# skillful — manage AI-tool config from sources

`skillful` installs reusable AI-tool configuration — **agents, commands, skills,
providers, MCP servers, and configs** — from git **sources** into one or more
**targets** (`opencode`, `claude-code`, `codex`), at **global** or **project**
scope. It has an interactive TUI for humans; this skill is its **Agent CLI** — a
non-interactive surface that prints one JSON object per call with stable exit
codes, for scripted or agent-driven use.

## Before you run anything

Confirm the CLI is available: `skillful version` (prints `{"ok":true,...}` and
exits 0). If that errors with "command not found", it isn't installed on this
machine — it's a private CLI installed by cloning its repo and running `npm run
setup` (which installs deps, builds, and `npm link`s it; Node 22+). Don't work
around a missing tool — stop and report it.

The surface is self-describing: **`skillful schema`** returns the full catalog
(every command, its usage, flags, and the exit/error-code table) as JSON. Treat
that as the source of truth — it can't go stale the way this page can.

## Grammar

```
skillful <verb> <noun> [id] [flags]
```

Output is always JSON on stdout. The noun is **plural to list** a collection
(`list skills`) and **singular to act on one** (`install skill <id>`).

## Sources — the git repos that supply items (no scope/target)

A single global registry, so these take no `--scope`/`--target`:

| Command | Does |
|---|---|
| `list sources` | list all configured sources |
| `add source <git-url> [--id --name --branch]` | clone + index a new source |
| `remove source <id>` | drop a source |
| `enable source <id>` / `disable source <id>` | toggle participation |
| `reorder source <id> --direction up\|down` | change merge priority |
| `update source <id>` | git fetch + reindex one source |
| `check sources` / `check source <id>` | read-only "is there an update?" check |

## Items — install/inspect content (require --scope AND --target)

Four verbs over six types (`agent command skill provider mcp config`):

```
skillful list skills          --scope <s> --target <t>
skillful info skill <id>      --scope <s> --target <t>
skillful install skill <id>   --scope <s> --target <t> [--dry-run]
skillful remove skill <id>    --scope <s> --target <t> [--dry-run]
```

Item commands **never guess** scope or target — deliberately, so nothing is
written to the wrong place. Always pass:

- **`--scope global|project`** — required, no default.
- **`--target <id>`** — required; `opencode`, `claude-code`, or `codex`.
  Repeatable (`--target opencode --target claude-code`) to act across several.
- **`--project <path>`** — optional with `--scope project`; defaults to the
  current dir, which must be a real project (`.git`/`.opencode`) or you get
  `NOT_A_PROJECT`. The resolved path is echoed back.
- **`--dry-run`** — on install/remove, report what *would* change without writing.

## Output contract

One envelope per call, success or failure:

```jsonc
{ "schemaVersion": "1", "ok": true, "command": "install skill",
  "data": { /* result, present when ok */ }, "warnings": [], "error": null }
```

On failure, `ok:false` and `error: { code, message, details? }`. Branch on the
exit code, then read `error.code`:

| Exit | Meaning | Example codes |
|---|---|---|
| 0 | success (incl. idempotent no-ops) | — |
| 1 | usage / validation | `MISSING_SCOPE`, `MISSING_TARGET`, `INVALID_TARGET`, `NOT_A_PROJECT` |
| 2 | not found | `ITEM_NOT_FOUND`, `SOURCE_NOT_FOUND` |
| 3 | operation failed | `NOT_ELIGIBLE`, `PARTIAL_FAILURE` |

Install/remove results echo `changedTargets` and `skipped` — report what those
say happened, not what you assumed would.

## Gotchas

- **Idempotent where safe:** re-`install` overwrites; `remove` of an absent item
  succeeds with `changed:false`; re-`add source` is a no-op. But a typo'd id
  (`remove source <unknown>`) fails with `SOURCE_NOT_FOUND`.
- **Not every target supports every type** (Claude Code has no providers; Codex
  has no commands). Ineligible requested targets land in `skipped`; if none are
  eligible you get `NOT_ELIGIBLE`. Use `--dry-run` first when unsure.
- **Unsure of a command or flag?** Run `skillful schema` rather than guessing.
