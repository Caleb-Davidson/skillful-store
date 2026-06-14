---
name: manage-gitignore
description: Creates or updates .gitignore using Toptal templates while preserving custom entries. Use when asked to create, generate, update, or refresh a .gitignore file.
license: MIT
metadata:
  audience: developers
  domain: git
  platform: universal
---

## Inputs

The user may provide optional context about technologies, tooling, or frameworks as arguments. If no context is provided, infer templates by scanning the project.

## Mission

1. Determine whether a `.gitignore` file already exists.
2. Build a suitable template list:
   - If the user supplied context, use it to choose templates.
   - If no context was supplied, scan the project to infer relevant technologies and tooling.
3. Use Toptal's gitignore API to build the generated content.
4. Preserve any existing custom ignore rules outside the auto-generated section.

## Required commands

- List available templates:
  - `curl -L -s https://www.toptal.com/developers/gitignore/api/list`
- Generate template content:
  - `curl -L -s https://www.toptal.com/developers/gitignore/api/node,unity,vscode > .gitignore`

## Update strategy

- If no `.gitignore` exists:
  - Generate one from the selected templates.
- If `.gitignore` exists:
  - Read it first.
  - Preserve custom rules that were added outside the auto-generated section.
  - Replace or refresh only the auto-generated section using newly selected templates.
  - Ensure preserved custom content is not lost.

## Output expectations

- Report which templates were selected and why.
- Confirm whether `.gitignore` was created or updated.
- Confirm that pre-existing custom ignore rules were preserved.
