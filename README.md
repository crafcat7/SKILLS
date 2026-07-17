# Skills

A collection of OpenCode skills that shape agent behavior during development workflows.

Each skill lives in its own directory with a `SKILL.md` that carries YAML frontmatter (`name`, `description`) and Markdown instructions. OpenCode surfaces the skill list to the agent; the agent loads a skill on demand when the `description` matches the task.

## Conventions

- **Language**: every `SKILL.md` — frontmatter and body — is written in English. Non-English content is allowed only in short illustrative quotes (sample user prompts, code comments) when the surrounding prose stays English.
- **Progressive disclosure**: keep `SKILL.md` under ~500 lines. Push deeper material into `references/` with explicit load-time guidance from `SKILL.md`.
- **Single responsibility per skill**: if a skill needs "and also" in its description, it probably should split.
- **Structure**:
  ```
  skill-name/
  ├── SKILL.md            # required
  ├── references/         # loaded on demand
  └── scripts/            # genuinely reusable tooling
  ```

## Skills

| Skill | Purpose |
|---|---|
| [`commit-format-standard`](./commit-format-standard/SKILL.md) | Standardized Git commit message format (`[type] scope: description` + Summary + Changes), always in English. |
| [`memory-repository-workflow`](./memory-repository-workflow/SKILL.md) | Stores shared repository knowledge as version-controlled Markdown under `./memories/repo/` instead of per-agent external memory tools. |

## Authoring a new skill

1. Draft `SKILL.md` with a pushy, scenario-rich `description`.
2. Commit using `commit-format-standard`.

## License

See [LICENSE](./LICENSE).
