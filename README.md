# skillful-store

Content repository for `skillful` store items.

This repo contains only store content (agents, commands, skills, providers, MCPs). The `skillful` app consumes these files by cloning the repo into a local cache and indexing `store/`.

## Layout

```text
skillful-store/
  store/
    agents/
    commands/
    skills/
    providers/
    mcps/
```

## Source Contract

- Root must include `store/`.
- Identity is derived from filenames/folder names per category:
  - agent: `store/agents/<id>.md`
  - command: `store/commands/<id>.md`
  - skill: `store/skills/<name>/SKILL.md` (frontmatter `name` should match folder)
  - provider: `store/providers/<id>.json`
  - mcp: `store/mcps/<id>.json`

## Notes

- `_meta` is required in provider and MCP JSON files and is stripped at install time.
- If multiple sources define the same `(type,id)`, `skillful` uses the highest-priority source.
