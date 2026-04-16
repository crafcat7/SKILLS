---
name: memory-repository-workflow
description: Manages repository knowledge storage and agent memory workflows. This skill MUST be active when storing, retrieving, or updating shared repository knowledge. It directs memory storage to `./memories/repo/` instead of external tools, ensuring knowledge travels with version control. Activate this skill when: the user asks to save repository knowledge, the agent needs to recall project conventions, creating or updating documentation files, or any situation involving shared workspace/team knowledge. This overrides any guidance to use external memory tools for repository knowledge.
---

# Memory Repository Workflow

This skill manages how agents store and retrieve shared repository knowledge. Repository knowledge must travel with version control, not live in external memory systems.

## Core Rule

**Repository knowledge goes in `./memories/repo/` as version-controlled files.** Do NOT use external memory tools (like `memory` or Copilot memory storage) for facts that should be shared through VCS.

## Override Statement

Any earlier guidance suggesting external memory tools for repository knowledge is **invalid**. The canonical location is:
```
./memories/repo/
```

## What Belongs in `./memories/repo/`

- Codebase conventions
- Project structure facts
- Build commands
- Investigation results
- Workflow rules
- Any durable facts shared with future agents

## What Does NOT Belong

- Temporary working notes
- Task-specific scratch files
- One-off investigation results that won't be reused

## Language Convention

Write repository knowledge files in **Simplified Chinese (`zh-Hans`)** unless the user explicitly requests a different language.

## Workflow

### Before Creating a New Knowledge File

1. Check `./memories/repo/` for existing files
2. If a relevant file exists, update it instead of creating a duplicate
3. If the directory doesn't exist, create it first

### When Recording Knowledge

1. Determine the appropriate topic file (or create a new focused file)
2. Keep entries concise: bullet points, direct rules, verified commands
3. Write in Simplified Chinese
4. Organize by topic to keep the folder reusable

### When Knowledge Becomes Obsolete

- Update or correct the file when older guidance is wrong
- Do not leave obsolete information that could mislead future agents

## Directory Structure

```
./memories/repo/
├── build-workflow.md      # Build and test commands
├── code-conventions.md    # Coding style and patterns
├── project-structure.md   # Directory layout and purposes
├── investigation-results/ # Organized by topic
│   ├── api-analysis.md
│   └── performance-notes.md
└── workflow-rules.md      # Process and procedure rules
```

## Examples

**Correct:**
- Save build workflow to `./memories/repo/build-workflow.md`
- Update `./memories/repo/code-conventions.md` with new pattern

**Incorrect:**
- Store conventions only in external memory tool
- Scatter authoritative knowledge across unrelated folders
- Use temporary notes as authoritative substitute for repo knowledge

## Integration with Other Skills

This skill should be consulted alongside other skills. When another skill produces durable knowledge that should be shared, the agent should invoke this skill's guidance to store it properly.
