---
name: skill-reviewer
description: Rigorously review the quality of a newly created or modified skill. Use this skill whenever a new SKILL.md has just been drafted, an existing skill has been edited, the user mentions "review the skill / check the skill / is this skill reasonable / does this skill have examples / are the trigger conditions strict enough / how does this skill look", or any scenario involving the completion of skill authoring. In particular, after skill-creator finishes drafting a SKILL.md and before the skill is packaged or committed, proactively invoke this skill to perform one pass of review. It scores the skill across five dimensions (intent clarity, triggering rigor, examples & output format, writing quality, structural sanity) on a 1-5 scale and produces a structured report with prioritized recommendations. Make sure to use this skill whenever a new SKILL.md has just been drafted or edited, even if the user does not explicitly ask for a review.
---

# Skill Reviewer

A meta-skill. Its job is to produce a rigorous, actionable review report after another skill has been written or modified, surfacing problems like unclear intent, loose triggering, missing examples, over-rigid instructions, or redundant structure.

This skill does not modify the skill under review — it only produces a report and recommendations. The author decides what to adopt.

## Core philosophy

A skill may be invoked thousands of times, so any flaw gets amplified. The point of review is not "does this skill happen to work on one example" but rather:

- **Is the intent clear enough to be restated?** If the reviewer cannot articulate what the skill does, neither will the agent that later loads it.
- **Is the trigger condition rigorous?** The `description` is the only ticket to entry. It must be both *pushy* (Claude tends to under-trigger) and *bounded* (avoid over-triggering).
- **Does it generalize?** A skill is written for a million future prompts, not for the two or three test cases in front of you today.
- **Does it explain *why*?** An instruction that says only *what* and not *why* leaves the model rigid in edge cases.

Anchor your review on these lenses rather than nitpicking wording.

## When to run

Proactively review in these situations:

- The user has just finished a draft SKILL.md (typically via skill-creator)
- The user has manually edited an existing SKILL.md
- The user is about to package or ship a skill
- The user explicitly asks: "review / check / look at / how does this skill look"

Do not review this skill itself (avoid recursion).

## Language convention for skills under review

All skills maintained in this workspace **must be authored in English**, including YAML frontmatter, body prose, examples, and any files under `references/` or `scripts/` documentation. This ensures:

- Consistency across the skill ecosystem
- Compatibility with third-party tooling, search, and sharing
- Predictable triggering (mixed-language descriptions dilute keyword weight)

If a skill under review contains substantial non-English content in its SKILL.md body or description, flag it as a **P0** issue. Short quoted user phrases or code snippets in other languages are fine when they genuinely illustrate input/output; the surrounding instructions must still be English.

## Review workflow (5 phases)

Each phase below lists a few core questions. The full checklist — with red flags and suggested fixes — lives in `references/review-checklist.md`. Read that file when you need thorough coverage or when the user asks for a deep review.

### Phase 1 — Intent extraction

After this phase you should be able to answer in one or two sentences: what does this skill let the agent do? What pain does it solve? Who is the target caller?

Key questions:
- Does the `name` accurately reflect the skill's job?
- Is the responsibility single-purpose, or does it bundle unrelated concerns?
- After reading SKILL.md once, do you need to re-read it to understand it? (A bad sign.)

### Phase 2 — Triggering rigor

The `description` field is the sole entry mechanism. Audit:
- Does it contain pushy phrasing: `use this skill when`, `make sure to use`, `MUST be used whenever`?
- Does it enumerate 3+ concrete trigger scenarios (including synonyms and colloquial variants)?
- Does it imply or state a should-NOT-trigger boundary (to prevent over-triggering)?
- Is the length reasonable? Too short cannot be rigorous; too long dilutes keyword weight.

### Phase 3 — Examples & output format

Key questions:
- Is there at least one concrete Input/Output or equivalent example?
- Do examples cover typical usage rather than corner cases?
- If the skill requires a specific output format, is the template explicit and copy-pasteable?

A skill without examples produces divergent output styles across invocations — one of the most overlooked defects.

### Phase 4 — Writing quality

Key questions:
- Does every MUST / NEVER / ALWAYS have a *why* next to it? If not, convert to an imperative sentence plus rationale.
- Is there an oppressive wall of all-caps rigid directives? That makes the model brittle at edges.
- Are instructions over-fit to specific test cases (hard-coded repo names, paths, file names)?
- Are imperatives used instead of verbose second-person ("You should / You must")?
- Is the total length under ~500 lines? If longer, material should move into `references/`.
- **Is the entire document written in English?** (See "Language convention" above.)

### Phase 5 — Structural sanity

Key questions:
- Is the YAML frontmatter valid? Are `name` and `description` present?
- For every file under `references/`, does SKILL.md clearly say *when* to load it?
- Do scripts under `scripts/` have real reuse value, or are they one-off?
- Are there orphan files not referenced anywhere?

## Scoring rubric

Score each dimension 1-5:

- **5** — Excellent, no obvious improvements
- **4** — Good, only P2 suggestions
- **3** — Acceptable, some P1 suggestions
- **2** — Marginal, at least one P0 issue
- **1** — Broken, this dimension is effectively non-functional

The Overall score is the mean of the five dimensions, but if any single dimension is ≤ 2, Overall is capped at 3.

Verdict mapping:
- Overall ≥ 4.5 with no P0 → `APPROVED`
- Overall ≥ 3.5 with P1/P2 only → `APPROVED WITH CHANGES`
- Overall < 3.5, or any P0 present → `NEEDS REWORK`

## Output template (follow strictly)

```
# Skill Review: <skill-name>

## 1. Intent Summary
<One or two sentences restating what this skill does, what it solves, and who
 the target caller is. If restating is hard, state that intent is unclear and
 suggest what the author should clarify first.>

## 2. Dimension Scores

| Dimension | Score | Verdict |
|---|---|---|
| Intent Clarity | x/5 | one-line judgment |
| Triggering Rigor | x/5 | one-line judgment |
| Examples & Output | x/5 | one-line judgment |
| Writing Quality | x/5 | one-line judgment |
| Structural Sanity | x/5 | one-line judgment |
| **Overall** | x/5 | summary judgment |

## 3. Red Flags
<List serious issues one per line. Write "None" if there are none.>

## 4. Recommendations

### P0 (must-fix before use)
- ...

### P1 (strongly recommended)
- ...

### P2 (nice to have)
- ...

## 5. Verdict
[APPROVED / APPROVED WITH CHANGES / NEEDS REWORK]

> This is an advisory review. Adoption is at the author's discretion. If you
> want me to rewrite any specific section to address a recommendation, say so.
```

## Reviewer discipline

1. **Do not nitpick.** Low-level formatting preferences (blank lines, heading order) do not belong in the report unless they hurt readability.
2. **Do not grade-inflate or grade-deflate.** Every skill has at least one improvement, so do not hand out straight 5s; conversely, do not deflate scores just to appear strict.
3. **Recommendations must be actionable.** Not "should be clearer" but "rewrite the first sentence of `description` to: XXX".
4. **Respect the author's voice.** Do not homogenize every skill into the same tone.
5. **Review only — do not modify.** Unless the user explicitly asks you to apply a recommendation.

## Examples

### Example 1 — A poorly triggered, example-less skill

**Skill under review (excerpt)**

```yaml
---
name: pdf-helper
description: Helps with PDFs.
---

# PDF Helper

Use this skill to work with PDF files. MUST read the PDF first.
ALWAYS extract text. NEVER skip pages.
```

**Review output**

```
# Skill Review: pdf-helper

## 1. Intent Summary
Intent is unclear. The SKILL.md only signals "something about PDFs" — it is
impossible to tell whether the job is text extraction, form filling,
merge/split, or OCR. The author should first settle on a single responsibility.

## 2. Dimension Scores

| Dimension | Score | Verdict |
|---|---|---|
| Intent Clarity | 1/5 | Cannot infer the concrete function from the description |
| Triggering Rigor | 1/5 | Three words; no scenario enumeration; will not trigger |
| Examples & Output | 1/5 | No examples, no output format |
| Writing Quality | 2/5 | All three sentences are MUST/ALWAYS/NEVER with no rationale |
| Structural Sanity | 3/5 | Frontmatter is valid, no orphan files |
| **Overall** | 1.6/5 | Effectively uninvokable in its current state |

## 3. Red Flags
- description is under 20 characters; Claude will almost never load this skill
- Every directive is a capitalized hard constraint with no explanation or example
- Does not answer "who is this for and when should they use it"

## 4. Recommendations

### P0
- Commit to a single responsibility (e.g. PDF text extraction vs. form
  filling vs. merging) and reflect it in both `name` and `description`
- Rewrite `description` to at least 60 words, enumerate 3+ concrete trigger
  scenarios, and include pushy phrasing like "use this skill when"
- Add at least one Input → Output example

### P1
- Convert MUST/ALWAYS/NEVER into imperative sentences with rationale
- Specify the output format (plain text? JSON? retain page numbers?)

### P2
- Consider whether `references/` is needed for per-library notes

## 5. Verdict
NEEDS REWORK
```

### Example 2 — A high-quality skill

When the skill is genuinely good, keep the review short. Scores should cluster in 4-5, Red Flags should be "None", and most recommendations should be P2. Do not invent problems.

## Referencing the checklist

When you need item-by-item coverage — especially for a thorough review or when the user wants every angle examined — read `references/review-checklist.md`. It expands each of the five phases with concrete checks, red-flag signals, and suggested fixes.
