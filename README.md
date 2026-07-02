# Second Brain

A personal, LLM-powered wiki that grows with you. You feed it sources and ask it
questions; the assistant does all the bookkeeping — writing pages, linking ideas,
flagging contradictions, and keeping the knowledge base healthy over time.

This repo contains the [`second-brain`](second-brain/SKILL.md) skill that powers it.

---

## What it does

- **Ingests** articles, notes, and documents into interlinked markdown wiki pages.
- **Answers questions** from your accumulated knowledge, with citations back to pages.
- **Maintains itself** — detects contradictions, stale pages, orphans, and gaps.
- **Compounds** — every good answer can be filed back as a page, so knowledge builds up.

You curate the sources and ask the questions. The assistant writes and maintains the wiki.

---

## How the brain is organized

```
brain/
  raw/            ← you drop immutable source files here (never edited by the assistant)
  wiki/
    index.md      ← catalog of every wiki page (assistant-maintained)
    log.md        ← append-only history of every operation
    entities/     ← people, orgs, things
    concepts/     ← ideas and definitions
    sources/      ← one summary page per ingested source
    syntheses/    ← filed answers to your questions
  SCHEMA.md       ← the conventions this brain follows
second-brain.md   ← config pointing at the brain root + topic
```

- **`raw/` is immutable** — the assistant reads sources but never changes them.
- **`wiki/` is assistant-owned** — you browse; the assistant writes.
- Pages cross-link with Obsidian-style `[[wikilinks]]` and use YAML frontmatter,
  so the `wiki/` folder is a valid **Obsidian vault** and Dataview-compatible.

---

## Getting started

The skill is triggered by plain-language requests inside your Copilot session — there
are no commands to memorize. Just say what you want.

### 1. Initialize a brain

> "Set up a second brain" / "Initialize my wiki"

The assistant asks two things:

1. **What topic** is this brain for? (e.g. "research on X", "book notes", "personal journal")
2. **Where** should it live? (default: `./brain/`)

It then scaffolds the directory structure above, writes `SCHEMA.md`, and offers to
`git init` the brain for version history.

### 2. Add sources (ingest)

> "Add this to my second brain" / "Ingest this article" / "File this"

Drop a file into `raw/` and point the assistant at it, or just paste the content into
chat. The assistant will:

1. Summarize 3–5 key takeaways and ask what to emphasize.
2. Tell you which existing pages the source affects **before** writing.
3. Write a source summary page, update affected pages, and create new ones.
4. Flag any contradictions with existing knowledge (`⚠️ Contradiction:`).
5. Update `index.md` and append to `log.md`.

### 3. Ask questions (query)

> "What does my wiki say about X?" / "Query my second brain"

The assistant reads the index, pulls the relevant pages, and answers with inline
citations. Rich answers can be filed back into `wiki/syntheses/` so your explorations
compound.

### 4. Keep it healthy (lint)

> "Lint my wiki" / "Health-check the wiki"

Produces a report covering contradictions, stale content, orphan pages, missing
(dangling) links, thin/data-gap topics, and cross-reference gaps. Can be saved as
`wiki/lint-{date}.md`.

---

## Core rules the assistant follows

- **Raw sources are immutable** — never modified.
- **The assistant owns `wiki/`** — it writes; you read.
- **Index-first** — the index is always read before drilling into pages (cheaper/faster).
- **The log is append-only** — past entries are never edited.
- **Good answers get filed** — so knowledge accumulates instead of evaporating.
- **`SCHEMA.md` co-evolves** — new conventions discovered mid-session are written back.

---

## Tooling & compatibility

- **Git-friendly** — the entire brain directory is a valid git repo; versioning is recommended.
- **Obsidian-compatible** — `wiki/` is a valid vault; `[[wikilinks]]` and frontmatter work out of the box.
- **Scales** — past ~100 pages, consider [qmd](https://github.com/tobi/qmd) for hybrid
  BM25/vector search over the wiki.

---

## Reference

The full skill specification lives in [`second-brain/SKILL.md`](second-brain/SKILL.md).
