# Cortex — repo instructions

This repo is a shipped product: a local knowledge brain built on Claude Code (skill + agent + templates + example). People copy files out of it verbatim. Treat every file as user-facing.

## What lives where

- `skills/cortex/SKILL.md` — the product core: ingestion and query protocols. Changes here change the product's behavior for every user.
- `agents/cortex-librarian.md` — batch ingestion agent. Must stay consistent with the skill's protocol; the skill is the source of truth.
- `templates/` — files users copy to start a brain. Must match what the skill tells Claude to produce.
- `examples/` — a complete fictional brain (Ledgerline). Every card in it must be a perfect demonstration of the card rules: cited facts, absolute dates, <100 lines, one topic.
- `docs/` — INSTALL.md and HOW_TO_USE.md.

## Rules when editing

1. **Consistency is the product.** The card format is defined in four places (SKILL.md, templates/card.md, docs/HOW_TO_USE.md examples, examples/brain/cards/*). If you change the format, change all four in the same commit.
2. **The examples must obey the skill.** Never add a fact to an example card without a citation to one of the two example source docs. Never add a relative date. The examples double as tests-by-inspection.
3. **Frontmatter is load-bearing.** `skills/cortex/SKILL.md` needs valid `name` and `description` (description starts with "Use when"); `agents/cortex-librarian.md` needs `name`, `description`, `tools`. Broken frontmatter silently disables the component.
4. **No dependencies, ever.** The pitch is files-only. Do not add scripts that require installs, package manifests, or services. Plain Markdown, plain shell in docs.
5. **Tone:** plain, confident, specific. No hype words, no exclamation marks, no "powerful"/"seamless"/"magical". Claims about behavior must be things the skill actually enforces.
6. **Docs stay copy-paste runnable.** Any shell block in docs/ must work as written on a clean machine (bash; note Windows users run these in Git Bash).

## Verifying changes

There is no test suite; verification is manual:

- After editing SKILL.md or the agent: install into a scratch project per docs/INSTALL.md and run the walkthrough in section 3–4, including the negative test (asking something the brain can't know must produce "no card covers X", not an invented answer).
- After editing templates or examples: check each example card against the "Core rules" list in SKILL.md.

## What not to do

- Do not add a vector DB, embeddings, or any retrieval infra. "Why no vector DB?" in the README is a design decision, not a gap.
- Do not grow SKILL.md past what fits comfortably in context (~150 lines). Discipline beats detail.
- Do not put real client, prospect, or company data into examples/. The Ledgerline company and all people in it are fictional and must stay that way.
