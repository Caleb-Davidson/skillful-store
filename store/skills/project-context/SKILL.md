---
name: project-context
description: The repo-specific facts the feature-team agents need — package manager and commands, the quality gate, the architecture and dependency rules, the language/type rules, the area labels, and pointers to the convention docs. This skill is what makes the shared, frozen agent definitions concrete for a repo; each project fills in its own version. Loaded automatically by the coder, tester, reviewer, and doc-keeper agents.
---

# Project Context — BLANK STARTER

<!--
  This is the STARTER shipped by skillful. After installing it into a repo, fill in every section
  below with that repo's real values, then COMMIT it. skillful `install` overwrites this file, so if
  a re-install ever clobbers your filled-in copy, restore it with `git checkout` — it's committed.

  This file is the per-repo half of the agent team. The agent definitions (coder, tester, reviewer,
  doc-keeper) are shared and frozen; they defer every project-specific fact to this file. Keep it
  accurate and low-churn.

  Delete this comment block once the file is filled in.
-->

## Package manager & commands

- **Package manager:** TODO (e.g. `pnpm` / `npm` / `yarn`).
- **Targeted tests** (coder/tester red→green loop): TODO — the single-file/focused test command.
- **The quality gate** (lead runs it end-of-phase): TODO — the full gate command and what it covers.
- **Docs lint** (doc-keeper), if any: TODO.
- Full command list is in `AGENTS.md` → Commands.

## Work tracker

- Issues live on Gitea repo **`TODO owner/name`**.
- Read: TODO — the tracker's list/details/claim command.

## Area labels (for issue-triage)

TODO — the repo's area labels (e.g. `core`, `app`, `docs`). Multi-label where cross-cutting; a phase
umbrella that fits no area carries no area label.

## Architecture & dependency rules (fixed — don't change unless the plan says so)

- TODO — the repo's core architectural invariants and the one-way dependency rule.
- TODO — any purity / boundary constraints enforced at compile time.
- TODO — scope constraints (e.g. read-only), if any.
- Canonical detail: TODO — the architecture/requirements doc(s).

## Language & type rules (the ones agents most often slip on)

- TODO — the hard language rules for this repo (e.g. "no `as`/`any`, narrow from `unknown`"), or
  "None beyond the coding conventions" if the repo has no extra hard rules.
- Full rules: TODO — the coding-conventions doc (loaded via `AGENTS.md`).

## Convention docs (the standards the agents enforce)

- **Coding:** TODO
- **Unit testing:** TODO
- **Reviewing:** TODO
- **Documentation style:** TODO

### Docs the doc-keeper reads first

- TODO — the docs contract / documentation-architecture files, if the repo has them.

Decision policy: TODO — where architectural decisions are recorded (an ADR dir, or "no ADRs; decisions
live in AGENTS.md + commit history").

## Project-specific workflow steps

TODO — anything the shared `feature-workflow` doesn't cover: artifact regeneration (manifests,
codegen), a dogfood pass, an end-to-end smoke test, teardown exceptions. Write "None beyond the shared
`feature-workflow`." if there are none.
