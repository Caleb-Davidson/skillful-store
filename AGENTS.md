# AGENTS

Guidance for contributors editing this store repository.

## Required Structure

- All installable content lives under `store/`.
- Supported categories:
  - `store/agents/*.md`
  - `store/commands/*.md`
  - `store/skills/<name>/SKILL.md`
  - `store/providers/*.json`
  - `store/mcps/*.json`

## Conventions

- Keep IDs stable. Renaming files/folders changes item identity.
- Prefer additive changes; avoid replacing existing IDs unless intentionally overriding behavior.
- Skills should keep `name` in frontmatter aligned with directory name.
- Providers/MCPs must include `_meta.description`; `_meta.tags` is optional.
