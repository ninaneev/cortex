# Example brain — Ledgerline

A complete, small Cortex brain for a fictional company: **Ledgerline**, a B2B
SaaS for e-commerce invoice reconciliation, founded by Mara Voss. Every person,
company, and number in this example is invented.

Layout (this folder plays the role of a project root):

```
examples/
├── source-docs/                          # the raw documents, as ingested
│   ├── 2026-02-17-strategy-memo.md
│   └── 2026-03-04-pricing-meeting.md
└── brain/
    ├── INDEX.md                          # 5 cards indexed
    ├── cards/                            # pricing, icp, churn-drivers,
    │                                     # competitors, roadmap
    └── sources/SOURCES.md                # 2 documents registered
```

What to notice:

- **Two documents fan out into five cards.** Routing is by topic, not by
  document — the strategy memo alone feeds four cards.
- **Every fact carries a citation** with path and section. Pick any line in
  any card, open the cited source, and you'll find it.
- **Dates are absolute.** "Next quarter" in a source becomes "Q3 2026" in a
  card.
- **Cross-document topics merge.** The pricing card draws its decision from
  the meeting notes and its rationale from the memo, and its `sources:` list
  names both.

Try it: open this repo in Claude Code with the cortex skill installed and ask,
from the `examples/` directory, "What did Ledgerline decide about pricing in
March, and why?" — then click through the citations in the answer.
