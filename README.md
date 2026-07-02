# Second Brain

A personal, LLM-powered wiki that grows with you. You feed it sources and ask it
questions; the assistant does all the bookkeeping — writing pages, linking ideas,
flagging contradictions, and keeping the knowledge base healthy over time.

This repo contains two skills:

- [`second-brain`](second-brain/SKILL.md) — the base wiki (ingest, query, lint).
- [`hashed-second-brain`](hashed-second-brain/SKILL.md) — the base wiki **plus** tiered
  memory and Knowledge Compression Hashing (see [below](#variant-hashed-second-brain)).

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

## Variant: hashed-second-brain

[`hashed-second-brain`](hashed-second-brain/SKILL.md) does everything above and adds a
memory model that behaves like a brain instead of a landfill.

**Knowledge Compression Hashing.** Instead of hoarding a thousand raw notes and hoping
semantic search finds the right one, it periodically **compresses clusters of related
memory into one dense artifact** (keep the signal, drop the noise) and **hashes it to a
stable key** so retrieval is a *direct lookup, not a scan*. The key is derived from the
topic's identity, not its content — so as an artifact is compressed harder over time, the
key still resolves.

**Tiered memory.** Knowledge flows through three tiers, and **retrieval cost scales with
age**, like a human brain:

| Tier | Holds | Read cost |
|------|-------|-----------|
| **hot** | recent, full-detail pages (days–weeks) | cheap — read first |
| **warm** | consolidated summaries (compressed once) | medium |
| **cold** | archived gist-only artifacts | expensive — surfaced only on explicit relevance |

As memory ages it is compressed harder (detail drops, gist stays) and pushed down a tier;
accessing an old artifact *reheats* it back toward hot. Two extra verbs drive this:

- **compress / consolidate** — cluster hot pages → dense hash-addressed artifacts; fold
  the originals into `archive/` (kept for rehydration, never deleted).
- **age / sweep** — move stale, low-hit memory down a tier and compress it harder.

Use this variant when you want the knowledge base to stay fast and small over the long
run; use the base `second-brain` when you want every page kept at full detail.

---

## Reference

- Base skill: [`second-brain/SKILL.md`](second-brain/SKILL.md)
- Tiered + hashed variant: [`hashed-second-brain/SKILL.md`](hashed-second-brain/SKILL.md)
