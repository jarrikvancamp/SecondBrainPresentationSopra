# SCHEMA — Dungeons & Dragons Books Brain

Conventions and workflows for this second brain. Co-evolved: when new conventions are
discovered mid-session, update this file so future sessions inherit them.

## Domain

A personal knowledge base of **Dungeons & Dragons books** — rulebooks, sourcebooks,
adventure modules, and lore. Captures rules, mechanics, monsters, spells, characters,
deities, locations, factions, and story/adventure content across editions.

## Directory conventions

- `raw/` — **immutable** source material you drop in (PDFs, text excerpts, notes,
  scanned pages). The assistant reads from here but never edits it.
- `wiki/` — **assistant-owned** interlinked markdown pages. You browse; the assistant
  writes and maintains.
- `wiki/index.md` — catalog of every wiki page (kept current by the assistant).
- `wiki/log.md` — append-only history of every operation.

## Page naming

Kebab-case filenames with category prefixes:

- `entities/` — named things: characters/NPCs, monsters, deities, locations, planes,
  factions, magic items, artifacts. (e.g. `entities/mordenkainen.md`,
  `entities/beholder.md`, `entities/waterdeep.md`)
- `concepts/` — rules & mechanics: classes, subclasses, spells, feats, conditions,
  combat rules, character options. (e.g. `concepts/spellcasting.md`,
  `concepts/wizard.md`, `concepts/advantage.md`)
- `sources/` — one summary page per book/source ingested. (e.g.
  `sources/players-handbook-5e.md`)
- `syntheses/` — filed answers to questions and cross-book explorations.

## Frontmatter schema (wiki pages)

```yaml
---
title: ""
tags: []          # e.g. [monster, cr-10, undead, 5e]
sources: []       # source slugs that informed this page
edition: ""       # e.g. 5e, 3.5e, 2e (when edition-specific)
updated: YYYY-MM-DD
---
```

Source pages use instead:

```yaml
---
title: ""
date: YYYY-MM-DD
source-type: ""   # rulebook | sourcebook | adventure-module | supplement | notes
edition: ""
tags: []
key-claims: []
---
```

## Cross-references

Use Obsidian wikilinks: `[[entities/beholder]]`, `[[concepts/spellcasting]]`,
`[[sources/players-handbook-5e]]`. The `wiki/` folder is a valid Obsidian vault root.

## Log entry format

```
## [YYYY-MM-DD] {operation} | {title}
```

Operations: `init`, `ingest`, `query`, `lint`.

## Contradiction callout

When a new source conflicts with existing wiki knowledge (common across editions), flag
it inline:

```
> ⚠️ **Contradiction:** {page A says X (edition/source); page B says Y (edition/source)}
```

Prefer noting the **edition** on edition-dependent rules rather than "resolving" a
contradiction — different editions legitimately differ.
