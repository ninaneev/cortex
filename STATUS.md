# STATUS — Cortex

**v1 scaffold complete 2026-07-15.**

All product files written and internally consistent: skill, librarian agent, templates, install + usage docs, MIT license, and a complete fictional example brain (Ledgerline: 2 source docs, 5 cards, filled INDEX and SOURCES). Repo is files-only — not yet git-initialized, not yet published.

## Done

- [x] `skills/cortex/SKILL.md` — ingestion + query protocols, core rules, maintenance
- [x] `agents/cortex-librarian.md` — batch ingestion with dedupe and report format
- [x] `templates/` — card, INDEX, SOURCES
- [x] `docs/INSTALL.md` — install, first-ingestion walkthrough, verification prompts (incl. negative test)
- [x] `docs/HOW_TO_USE.md` — daily loop, good vs bad card examples, anti-patterns
- [x] `examples/` — Ledgerline brain, every fact cited to the two source docs
- [x] README, CLAUDE.md, BRIDGE.md, LICENSE, .gitignore

## Next steps

1. **Manual verification pass** — install into a scratch project and run the INSTALL.md walkthrough end to end, including the negative test.
2. **git init + first commit + public GitHub repo** (deliberately not done yet; files only so far).
3. **Demo video** — 2–3 min: ingest a real-looking doc, ask a question, click the citation, ask an unknown question and show the honest "not in brain" answer. That last beat is the hook.
4. **Website listing** — add Cortex to the claude-systems lineup with the "answers with receipts" positioning.
5. **Example expansion** — a second example brain from a different persona (consultant or researcher) to show the pattern generalizes beyond SaaS.
6. **Feedback loop** — after first external users: does anyone hit the ~100-line card cap in practice? Does batch ingestion dedupe hold up on 50+ documents?

---

## Session handoff 2026-07-15

- Done: repo scaffolded, committed (04a721c), published public to https://github.com/ninaneev/cortex (MIT where applicable)

- Known broken: nothing known; no verification run in a clean session yet
- Next session: Run docs/INSTALL.md first-ingestion walkthrough as verification (incl. the negative test), then script the demo video.
