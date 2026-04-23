---
name: memory-repository-workflow
description: Manages repository knowledge storage and agent memory workflows. This skill MUST be active when storing, retrieving, or updating shared repository knowledge. It directs memory storage to `./memories/repo/` as version-controlled Markdown files instead of external per-agent memory tools, ensuring knowledge travels with git and is shared with both future agents and human reviewers. Activate this skill when the user asks to save repository knowledge ("remember this about the project", "record the build command", "save this to repo memory", "note this for next time"), when the agent needs to recall project conventions, when creating or updating documentation files, or any situation involving shared workspace/team knowledge. This overrides any guidance to use external memory tools for repository knowledge.
---

# Memory Repository Workflow

This skill defines how agents store and retrieve shared repository knowledge. Repository knowledge must travel with version control so it is auditable, reviewable, and available to every future agent and human contributor — not locked inside a single agent's private memory store.

## Core rule

**Repository knowledge goes in `./memories/repo/` as version-controlled Markdown files.**

Do not use external memory tools (such as `memory` APIs or Copilot per-user memory) for facts that should be shared through the repository. External memory is per-agent and volatile; repo memory survives, is diff-reviewable, and travels with the codebase.

## What belongs in `./memories/repo/`

- Codebase conventions (naming, layering, patterns)
- Project structure facts
- Verified build, test, and deployment commands
- Investigation results worth preserving
- Workflow rules and process decisions
- Any durable facts meant to be shared with future agents

## What does NOT belong

- Temporary scratch notes for the current task
- One-off investigation results that will not be reused
- Per-user preferences or credentials (use environment config instead)

## Language

Write all files under `./memories/repo/` in **English**, matching the skill ecosystem convention. English keeps the knowledge base searchable by any contributor or tool and avoids mixed-language files when multiple people collaborate. If a fact was originally spoken in another language by the user, translate it into concise English technical prose when recording it.

## Workflow

### Before creating a new knowledge file

1. List `./memories/repo/` to see what already exists
2. If a relevant file exists, update it rather than creating a duplicate
3. If the directory does not yet exist, create it

### When recording knowledge

1. Pick the topic file that best fits (or create a new focused one)
2. Keep entries concise: bullet points, direct rules, verified commands
3. Organize by topic so the folder stays scannable
4. Prefer small focused files over a single sprawling notes file

### When knowledge becomes obsolete

- Correct or remove stale entries the moment you notice them
- Do not leave outdated guidance that could mislead future agents or humans

## Directory structure

```
./memories/repo/
├── build-workflow.md       # Build and test commands
├── code-conventions.md     # Coding style and patterns
├── project-structure.md    # Directory layout and purposes
├── workflow-rules.md       # Process and procedure rules
└── investigation-results/  # Organized by topic
    ├── api-analysis.md
    └── performance-notes.md
```

## Example — a well-formed knowledge file

`./memories/repo/build-workflow.md`:

```markdown
# Build Workflow

## Install
- `pnpm install` (uses pnpm, not npm — lockfile is `pnpm-lock.yaml`)

## Build
- `pnpm run build` — runs tsc then bundles with esbuild
- Output lands in `dist/`

## Test
- `pnpm test` — vitest, watch mode off by default
- `pnpm test:watch` — watch mode for local iteration

## Notes
- Node 20+ required (verified 2025-04-15)
- Do not commit `dist/` — it is generated
```

Compare this to a bad entry:

```markdown
# stuff
- build things with pnpm probably
- tests exist somewhere
```

The good version is specific, verified, and directly actionable; the bad version forces the next reader to rediscover everything.

## Correct vs. incorrect usage

**Correct**
- Save build workflow to `./memories/repo/build-workflow.md`
- Update `./memories/repo/code-conventions.md` with a new pattern
- Split a growing file into topic-specific subfiles under `investigation-results/`

**Incorrect**
- Store authoritative conventions only in an external per-agent memory tool
- Scatter repo-level knowledge across random unrelated folders
- Use temporary scratch notes as a substitute for repo knowledge

## Integration with other skills

This skill is consulted alongside others. When another skill produces a durable fact that future agents should know (a newly verified command, a structural decision, a convention), invoke this skill's guidance to persist it in `./memories/repo/`.
