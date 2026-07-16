---
name: cortex
description: Use when the user asks a question that should be answered from their own documents ("what did we decide about...", "what do my notes say...", "when did we..."), asks to ingest, add, or file a document into the brain, or asks to maintain/clean up the knowledge base. Also use before answering any factual question about the user's projects, decisions, clients, or history when a brain/ directory exists.
---

# Cortex — local knowledge brain

Cortex is a plain-Markdown knowledge base in `brain/`:

- `brain/INDEX.md` — master map, one line per card.
- `brain/cards/*.md` — knowledge cards: one topic per file, YAML frontmatter, every fact cited to a source file.
- `brain/sources/SOURCES.md` — registry of every ingested source document.

Two protocols govern all work: **ingestion** (documents in) and **query** (answers out). Follow them exactly. The entire value of this system is that answers are grounded and auditable; a single uncited or invented fact breaks the contract.

## Core rules (apply always)

1. **Every fact has a source pointer.** Never write a claim into a card without `(source: path, section)` or an equivalent inline citation. If you cannot point to where a fact came from, do not store it.
2. **One topic per card.** A card answers one recurring question ("pricing", "ICP", "churn drivers"), not one document. Documents fan out into many cards; cards draw from many documents.
3. **Cards stay under ~100 lines.** When a card grows past that, split it into narrower topics and update INDEX.md.
4. **Absolute dates only.** Write `2026-03-04`, never "last Tuesday", "recently", or "next quarter". Resolve relative dates at ingestion time using the document's date.
5. **No invention.** If the brain does not contain the answer, say exactly that. Suggest which documents might contain it. Never fill gaps with general knowledge presented as if it came from the brain.
6. **INDEX.md and SOURCES.md are always current.** Any card created, renamed, or split is reflected in INDEX.md in the same operation. Any document ingested is registered in SOURCES.md in the same operation.

## Ingestion protocol

Run this for each new or updated document.

1. **Check SOURCES.md.** If the document path is already registered and unchanged, stop — report "already ingested". If registered but the document changed, treat this as a re-ingestion: you will update, not duplicate.
2. **Read the document fully.** Note its date (from filename, frontmatter, or content). This date anchors all relative-date resolution.
3. **Extract atomic units:** decisions, facts, claims, numbers, commitments, open questions. Skip filler, pleasantries, and anything already known to the brain word-for-word. Each unit gets the section or heading it came from.
4. **Route each unit:**
   - Read `brain/INDEX.md` and identify the card whose topic covers the unit.
   - **Existing card:** open it, integrate the unit in the right section, add the citation, update the `updated:` date, and add the document to its `sources:` list if new. If the unit *contradicts* an existing fact, keep the newer fact, and note the supersession explicitly: `(supersedes 2026-01 figure, see source)`. Never silently overwrite history that still matters.
   - **No matching card:** create one from `templates/card.md` conventions — frontmatter with `name`, `description`, `sources:`, `updated:`; body organized by sub-question, every line cited.
5. **Cite precisely.** Format: `(source: docs/2026-03-04-pricing-meeting.md, "Decision" section)`. Path relative to project root, plus section or heading. A card-level `sources:` list is not enough — facts carry their own citations.
6. **Update INDEX.md:** one line per new card — `- [card-name](cards/file.md) — one-line description`. Keep alphabetical order within category groups if the index uses them.
7. **Update SOURCES.md:** add or update the row — path, date ingested (today, absolute), one-line summary of what the document contains.
8. **Report:** cards created, cards updated, units skipped as duplicates, contradictions found.

For a *folder* of documents, delegate to the `cortex-librarian` agent rather than ingesting inline.

## Query protocol

Run this for every question that should be answered from the brain.

1. **Read `brain/INDEX.md`.** Identify candidate cards by topic. If the index line is ambiguous, prefer opening one card too many over guessing.
2. **Open the matching cards.** Read them in full — they are short by design.
3. **Verify when it matters.** If the answer hinges on an exact number, a quote, a date, or a decision the user will act on, open the cited source file and confirm the card matches it. If card and source disagree, the source wins — answer from the source and fix the card.
4. **Answer with citations.** Every substantive claim in the answer names its card and its underlying source file. Example: "Pro moved to $79/seat on 2026-03-04 (brain/cards/pricing.md, from docs/2026-03-04-pricing-meeting.md)."
5. **If nothing matches:** say explicitly "The brain has no card covering X." List the nearest cards checked, and if SOURCES.md suggests an un-ingested document might cover it, say which. **Do not answer from general knowledge as a substitute.** If general context would still help the user, give it clearly separated and labeled as not from the brain.
6. **Grep fallback.** If INDEX.md yields no candidate but the topic could plausibly be in the brain under other words, `grep -ri` across `brain/cards/` before concluding it's absent.

## Maintenance

When asked to maintain or audit the brain:

- Split any card over ~100 lines into narrower topics; update INDEX.md.
- Merge cards whose topics have converged; combine their `sources:` lists and update INDEX.md.
- Verify a sample of citations: open the source, check the fact. Flag facts whose source file no longer exists as `[SOURCE MISSING]` rather than deleting them.
- Check INDEX.md against `brain/cards/` — every card indexed, no dead index lines.
