---
name: hashed-second-brain
description: Build and maintain a personal LLM-powered wiki (second brain) with tiered memory (hot/warm/cold) and Knowledge Compression Hashing — periodically consolidate clusters of related notes into dense, hash-addressed artifacts so retrieval is a direct key lookup instead of a graph scan, and old knowledge ages by getting compressed harder while its key still resolves.
---

# Skill: Hashed Second Brain

## Purpose

A superset of the `second-brain` skill. It keeps a persistent, compounding wiki of
interlinked markdown files, but adds a memory model that behaves like a human brain
instead of a landfill:

- **Knowledge Compression Hashing.** Instead of hoarding a thousand raw notes and hoping
  semantic search finds the right one, the skill periodically **compresses clusters of
  related memory into a single dense artifact** — a summary that keeps the signal and
  drops the noise — and **hashes it to a stable key** so retrieval is a *direct lookup,
  not a scan* across the whole graph. Compression is the consolidation step (many sloppy
  notes collapse into one high-value artifact); the hash is what gives fast,
  recency-weighted access without re-reading everything.
- **Tiered memory.** `hot` (recent, days/weeks) → `warm` (consolidated summaries) →
  `cold` (archived, surfaced only on explicit relevance). As memory ages it gets
  compressed harder: fine-grained detail drops out, the gist stays, and the key still
  resolves — so old knowledge stays reachable when relevant but stops cluttering the hot
  path. **Retrieval cost scales with age**, same as a human brain.

The human curates sources and asks questions; the LLM does all the compression,
hashing, tiering, and bookkeeping.

## When to invoke

- "add this to my hashed brain / second brain / knowledge base"
- "ingest [source / article / file]"
- "what does my brain say about X" / "query my second brain"
- "compress / consolidate my brain" / "run consolidation"
- "age / sweep my brain" / "run memory maintenance"
- "lint my wiki" / "health-check the brain"
- "set up a hashed second brain" / "initialize tiered memory"
- User pastes an article or document and says "file this" / "remember this"

## Model Selection

**Task tier:** standard (with mechanical + deterministic phases split out).
**Reasoning:** Consolidation and query synthesis require multi-file prose compression and
maintaining coherent cross-references across many pages — standard tier is the minimum
viable tier. Hashing is a deterministic command (no model). Aging/sweep is mostly file
movement and date math (cheap tier or inline).

**Sub-agent calls inside this skill:**
- Compress/consolidate: cluster → dense artifact → `claude-sonnet-4.6` — lossy prose
  compression that must preserve key claims and flagged contradictions while dropping
  redundancy. One sub-agent per cluster; parallelize when 3+ clusters.
- Query: tiered synthesis from artifacts → `claude-sonnet-4.6` — reading resolved
  artifacts and producing a cited answer.
- Lint: contradiction + tier/hash health scan → `claude-sonnet-4.6`.
- Aging/sweep: tier transitions, recompression to gist → `claude-haiku-4.5` or inline —
  largely mechanical (date math, moving files, harder re-summarize).
- Hashing: **no model** — always the deterministic command in "Computing the hash key".

When the `task` tool is used for parallel consolidation, always pass
`model: "claude-sonnet-4.6"` explicitly on each call. Init, log-append, and hash
computation run inline.

---

## Behavior

### Step 0: Locate the brain

Look for a `second-brain.md` config file in the cwd. If found, read `brain-root`,
`topic`, and the tier thresholds. If not found, ask:

> "Should I initialize a new hashed brain here, or do you have an existing brain at a
> specific path?"

Route to **init** or use the provided path.

---

### Computing the hash key (used by every operation)

The key is a **stable content-address of the topic's identity, NOT a hash of the
content.** Content changes every time an artifact is compressed harder; the key must not.
This is what lets old knowledge stay reachable — "the key still resolves" after the memory
has been compressed from full detail down to a gist.

Algorithm (always identical, so lookups are reproducible):

1. **Canonicalize** the topic: lowercase → trim → replace every run of non-alphanumeric
   characters with a single hyphen → strip leading/trailing hyphens. → a kebab-case slug.
2. **Hash** the canonical slug with SHA-1 and take the first 8 hex chars, prefixed `h:`.

```bash
# python (portable)
python -c "import hashlib,sys; print('h:'+hashlib.sha1(sys.argv[1].encode()).hexdigest()[:8])" "distributed-consensus"
# -> h:f9acf2cb
```

```powershell
# PowerShell
$s='distributed-consensus'
'h:'+(-join ([System.Security.Cryptography.SHA1]::Create().ComputeHash(
  [Text.Encoding]::UTF8.GetBytes($s)) | ForEach-Object { $_.ToString('x2') })).Substring(0,8)
```

The same topic always maps to the same key across every tier, so consolidation and aging
repoint the key without ever changing it.

---

### Operation: init

**Trigger:** User wants to create a new hashed brain.

1. Ask: "What topic or domain is this brain for?"
2. Ask: "Where should the brain live?" (default: `./brain/` in cwd)
3. Create this structure:
   ```
   {brain-root}/
     raw/                     ← immutable source files (never modified)
     archive/                 ← full-detail pages demoted by consolidation; kept for rehydration
     wiki/
       hot/                   ← recent, full-detail pages (L0)
         entities/  concepts/  sources/  syntheses/
       warm/                  ← consolidated artifacts (L1: signal kept, noise dropped)
       cold/                  ← gist-only artifacts (L2: fine detail dropped, key still resolves)
       index.md               ← catalog of pages + tier map
       hashes.md              ← the hash table: key → artifact + tier + recency metadata
       log.md                 ← append-only operation log
     SCHEMA.md
   second-brain.md            ← config (brain-root, topic, tier thresholds)
   ```
4. Write `SCHEMA.md` with:
   - Domain/topic of this brain.
   - Directory conventions (`raw/` immutable, `archive/` rehydration store, `wiki/` LLM-owned).
   - Tier definitions and compression levels (L0 hot / L1 warm / L2 cold — see below).
   - The **hash-key algorithm** (copy "Computing the hash key" verbatim).
   - Page naming: kebab-case; hot pages keep category prefixes; warm/cold artifacts are
     named by canonical topic slug.
   - Artifact frontmatter schema:
     ```yaml
     ---
     key: h:xxxxxxxx
     topic: canonical-topic-slug
     tier: hot            # hot | warm | cold
     level: 0             # 0 = full detail, 1 = consolidated, 2 = gist
     sources: []          # source slugs that informed this
     consolidates: []     # pages folded into this artifact (paths, now in archive/)
     last-compressed: YYYY-MM-DD
     last-accessed: YYYY-MM-DD
     access-count: 0
     updated: YYYY-MM-DD
     ---
     ```
   - Cross-reference convention: `[[key]]` or `[[topic-slug]]` wikilinks.
   - `hashes.md` table format (see below).
   - Log entry format: `## [YYYY-MM-DD] {operation} | {title}`.
   - Contradiction callout: `> ⚠️ **Contradiction:** …`
5. Write `wiki/index.md` with sections `## Hot`, `## Warm`, `## Cold`.
6. Write `wiki/hashes.md` with the empty hash table header:
   ```
   | key | topic | tier | level | artifact | last-compressed | last-accessed | hits |
   |-----|-------|------|-------|----------|-----------------|----------------|------|
   ```
7. Write `wiki/log.md` with the init entry.
8. Write `second-brain.md` config:
   ```
   brain-root: {absolute-path}
   topic: {topic}
   initialized: {date}
   hot-window-days: 21        # a page/artifact touched within this window counts as hot
   warm-max-age-days: 120     # warm artifacts older than this (and low-hit) sweep to cold
   hot-page-budget: 60        # when hot exceeds this, recommend consolidation
   ```
9. Confirm paths, how to drop sources into `raw/`, and the four verbs: ingest, compress,
   age, query.
10. Ask: "Should I `git init` the brain directory for version history?"

---

### Operation: ingest

**Trigger:** User adds a source (file path or pasted content). New knowledge always lands
in the **hot** tier at full detail.

1. **Read** the source (a file in `raw/` or pasted content).
2. **Discuss** 3–5 key takeaways. Ask what to emphasize or deprioritize.
3. **Assess impact:** read `wiki/index.md` (+ `hashes.md`) to find touched topics. List
   them before writing.
4. **Write source summary page** at `wiki/hot/sources/{slug}.md` (L0): frontmatter,
   one-paragraph summary, key claims/data, contradictions vs. existing knowledge,
   wikilinks.
5. **Update / create hot pages** (`wiki/hot/entities|concepts/…`, L0). Preserve existing
   content; add or revise only what the source changes; flag contradictions.
6. **Register/refresh hash keys.** For every topic created or touched, compute its key and
   upsert a row in `hashes.md` pointing at the hot page, `tier: hot`, `level: 0`,
   `last-accessed = today`, `hits += 1` when touched.
7. **Update `wiki/index.md`** (Hot section) and **append to `wiki/log.md`**:
   ```
   ## [YYYY-MM-DD] ingest | {Source Title}
   - Hot pages created: {list}
   - Hot pages updated: {list}
   - Keys registered/updated: {list}
   - Contradictions flagged: {count or "none"}
   ```
8. Report pages created/updated, keys registered, contradictions. If hot page count now
   exceeds `hot-page-budget`, suggest running **compress**.

---

### Operation: compress  (consolidation — the core mechanism)

**Trigger:** "compress" / "consolidate my brain", or automatically recommended when the
hot tier exceeds `hot-page-budget`. This is the step where many sloppy notes collapse
into one high-value artifact — the way a week of experience becomes a handful of things
you actually remember.

1. **Cluster** hot pages into topic clusters by shared tags, wikilinks, and entities.
   Present the proposed clusters to the user before compressing.
2. For **each cluster** (parallel `task`, `model: "claude-sonnet-4.6"` when 3+):
   a. Compute the cluster's **canonical topic + stable key**.
   b. Produce a **dense L1 artifact**: keep signal (key claims, resolved contradictions,
      the consolidated cross-reference map), drop noise (redundancy, restated detail,
      chatter). Target a large compression ratio while losing no key claim.
   c. Write `wiki/warm/{topic}.md` with frontmatter `tier: warm`, `level: 1`,
      `consolidates: [<folded page paths>]`, `last-compressed = today`.
   d. **Repoint the key** in `hashes.md` → the warm artifact; set `tier: warm`,
      `level: 1`. The key does not change — only what it resolves to.
   e. **Demote, don't delete:** move the folded-in hot pages to `archive/` (immutable,
      rehydratable). They leave the hot path; a backlink from the artifact remains.
3. **Update `wiki/index.md`** (move entries Hot → Warm) and **append to `wiki/log.md`**:
   ```
   ## [YYYY-MM-DD] compress | {N clusters}
   - Artifacts written: {key → topic list}
   - Hot pages folded → archive/: {count}
   - Approx compression ratio: {pages in : artifact out}
   - Key claims preserved / dropped detail: {note}
   ```
4. Report artifacts created, keys repointed, hot pages reclaimed.

---

### Operation: age  (sweep — retrieval cost scales with age)

**Trigger:** "age" / "sweep my brain" / periodic maintenance. Older memory is compressed
harder and pushed down a tier; recency (not just calendar age) governs the move.

1. Compute each item's **effective age** from `last-accessed`, recency-weighted: a
   frequently accessed artifact stays warm even if old; a never-revisited one ages fast.
2. **Hot → Warm:** hot pages older than `hot-window-days` with no recent access are queued
   for **compress** (consolidated into/added to a warm artifact).
3. **Warm → Cold:** warm artifacts older than `warm-max-age-days` with low `hits` are
   **compressed harder to an L2 gist** — a few sentences / a "ball of knowledge" bullet
   list. Drop fine-grained detail; keep the gist, the key, and a pointer to the folded
   pages in `archive/` for rehydration. Write to `wiki/cold/{topic}.md`, set `tier: cold`,
   `level: 2`. **The key still resolves.**
4. Update `hashes.md` tiers, move `index.md` entries, and append a `age` log entry
   (items demoted, new levels, detail dropped).
5. Report what moved down and what is now cold.

---

### Operation: query  (direct lookup, tiered, recency-weighted)

**Trigger:** User asks a question against the brain. Retrieval is a key lookup first and a
scan only as a fallback; cost grows only if the answer forces you into older tiers.

1. **Derive canonical topic key(s)** from the question (same hashing algorithm).
2. **Direct lookup** in `hashes.md`: resolve matching keys → candidate artifacts. Only if
   no key matches, fall back to `index.md` and then grep (the expensive path).
3. **Read tier by tier, cheapest first:**
   - Read **hot** artifacts for the resolved keys.
   - If unanswered, read **warm** artifacts.
   - Read **cold** **only** if (a) a cold key is a strong/explicit match, (b) hot+warm
     don't answer, or (c) the user asked to include archived memory. If cold detail is
     needed, **rehydrate** from `archive/`.
4. **Synthesize** a cited answer: `([topic](wiki/<tier>/<topic>.md), key h:xxxxxxxx)`.
   Let the question shape the format (prose, table, bullets).
5. **Reheat on access (reconsolidation):** for every artifact used, set
   `last-accessed = today` and `hits += 1` in `hashes.md`. If a **cold** artifact proved
   relevant, **promote it back toward warm** (and rehydrate detail from `archive/` if the
   answer needed it) — accessing old memory strengthens it, human-style.
6. Offer to file the answer: if yes, write `wiki/hot/syntheses/{slug}.md`, register its
   key, update `index.md` and `log.md`
   (`## [YYYY-MM-DD] query | {question truncated}`).

---

### Operation: lint

**Trigger:** "lint" / "health-check" / periodic maintenance. Base wiki checks **plus**
tier + hash integrity.

| Category | What to look for |
|----------|-----------------|
| **Contradictions** | Claims on two artifacts that conflict |
| **Dangling keys** | `hashes.md` rows whose artifact file is missing |
| **Unhashed topics** | Artifacts/pages with no key in `hashes.md` |
| **Hot bloat** | Hot exceeds `hot-page-budget` → recommend **compress** |
| **Stale warm** | Warm past `warm-max-age-days` with low hits → recommend **age** |
| **Over-compression** | Cold gist dropped a key claim or has no `archive/` rehydration pointer |
| **Key collisions** | Two distinct topics resolving to the same key (rare) → disambiguate |
| **Orphan artifacts** | No inbound `[[...]]` links |
| **Missing pages** | `[[wikilinks]]` pointing to artifacts that don't exist |

Offer to file the report as `wiki/lint-{date}.md`; append a `lint` log entry.

---

## Compression levels

| Level | Tier | Content | Typical trigger |
|-------|------|---------|-----------------|
| **L0** | hot  | Full-detail page as ingested (source/entity/concept/synthesis) | ingest |
| **L1** | warm | Cluster consolidated to one dense artifact — signal kept, redundancy/detail dropped, folded pages archived | compress |
| **L2** | cold | Gist only — a few sentences / "ball of knowledge"; fine detail gone; key + archive pointer retained | age |

---

## Important Notes

- **Keys are stable topic-addresses, not content hashes.** They survive every
  recompression, so a memory can be squeezed from full detail to a gist and the key still
  resolves. Always compute keys with the exact algorithm in "Computing the hash key".
- **Compress signal, drop noise.** Consolidation is lossy on purpose, but it must never
  drop a key claim or a flagged contradiction — only redundancy and fine detail.
- **Compression, not deletion.** Folded pages move to `archive/` (immutable,
  rehydratable); cold artifacts keep a pointer back. Nothing is destroyed.
- **Raw sources are immutable.** The LLM reads `raw/` but never writes there.
- **The LLM owns `wiki/`.** The user browses; the LLM writes and maintains.
- **Lookup before scan.** Queries resolve keys in `hashes.md` first; scanning `index.md`
  or grepping is the fallback, not the default.
- **Recency-weighted, self-heating.** Access reheats artifacts and can promote cold →
  warm, keeping the hot path small and the useful stuff fast.
- **Retrieval cost scales with age.** Hot is cheap and read first; cold is expensive and
  touched only on explicit relevance — a brain, not a landfill.
- **The log is append-only.** Never edit past entries.
- **Obsidian- & git-friendly.** `wiki/` is a valid Obsidian vault; `[[wikilinks]]` and
  YAML frontmatter work with Dataview. The whole brain is a valid git repo — recommend
  `git init`.
- **Scale-out search:** past ~100 warm/cold artifacts, suggest
  [qmd](https://github.com/tobi/qmd) for hybrid BM25/vector search as a fallback layer
  behind the hash table.
