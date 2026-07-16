# Installing Cortex

Cortex installs into any project where you run Claude Code. It is three copies and two directories — no build step, no services, no dependencies.

## Prerequisites

- Claude Code installed and working (`claude --version`)
- A project directory where your documents live (or will live)

## 1. Install the skill and the agent

From this repo, copy into your project's `.claude/` directory:

```bash
cd /path/to/your-project

mkdir -p .claude/skills/cortex .claude/agents
cp /path/to/cortex/skills/cortex/SKILL.md   .claude/skills/cortex/SKILL.md
cp /path/to/cortex/agents/cortex-librarian.md .claude/agents/cortex-librarian.md
```

To use Cortex in *every* project instead, copy to `~/.claude/skills/cortex/` and `~/.claude/agents/` — but note the brain itself stays per-project.

## 2. Create the brain structure

```bash
mkdir -p brain/cards brain/sources
cp /path/to/cortex/templates/INDEX.md   brain/INDEX.md
cp /path/to/cortex/templates/SOURCES.md brain/sources/SOURCES.md
```

You can delete the placeholder lines in both files or leave them — the first ingestion will replace them.

Your project now looks like:

```
your-project/
├── .claude/
│   ├── skills/cortex/SKILL.md
│   └── agents/cortex-librarian.md
├── brain/
│   ├── INDEX.md
│   ├── cards/
│   └── sources/SOURCES.md
└── docs/            # wherever your documents are
```

## 3. First ingestion — walkthrough

Create a test document so you can see the full loop before trusting it with real material:

```bash
mkdir -p docs
cat > docs/2026-07-10-test-note.md <<'EOF'
# Note to self — 2026-07-10

Decided today: the newsletter goes out Thursdays, not Mondays.
Open rate on the last three Monday sends averaged 31%.
Sarah suggested testing a Tuesday slot in September.
EOF
```

Start Claude Code and ingest it:

```
claude
> Ingest docs/2026-07-10-test-note.md into the brain
```

Expected result — verify all four:

1. A new card exists (e.g. `brain/cards/newsletter.md`) with YAML frontmatter (`name`, `description`, `sources`, `updated`) and each fact followed by a citation like `(source: docs/2026-07-10-test-note.md)`.
2. `brain/INDEX.md` has one new line pointing to that card.
3. `brain/sources/SOURCES.md` has a row for the document with today's date and a one-line summary.
4. The relative date "September" was stored as an absolute reference (2026-09).

## 4. Verification prompt

Now ask a question and check the discipline holds:

```
> What did I decide about the newsletter schedule?
```

A correct answer cites both the card and the source file, e.g. "Newsletter moved to Thursdays, decided 2026-07-10 (brain/cards/newsletter.md, from docs/2026-07-10-test-note.md)."

Then test the anti-hallucination guarantee — ask something the brain cannot know:

```
> What did I decide about the podcast?
```

The correct answer is a variant of "The brain has no card covering a podcast" — with no invented content. If you get a confident made-up answer instead, the skill is not loading; check that `.claude/skills/cortex/SKILL.md` exists and its frontmatter is intact.

## 5. Batch ingestion

For a whole folder, use the librarian agent:

```
> Use the cortex-librarian agent to ingest everything in docs/
```

It processes documents oldest-first, dedupes against existing cards, and reports created / updated / skipped.

## Uninstall

Delete `.claude/skills/cortex/`, `.claude/agents/cortex-librarian.md`, and (if you want to discard the knowledge base) `brain/`. Nothing else was touched.
