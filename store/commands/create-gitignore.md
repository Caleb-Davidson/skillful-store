---
description: Creates or updates .gitignore using Toptal templates while preserving custom entries.
---

You are tasked with creating or updating the repository's `.gitignore` file.

## Inputs
- The user may provide optional inline context about technologies, tooling, or frameworks in this section.
- If no extra context is present, infer templates by scanning the project.
User Input:
$ARGUMENTS

## Mission
1. Determine whether a `.gitignore` file already exists.
2. Build a suitable template list:
   - If the prompt includes user context, use it to choose templates.
   - If the prompt does not include user context, scan the project to infer relevant technologies and tooling.
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
