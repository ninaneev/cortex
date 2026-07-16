# Cortex

**A brain for your documents. Answers with receipts.**

Cortex turns a folder of your documents — notes, specs, meeting notes, research, exported PDFs — into a structured knowledge base that Claude Code can answer from *with citations to the source files*. No vector database. No embeddings service. No cloud. Just files, structure, and retrieval discipline.

## The problem

LLMs answer confidently from nothing. Ask "what did we decide about pricing in March?" and you get a fluent, plausible paragraph that may have no connection to what you actually decided. Your real answer is sitting in a meeting note on your disk — but the model never read it, and even if it did, it won't tell you where the claim came from.

The failure mode isn't ignorance. It's *unverifiable confidence*.

## How Cortex is different

Every answer cites a file you own.

Cortex maintains a `brain/` directory of distilled **knowledge cards** — one topic per file, every fact tagged with the source document it came from. When you ask a question, Claude follows a strict query protocol: consult the index, open the matching cards, answer from what's written there, and cite both the card and the underlying source. If the brain doesn't contain the answer, it says so. It does not fill the gap with plausible text.

You can audit any answer in seconds: open the cited file, read the cited section, done.

## 60-second quickstart

```bash
# 1. Get the repo
git clone <this-repo> cortex && cd cortex

# 2. Install the skill and agent into your project (or ~/.claude for global)
mkdir -p /path/to/your-project/.claude/skills/cortex /path/to/your-project/.claude/agents
cp skills/cortex/SKILL.md  /path/to/your-project/.claude/skills/cortex/
cp agents/cortex-librarian.md /path/to/your-project/.claude/agents/

# 3. Create the brain structure in your project
mkdir -p /path/to/your-project/brain/cards /path/to/your-project/brain/sources
cp templates/INDEX.md   /path/to/your-project/brain/
cp templates/SOURCES.md /path/to/your-project/brain/sources/

# 4. Ingest your first document
cd /path/to/your-project && claude
> Ingest docs/meeting-notes-march.md into the brain

# 5. Ask something
> What did we decide about pricing in March?
```

Full walkthrough in [docs/INSTALL.md](docs/INSTALL.md). Daily usage in [docs/HOW_TO_USE.md](docs/HOW_TO_USE.md).

## Architecture

```
your-project/
├── .claude/
│   ├── skills/cortex/SKILL.md      # ingestion + query protocols
│   └── agents/cortex-librarian.md  # batch ingestion agent
├── brain/
│   ├── INDEX.md                    # master map: one line per card
│   ├── cards/                      # distilled knowledge, one topic per file
│   │   ├── pricing.md              #   every fact cites its source
│   │   └── ...
│   └── sources/
│       └── SOURCES.md              # registry of everything ingested
└── docs/                           # your raw documents (any folder works)
```

Data flow:

```
raw document ──ingest──▶ claims/decisions/facts ──route──▶ cards (cited)
                                                              │
question ──▶ INDEX.md ──▶ matching cards ──▶ verify source ──▶ answer + citations
```

Ingestion distills; it never copies documents wholesale. Query retrieves; it never invents. The brain is plain Markdown, so `grep`, `git diff`, and your own eyes all work on it.

## FAQ

**Why no vector database?**
Three reasons. *Transparency:* a vector index is opaque — you can't read it, diff it, or audit why a chunk was retrieved. A Markdown card you can open. *Zero dependencies:* nothing to install, host, re-embed, or keep in sync; the brain works on any machine with the files on it. *Grep is enough at personal scale:* at hundreds of documents and a few dozen well-named cards, an index file plus full-text search retrieves as well as embeddings do — and the failure mode is "not found" instead of "silently wrong chunk."

**Isn't this just RAG?**
It's retrieval, but with distillation instead of chunking. Chunks are arbitrary slices of text; cards are curated claims with provenance. The distillation step is where the value concentrates — and where duplicates get merged and contradictions get surfaced instead of retrieved side by side.

**What happens when a source document changes?**
Re-ingest it. The protocol updates the affected cards, refreshes their `updated:` dates, and notes the revision in `SOURCES.md`. Facts whose source disappeared are flagged, not silently kept.

**How big can the brain get?**
Cards are capped at ~100 lines and split by topic, so INDEX.md stays scannable. Personal and small-team scale — hundreds of sources, dozens of cards — works comfortably. If you have millions of documents and a search team, use a search engine.

**Does it work with PDFs?**
Ingest the extracted text (any `pdftotext`-style output). Cortex cites the text file you ingested; keep it next to the original PDF.

**Can I edit cards by hand?**
Yes — that's the point of plain Markdown. Keep the rules: one topic per card, every fact cited, absolute dates, update INDEX.md if you add or rename a card.

## What's in this repo

| Path | What it is |
|---|---|
| `skills/cortex/SKILL.md` | The Cortex skill: ingestion and query protocols |
| `agents/cortex-librarian.md` | Batch-ingestion agent for whole folders |
| `templates/` | Starter card, INDEX, and SOURCES files |
| `examples/` | A small, complete example brain for a fictional SaaS founder |
| `docs/INSTALL.md` | Installation and first-ingestion walkthrough |
| `docs/HOW_TO_USE.md` | Daily usage: ingesting, querying, maintaining |

## License

MIT — see [LICENSE](LICENSE).
