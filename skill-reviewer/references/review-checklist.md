# Skill Review Checklist

This checklist expands the SKILL.md review workflow into per-item detail, organized by the same five phases. Each item lists: the check, why it matters, red-flag signals, and typical fixes.

The reviewer does not need to walk through every item mechanically. Use this document when scoring is hard to decide or when a specific recommendation needs grounding.

---

## Phase 1 — Intent extraction

### 1.1 Name matches actual function
- **Check**: From the `name` alone, can the reader roughly guess what the skill does?
- **Why**: `name` is the shortest identifier and shows up first in the `available_skills` listing.
- **Red flags**: Overly generic names (`helper`, `utils`, `assistant`); a name that contradicts the description.
- **Fix**: Rename using a verb-object pattern (`pdf-text-extractor`, `arabella-strategy-engineer`).

### 1.2 Single responsibility
- **Check**: Does the skill do one thing, or is it bundling several?
- **Why**: Multi-responsibility skills trigger ambiguously and produce uneven quality.
- **Red flags**: The `description` uses "and also", "or", or "can additionally" to glue unrelated jobs.
- **Fix**: Split into multiple skills, or designate a primary responsibility and mark others as side-effects.

### 1.3 Identifiable target user
- **Check**: Is the audience clear — backend engineers in Claude Code? Analysts writing reports? Ops staff?
- **Why**: Audience determines terminology, detail level, and expected format.
- **Red flags**: After reading, you cannot tell who this is for.
- **Fix**: Add a one-line audience statement near the top of SKILL.md.

---

## Phase 2 — Triggering rigor

### 2.1 Description length
- **Check**: At least ~60 words (English) or equivalent in other languages?
- **Why**: Too short cannot cover the trigger surface; Claude tends to under-trigger.
- **Red flags**: One-sentence descriptions like `"Helps with X"` or `"A skill for Y"`.
- **Fix**: Expand to cover *what* + *when* + 3 concrete scenarios.

### 2.2 Pushy phrasing
- **Check**: Contains at least one of: `use this skill when`, `make sure to use`, `MUST be used whenever`.
- **Why**: Neutral descriptions are ignored; Claude errs on the side of not loading skills.
- **Red flags**: The description is purely descriptive ("A skill that does X") with no invocation signal.
- **Fix**: Append "Make sure to use this skill whenever ..., even if the user does not explicitly ask for it."

### 2.3 Scenario enumeration
- **Check**: Are at least 3 concrete trigger scenarios listed, with synonyms and colloquial variants?
- **Why**: Real user phrasing varies; keyword coverage drives hit rate.
- **Red flags**: A single phrasing covers all cases; no abbreviations or casual wording.
- **Fix**: Brainstorm 3-5 realistic ways a user might phrase the need, including abbreviated or informal variants.

### 2.4 Boundary signal (should-NOT-trigger)
- **Check**: Does the description hint at when *not* to use this skill?
- **Why**: Prevents over-triggering that crowds out other skills or simple direct work.
- **Red flags**: Keywords are so broad that they overlap with several existing skills.
- **Fix**: Add "Use this only when X; if only Y, no invocation needed."

### 2.5 Avoid keyword dilution
- **Check**: Is the description bloated with unrelated terminology?
- **Why**: Too many keywords lower the weight of each.
- **Red flags**: Descriptions over 200 words where less than a quarter is actually relevant.
- **Fix**: Trim; keep only high-signal terms.

---

## Phase 3 — Examples & output format

### 3.1 At least one concrete example
- **Check**: Does SKILL.md contain an explicit Input/Output, Example, or Before/After block?
- **Why**: Examples teach the model more effectively than prose (few-shot effect).
- **Red flags**: Purely abstract instructions with no concrete text.
- **Fix**: Add one typical Input → expected Output pair.

### 3.2 Representativeness
- **Check**: Do examples cover common usage rather than rare corner cases?
- **Why**: Examples get imitated; corner-case examples bias generalization.
- **Red flags**: Examples only work for one specific file or customer.
- **Fix**: Use more general scenarios; add a second example to cover a different case class if needed.

### 3.3 Output format template
- **Check**: If the skill requires a fixed output (report, commit message, JSON, etc.), is the template copy-pasteable?
- **Why**: Without a template, output format drifts across invocations and becomes unusable downstream.
- **Red flags**: Prose says "produce a report" without specifying the structure.
- **Fix**: Provide an exact template in a fenced code block, with ordered sections and placeholders.

### 3.4 Number of examples
- **Check**: Not 10+ examples turning SKILL.md into an example collection.
- **Why**: Too many examples crowd out the instructions and can over-converge style.
- **Red flags**: Examples take up more than half the SKILL.md.
- **Fix**: Keep 1-3 representative examples; move the rest into `references/`.

---

## Phase 4 — Writing quality

### 4.1 Explain why
- **Check**: Does each hard constraint come with its reason?
- **Why**: Instructions with rationale stay robust in novel situations; bare commands go brittle.
- **Red flags**: `MUST do X.` ending on a period with no explanation.
- **Fix**: Rewrite as `Do X, because otherwise Y would lead to Z.`

### 4.2 Avoid all-caps overuse
- **Check**: Is the body flooded with MUST / NEVER / ALWAYS?
- **Why**: Too many hard rules make the model rigid; the truly important ones lose signal.
- **Red flags**: 3+ all-caps directives inside a single paragraph.
- **Fix**: Keep 1-2 genuinely important strong constraints; turn the rest into explanatory imperatives.

### 4.3 Prefer imperatives
- **Check**: Does the skill use imperatives ("Read the file") instead of "You must read the file"?
- **Why**: Imperatives are tighter and reduce verbose second-person phrasing.
- **Red flags**: Frequent "You should / You must / You need to".
- **Fix**: Rewrite in the imperative voice.

### 4.4 Avoid over-fitting
- **Check**: Are instructions hard-coded to a specific project, path, file name, or repo?
- **Why**: Skills generalize to a million invocations; hard-coding breaks them elsewhere.
- **Red flags**: `Always cd to /home/lune/proj-X`, `The file must be named sales_Q4.xlsx`.
- **Fix**: Parameterize — "the user-specified project directory", "the input spreadsheet".

### 4.5 Length control
- **Check**: Is SKILL.md under ~500 lines?
- **Why**: Anything longer crowds the main context and hurts model performance.
- **Red flags**: SKILL.md over 500 lines with content that could clearly be tiered.
- **Fix**: Move deep reference material into `references/xxx.md` and add a one-line "read when" pointer in SKILL.md.

### 4.6 Consistency
- **Check**: Are terminology, pronouns, and style consistent throughout?
- **Why**: Stylistic whiplash distracts the model.
- **Red flags**: The same concept is called "skill" in one paragraph and "agent instruction" in another.
- **Fix**: Pick one term and use it consistently.

### 4.7 Language — English-only
- **Check**: Is the entire skill (YAML, prose, examples, references) written in English?
- **Why**: Consistency across the skill ecosystem, tooling compatibility, and predictable triggering. Mixed-language content dilutes keyword weight in the description.
- **Red flags**: Description or body in another language; mixed-language paragraphs; non-English headings outside of illustrative quotes.
- **Fix**: Translate the document. Non-English content is allowed only when it genuinely illustrates input/output (e.g. a quoted user prompt or a code comment from real data); surrounding instructions stay in English. This is a **P0** issue.

---

## Phase 5 — Structural sanity

### 5.1 Frontmatter validity
- **Check**: Is the YAML valid? Are `name` and `description` present?
- **Red flags**: Indentation errors, stray quotes, misspelled field names.
- **Fix**: Run the YAML through a validator.

### 5.2 Reference-loading guidance
- **Check**: For every file under `references/`, does SKILL.md explicitly say when to load it?
- **Why**: Without guidance the model never loads the file, rendering it dead weight.
- **Red flags**: Files exist under `references/` but are never mentioned in SKILL.md.
- **Fix**: In the relevant phase, add "When X, read `references/Y.md` for details."

### 5.3 Script utility
- **Check**: Are scripts under `scripts/` genuinely reused across invocations?
- **Why**: One-off code should not be frozen into a skill.
- **Red flags**: A script only used during a single test case.
- **Fix**: Remove, or rewrite as a general-purpose tool.

### 5.4 Orphan files
- **Check**: Are there files that SKILL.md never references and that have no clear purpose?
- **Red flags**: Stray images in `assets/`, leftover debug output.
- **Fix**: Delete them, or add a proper reference.

### 5.5 Directory naming
- **Check**: Lowercase kebab-case?
- **Why**: Cross-platform friendly, URL-safe, community convention.
- **Red flags**: camelCase, snake_case, spaces.
- **Fix**: Rename.

---

## Scoring tie-breakers

When torn between two scores, use these:

- **5 vs 4**: A 5 requires no observed defects; any P2 suggestion drops it to 4.
- **4 vs 3**: 4 tolerates P2 but no P1; any P1 drops it to 3.
- **3 vs 2**: 3 tolerates P1 but no P0; any P0 drops it to 2.
- **2 vs 1**: 2 means at least the skill can still be invoked and produce output; 1 means this dimension is effectively dead (e.g. description is five words long).

## Red-flag quick index

Any of the following should appear in the Red Flags section of the report:

- `description` under ~15 words
- No Example / Input / Output block anywhere
- 5+ all-caps MUST/NEVER directives with no rationale
- Instructions hard-code a specific user path, project name, or file name
- Orphan files in `references/` or `scripts/`
- Invalid YAML frontmatter
- Skill claims multiple responsibilities without designating a primary
- Non-English content in SKILL.md body or description beyond short illustrative quotes
