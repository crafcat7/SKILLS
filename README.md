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
| [`conversation-continuation-policy`](./conversation-continuation-policy/SKILL.md) | Closes every interactive agent turn with a follow-up-question tool call (`question` in OpenCode, `vscode_askQuestions` in VSCode/Copilot) to stay inside the current premium request window. |
| [`error-log-analyzer`](./error-log-analyzer/SKILL.md) | Diagnoses error logs, proposes fixes for user approval, applies them, and writes a structured repair report. |
| [`memory-repository-workflow`](./memory-repository-workflow/SKILL.md) | Stores shared repository knowledge as version-controlled Markdown under `./memories/repo/` instead of per-agent external memory tools. |
| [`skill-reviewer`](./skill-reviewer/SKILL.md) | Meta-skill. Rigorously reviews a newly drafted or edited skill across five dimensions (intent, triggering, examples, writing, structure) and produces a scored report with prioritized recommendations. Invoke after every new SKILL.md draft. |

## Authoring a new skill

1. Draft `SKILL.md` with a pushy, scenario-rich `description`.
2. Invoke `skill-reviewer` to audit the draft before shipping it.
3. Address the P0 items surfaced by the review; consider the P1/P2 items.
4. Commit using `commit-format-standard`.

## License

See [LICENSE](./LICENSE).
