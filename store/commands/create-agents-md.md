---
description: Inspect a repository and generate a tailored AGENTS.md file that helps future AI agents understand the project structure, commands, architecture, and rules.
---

You are an AI coding agent working inside my project repository.

Your task is to review the current project and create a new `AGENTS.md` file at the repository root. This file should help future AI agents understand how to work safely and effectively in this codebase.

Do not invent details. Inspect the repository first, then populate the file from the actual project structure, package scripts, docs, configuration files, source layout, and conventions you observe.

## What to inspect first

Review, at minimum:

- `README.md`
- `package.json`, `tsconfig.json`, build configs, lint configs, test configs, or equivalent project config files
- Source folders and their responsibilities
- Existing documentation folders such as `docs/`, `documentation/`, `design/`, `architecture/`, `plot/`, `specs/`, or similar
- Build scripts, dev scripts, test scripts, validation scripts, and asset pipeline scripts
- Any framework-specific structure, such as Unity scenes, React app structure, Vite/Tauri setup, backend services, game scenes, API routes, etc.
- Any existing conventions in comments, docs, folder names, architecture docs, or code organization

## Project Description guidance

Before writing the `## Project Description` section, look for a high-level summary of the project's purpose in:

- `README.md` (introduction or overview section)
- `package.json` or `pyproject.toml` `description` field
- Any `docs/`, `design/`, or `specs/` files that describe what the product is
- Marketing copy, landing page copy, or pitch documents if present

If you can confidently summarize what the project is, who it is for, and what it produces or enables from these sources, write the section without asking.

If the repository gives you no meaningful signal on purpose or audience, ask the user one question before proceeding:

> "I couldn't find a clear project description in the repository. Can you briefly describe what this project is and what it's for? (1–3 sentences is enough.)"

Use their answer to write the description. Do not ask for more than what you need.

## Output requirement

Create or replace `AGENTS.md` at the repository root.

The document should be specific, practical, and useful for future AI agents. It should explain:

1. What technologies the project uses.
2. Where important code lives.
3. Which commands agents should run.
4. Which documentation should be read before modifying systems.
5. Any runtime, scene, app, service, or module structure that matters.
6. Build targets or deployment targets, if applicable.
7. Project rules and architectural boundaries agents must respect.

Use the following structure as a template. Populate it with this project's actual details.

---

# AGENTS.md Template

````md
# AGENTS.md

## Project Description

[1–3 paragraphs describing what this project is and what it is for. Scale to project size: one paragraph for small or single-purpose projects, up to three for large multi-system projects. Focus on the business or product purpose — what problem it solves, who uses it, and what it produces or enables. Avoid technical implementation details here; those belong in the sections below.]

## Technologies Used

- **[Technology Name]**: [What this technology is used for in this project.]
- **[Technology Name]**: [What this technology is used for in this project.]
- **[Technology Name]**: [What this technology is used for in this project.]

## Project Structure

- `[path/]`: [What belongs here and what future agents should know about it.]
- `[path/]`: [What belongs here and what future agents should know about it.]
- `[path/]`: [What belongs here and what future agents should know about it.]

## Commands

```bash
[command]                 # [What this command does and when to run it]
[command]                 # [What this command does and when to run it]
[command]                 # [What this command does and when to run it]
```

Include commands for:

* Starting local development
* Building the project
* Running tests
* Type checking
* Linting or formatting
* Asset pipelines or code generation
* Platform-specific builds, if applicable

Only include commands that actually exist or are clearly supported by the repository.

## Architecture Hub

**Important Documentation:**
Before working on any system, you **MUST** load and read the relevant architecture, contract, standard, or design document. These documents explain the "whats" and "whys" of the implementation and should be treated as the source of truth.

* **[Document Title](path/to/document.md)**: [What this document explains and when an agent should read it.]
* **[Document Title](path/to/document.md)**: [What this document explains and when an agent should read it.]
* **[Document Title](path/to/document.md)**: [What this document explains and when an agent should read it.]

If the project has no architecture docs yet, say so clearly and list the most relevant source files or folders that currently act as the source of truth.

## Runtime / Application Structure

Describe the main runtime structure of the app or game.

Examples:

* Main scenes, screens, routes, services, systems, or modules
* Startup flow
* Ownership boundaries between UI, state, engine, backend, data, or platform layers
* Important lifecycle rules
* How major systems communicate

Use a numbered list if there is a clear runtime order:

1. **[Entry Point / Scene / Module]** - [Responsibility]
2. **[Next Layer]** - [Responsibility]
3. **[Main Runtime Area]** - [Responsibility]

If this does not apply, replace this section with a more appropriate project-specific section, such as:

* `## Service Structure`
* `## Package Structure`
* `## Backend Structure`
* `## Frontend Structure`
* `## Game Structure`
* `## Data Flow`

## Build Targets

* **[Target]**: [Build output path, deployment target, or packaging notes.]
* **[Target]**: [Build output path, deployment target, or packaging notes.]

If the project has no distinct build targets, omit this section.

## Rules

* [Concrete architectural rule agents must follow.]
* [Concrete architectural rule agents must follow.]
* [Concrete architectural rule agents must follow.]
* [Concrete architectural rule agents must follow.]

Rules should be specific to this project. Prefer practical instructions like:

* Do not put UI state in engine/gameplay classes.
* Do not bypass the central state store.
* Do not scatter data definitions outside the approved registry.
* New assets must be registered in the loader before use.
* Read the relevant architecture doc before changing a system.
* Keep generated files out of manual edits.
* Run type checks before completing changes.

## Agent Workflow

When making changes:

1. Read any relevant architecture or standard docs listed above.
2. Inspect the existing implementation before editing.
3. Preserve the existing architecture unless the user explicitly asks for a redesign.
4. Update docs when behavior, architecture, commands, or conventions change.
5. Run the relevant validation commands before reporting completion.

## Notes for Future Agents

* [Any project-specific gotchas.]
* [Any generated files that should not be edited directly.]
* [Any fragile integrations.]
* [Any naming, style, or folder conventions.]

````

---

## Writing guidelines

- Keep the file concise but complete.
- For **Project Description**: write 1 paragraph for small/single-purpose projects, up to 3 for large multi-system ones. Focus on what the project is, who it serves, and what it produces — not how it is built.
- Prefer concrete project facts over generic advice.
- Do not include sections that are empty or irrelevant.
- Do not hallucinate architecture documents. Only list docs that actually exist.
- If something is unclear, infer cautiously from the repository and mark it as uncertain.
- If the repository has a docs style guide, markdown contract, or documentation standard, follow it.
- If there are existing AI-agent instructions, merge them rather than replacing important guidance.
- Use present tense.
- Avoid long code examples unless they are necessary.
- Make the resulting `AGENTS.md` immediately useful for a future coding agent.

## Final response

After creating `AGENTS.md`, summarize:

- The file path created.
- The main project facts you captured.
- Any uncertainty or missing docs you noticed.
- Which validation commands, if any, you ran.