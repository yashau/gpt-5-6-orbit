# Repository guidance

## Purpose

Maintain GPT-5.6 Orbit as a concise, original Codex orchestration skill. Preserve its central contract: Sol plans and judges, Terra performs most development, Luna performs mechanical work, and the invoking task conducts evidence-gated handoffs.

## Sources of truth

- Treat `.agents/skills/gpt-5-6-orbit/SKILL.md` as the behavioral source of truth.
- Treat `.agents/skills/gpt-5-6-orbit/references/effort-guide.md` as the source for benchmark numbers and price/performance conclusions.
- Keep `README.md` aligned with the skill, but do not place instructions there that the running skill requires.
- Keep `agents/openai.yaml` aligned with the skill name and explicit-invocation policy.

## Required invariants

- Keep only `name` and `description` in `SKILL.md` frontmatter.
- Keep the skill folder and frontmatter name exactly `gpt-5-6-orbit`.
- Keep `policy.allow_implicit_invocation` set to `false`.
- Keep the exact model IDs centralized in the model-contract table.
- Never silently substitute models, lower effort, or claim a child ran without evidence.
- Never let skill invocation imply deployment or another external side effect.
- Keep one writer per checkout and require an integration gate for parallel worktrees.
- Keep Ultra out of automatic routing until reliable benchmark evidence supports a default.
- Preserve original wording and structure; do not copy prose from other relay or orchestration skills.

## Editing workflow

1. Read the complete skill and every reference affected by the change.
2. Make the smallest coherent update.
3. Update `README.md` when public behavior or installation changes.
4. Regenerate `agents/openai.yaml` when the name, description, or default prompt changes; then restore the explicit-invocation policy if needed.
5. Run the skill-creator validator against `.agents/skills/gpt-5-6-orbit`.
6. Run `git diff --check` and inspect `git diff`.
7. Forward-test material routing changes on safe, local, non-production tasks.

## Writing style

- Write skill instructions in imperative form.
- Keep the primary skill focused and under 500 lines.
- Prefer a small table or decision rule over repeated prose.
- Put detailed or time-sensitive evidence in `references/`, one link away from `SKILL.md`.
- State authorization boundaries and failure behavior explicitly.
- Do not add auxiliary documentation unless it directly helps users install, understand, or maintain the repository.

## Validation expectations

At minimum, verify:

- valid YAML frontmatter;
- valid `agents/openai.yaml` metadata;
- no unfinished placeholder markers;
- every linked local reference exists;
- model IDs and effort values match the current host before changing defaults;
- benchmark claims include source and observation date;
- route examples obey the Sol/Terra/Luna responsibility split.
