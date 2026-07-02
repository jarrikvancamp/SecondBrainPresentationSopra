# SCHEMA — Hashed D&D Books Brain

Conventions for this **hashed** second brain. Co-evolved: update this file when new
conventions emerge.

## Domain

A personal knowledge base of **Dungeons & Dragons books** (2024 Player's Handbook, 2024
Dungeon Master's Guide, 2025 Monster Manual, and future sources) built on a **tiered
memory** model with **Knowledge Compression Hashing**.

## Directory conventions

- `raw/` — **immutable** source material, kept **local** (gitignored; may be large or
  copyrighted). The current source books live in the repo-root `raw/` folder.
- `archive/` — full-detail hot pages demoted by consolidation; retained for rehydration.
- `wiki/hot/` — recent, full-detail pages (L0), in category subfolders
  `entities/ concepts/ sources/ syntheses/`.
- `wiki/warm/` — consolidated artifacts (L1): a cluster compressed to one dense page.
- `wiki/cold/` — gist-only artifacts (L2): fine detail dropped, key still resolves.
- `wiki/index.md` — catalog with `## Hot`, `## Warm`, `## Cold` sections.
- `wiki/hashes.md` — the hash table: key → artifact + tier + recency metadata.
- `wiki/log.md` — append-only operation log.

## Tiers & compression levels

| Level | Tier | Content |
|-------|------|---------|
| L0 | hot  | Full-detail page as ingested |
| L1 | warm | Cluster consolidated to one dense artifact (signal kept, redundancy dropped; folded pages archived) |
| L2 | cold | Gist only; fine detail gone; key + `archive/` pointer retained |

**Retrieval cost scales with age:** hot is read first, warm next, cold only on explicit
relevance. Access **reheats** an artifact (bumps `last-accessed`/`hits`) and can promote
cold → warm.

## Computing the hash key

The key is a **stable content-address of the topic identity, NOT a hash of the content**
— it survives every recompression so old knowledge stays reachable.

1. **Canonicalize** the topic: lowercase → trim → replace runs of non-alphanumeric chars
   with a single hyphen → strip leading/trailing hyphens → kebab-case slug.
2. **Hash** the slug with SHA-1; take the first 8 hex chars, prefixed `h:`.

```powershell
$s='beholder'
'h:'+(-join ([System.Security.Cryptography.SHA1]::Create().ComputeHash(
  [Text.Encoding]::UTF8.GetBytes($s)) | ForEach-Object { $_.ToString('x2') })).Substring(0,8)
```

In this brain, the canonical topic = the page's kebab-case **filename slug** (e.g.
`beholder`, `spellcasting`, `nine-hells`).

## Page naming & frontmatter

Kebab-case filenames. Hot pages keep category prefixes; warm/cold artifacts are named by
canonical topic slug. Frontmatter schema:

```yaml
---
key: h:xxxxxxxx
topic: canonical-topic-slug
tier: hot            # hot | warm | cold
level: 0             # 0 = full detail, 1 = consolidated, 2 = gist
sources: []          # source slugs that informed this
consolidates: []     # pages folded into this artifact (paths, now in archive/)
edition: "5e (2024)"
last-accessed: YYYY-MM-DD
access-count: 0
updated: YYYY-MM-DD
---
```

## Cross-references

Use bare **topic-slug** wikilinks: `[[beholder]]`, `[[spellcasting]]`,
`[[nine-hells]]`. Because links address the topic (not a path), they keep resolving as a
page moves hot → warm → cold. The `wiki/` folder is a valid Obsidian vault.

## hashes.md table format

```
| key | topic | tier | level | artifact | last-compressed | last-accessed | hits |
|-----|-------|------|-------|----------|-----------------|----------------|------|
```

## Log entry format

```
## [YYYY-MM-DD] {operation} | {title}
```

Operations: `init`, `ingest`, `compress`, `age`, `query`, `lint`.

## Contradiction callout

```
> ⚠️ **Contradiction:** {A says X (edition/source); B says Y (edition/source)}
```

Prefer noting the **edition** on edition-dependent rules over "resolving" a contradiction.
