# Using Cortex day to day

Three verbs: **ingest**, **query**, **maintain**. Everything else is a variation.

## Ingesting

Whenever a document worth remembering lands — meeting notes, a client email you saved as text, a spec, research — file it:

```
> Ingest docs/2026-07-14-call-with-acme.md into the brain
```

For a batch (end of week, imported archive, exported PDF folder):

```
> Use the cortex-librarian agent to ingest everything in docs/imports/
```

What ingestion does — and does not do:

- It **distills**. The card gets the decisions, numbers, and claims, not a copy of the document.
- It **routes by topic, not by document**. One meeting note about pricing and hiring updates two cards.
- It **cites everything**. Each fact carries the source path and section it came from.
- It **resolves dates**. "Next month" in a March note becomes 2026-04 in the card.
- It **skips duplicates** and flags contradictions instead of silently overwriting.

Habit that makes this work: date your document filenames (`2026-07-14-call-with-acme.md`). The date anchors everything downstream.

## Querying

Ask in plain language:

```
> What did we decide about pricing in March?
> What do my notes say about Acme's renewal risk?
> When did we last change the onboarding flow, and why?
```

Every substantive claim in the answer will cite a card and a source file. To audit an answer, open the cited source and read the cited section — that's the entire trust model.

Two answers you should expect and welcome:

- **"The brain has no card covering X."** This is the product working. It tells you what to ingest next instead of inventing an answer.
- **"The card says X, but the source says Y — answering from the source and fixing the card."** Sources always win.

For high-stakes questions, ask for verification explicitly: "…and verify against the sources before answering."

## Maintaining

Monthly, or when the brain starts feeling stale:

```
> Audit the brain: split oversized cards, check INDEX against cards/, verify a sample of citations
```

This splits cards over ~100 lines, removes dead index lines, and flags facts whose source files have moved or vanished (`[SOURCE MISSING]`) without deleting them.

You can also edit cards by hand — they are plain Markdown. Keep the rules: one topic per card, every fact cited, absolute dates, INDEX.md updated.

## Good card vs bad card

**Good:**

```markdown
---
name: pricing
description: Current pricing, history of changes, and open pricing questions.
sources:
  - docs/2026-03-04-pricing-meeting.md
  - docs/2026-02-17-strategy-memo.md
updated: 2026-03-04
---

# Pricing

## Current position

- Pro is $79/seat/month as of 2026-03-04, up from $59; grandfathering existing
  customers for 12 months (source: docs/2026-03-04-pricing-meeting.md, "Decision").

## History

- $59/seat held from launch 2025-06 to 2026-03; two enterprise deals lost on
  "too cheap to be serious" feedback (source: docs/2026-02-17-strategy-memo.md,
  "Pricing signal").

## Open questions

- Annual-only tier: parked until Q3 2026 review (source:
  docs/2026-03-04-pricing-meeting.md, "Parked").
```

Why it's good: one topic, sectioned by sub-question, every fact cited to a path and section, absolute dates, supersession stated ("up from $59"), under 100 lines by a mile.

**Bad:**

```markdown
# Meeting notes March

We talked about pricing recently and decided to raise it. Everyone agreed
this was a good idea. Also discussed hiring — we should probably hire a
support person soon. The team feels good about growth. See meeting notes
for details.
```

Why it's bad, line by line: named after a document, not a topic ("Meeting notes March" — nothing routes to it). "Recently" instead of a date. "Raise it" without the number. Zero citations ("see meeting notes" points nowhere). Two unrelated topics in one card. Mood ("feels good") stored as if it were fact. A card like this makes the brain *worse* than grep — delete it and re-ingest the source.

## Anti-patterns to avoid

- **Pasting documents into cards.** Cards are distillate. If you want the full text, you have it — that's what `sources/` points to.
- **Letting INDEX.md drift.** An unindexed card is invisible to the query protocol.
- **Ingesting everything.** Ingest documents with decisions, facts, and commitments. A brain full of noise retrieves noise.
- **Trusting answers without spot-checks.** The citations exist so you can check them. Occasionally do.
