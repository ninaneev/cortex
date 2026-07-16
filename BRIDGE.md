# BRIDGE — Cortex

Handoff context for anyone (or any session) picking this repo up cold.

## What this is

Cortex is a self-serve local knowledge brain built on Claude Code: point it at a folder of documents and it maintains a `brain/` of distilled, cited knowledge cards that Claude answers from with receipts. It is the miniature, productized form of the "external AI brain" idea — no infra, no cloud, no vector DB. Target users: founders, consultants, researchers drowning in their own documents.

The product is four artifacts users copy into their own projects: a skill (`skills/cortex/SKILL.md`), an agent (`agents/cortex-librarian.md`), three templates, and docs. The `examples/` brain (fictional SaaS company "Ledgerline", founder Mara Voss) shows the end state.

## Design decisions and why

**Cards over chunks.** RAG chunks are arbitrary slices; retrieval quality depends on an embedding model nobody can inspect. Cortex instead distills documents into topic cards at ingestion time — the expensive thinking happens once, on write, and reads become trivial (index → card → source). This also gives dedup and contradiction-handling a natural home: they happen at routing time, per fact.

**Citation discipline as the core value.** Every fact in a card carries `(source: path, section)`. The query protocol refuses to answer beyond the brain and says "no card covers X" instead. This is the entire differentiation vs. raw LLM answers, so the skill states it as a hard rule, not a preference. Any future feature that weakens auditability is off-strategy.

**No vector DB, deliberately.** At personal scale (hundreds of docs, dozens of cards) INDEX.md + grep retrieves as well as embeddings, is fully transparent, diffs in git, and has zero dependencies. This is a positioning choice as much as a technical one — documented in the README FAQ. Do not "upgrade" it away.

**Plain Markdown everywhere.** Users can read, edit, grep, and version their brain with tools they already have. The brain remains useful even without Claude.

**~100-line card cap, one topic per card.** Keeps cards readable in one screen, keeps INDEX.md scannable, and forces splitting instead of accreting junk-drawer cards.

**Skill + agent split.** The skill handles inline single-document ingestion and all querying; the librarian agent exists so batch ingestion (many docs, lots of reading) doesn't flood the main conversation's context. The skill is the source of truth for the protocol; the agent defers to it.

**Absolute dates rule.** Distilled notes outlive their context; "next quarter" is meaningless a year later. Relative dates are resolved at ingestion using the document's own date.

## State of the repo

v1 scaffold, content-complete, never git-initialized (files only — see STATUS.md). No test suite; verification is the manual walkthrough in docs/INSTALL.md sections 3–4.

## Invariants to preserve (see CLAUDE.md for the full editing rules)

- Card format consistent across SKILL.md, templates/, HOW_TO_USE.md, and examples/.
- Examples contain only fictional data and perfectly obey the card rules.
- Zero dependencies; docs runnable as written.
- The negative test (query about an unknown topic → explicit "not in brain") must keep passing after any skill edit.

_Last updated: 2026-07-15 — v1 published to https://github.com/ninaneev/cortex. Business model: repos free/open source; revenue = €149 guided install per system + €1,500 Full Stack Install flagship (sold via ninaneev.github.io, Stripe pending)._
