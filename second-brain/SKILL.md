---
name: second-brain
description: Build and maintain a personal LLM-powered wiki (second brain) by ingesting sources, querying accumulated knowledge, and keeping the wiki healthy over time.
---

# Skill: Second Brain

## Purpose

Maintains a persistent, compounding personal knowledge base (wiki) of interlinked markdown
files. The LLM ingests source documents, synthesizes and updates wiki pages, answers questions
from accumulated knowledge, and periodically lints the wiki for gaps and contradictions.
The human curates sources and asks questions; the LLM does all the bookkeeping.

## When to invoke

- "add this to my wiki / second brain / knowledge base"
- "ingest [source / article / file]"
- "what does my wiki say about X" / "query my second brain"
- "lint my wiki" / "health-check the wiki"
- "set up a second brain" / "initialize my wiki"
- User pastes an article or document and says "file this" or "add this"

## Model Selection

**Task tier:** standard (with fast/cheap phases)
**Reasoning:** Ingest and query operations require multi-file prose synthesis and maintaining
coherent cross-references across many pages — standard tier is the minimum viable tier.
Mechanical phases (init scaffolding, log appending) need no sub-agent at all.

**Sub-agent calls inside this skill:**
- Ingest: parallel page updates → `claude-sonnet-4.6` — multi-file prose synthesis; must maintain
  coherent cross-references across many pages simultaneously
- Query: synthesis from wiki pages → `claude-sonnet-4.6` — reading multiple pages and producing
  a structured answer with citations
- Lint: contradiction + gap scan → `claude-sonnet-4.6` — detecting subtle contradictions across
  files requires standard-tier reasoning

When the `task` tool is used for parallel page updates during ingest, always pass
`model: "claude-sonnet-4.6"` explicitly on each call.

Init and log-append steps run inline (no sub-agents needed).

---

## Behavior

### Step 0: Locate the brain

Look for a `second-brain.md` config file in the cwd. If found, read it for `brain-root` and
`topic`. If not found, ask the user:

> "Should I initialize a new brain here, or do you have an existing wiki at a specific path?"

Route to **init** (new brain) or use the provided path (existing brain).

---

### Operation: init

**Trigger:** User wants to create a new brain.

1. Ask: "What topic or domain is this brain for?" (e.g. "research on X", "book notes", "personal journal")
2. Ask: "Where should the brain live?" (default: `./brain/` in cwd)
3. Create this directory structure:
   ```
   {brain-root}/
     raw/           ← user drops immutable source files here
     wiki/
       index.md     ← catalog of all wiki pages (LLM maintains)
       log.md       ← append-only operation log
     SCHEMA.md      ← wiki conventions and workflows (co-evolved)
   ```
4. Write `SCHEMA.md` with:
   - Domain/topic of this brain
   - Directory conventions (`raw/` = immutable sources, `wiki/` = LLM-owned pages)
   - Page naming conventions: kebab-case, category prefixes (`entities/`, `concepts/`,
     `sources/`, `syntheses/`)
   - Frontmatter schema for wiki pages:
     ```yaml
     ---
     title: ""
     tags: []
     sources: []       # source slugs that informed this page
     updated: YYYY-MM-DD
     ---
     ```
   - Cross-reference convention: `[[page-name]]` (Obsidian wikilinks)
   - Log entry format: `## [YYYY-MM-DD] {operation} | {title}`
   - Contradiction callout format: `> ⚠️ **Contradiction:** …`
5. Write `wiki/index.md` with a header and empty sections:
   `## Entities`, `## Concepts`, `## Sources`, `## Syntheses`
6. Write `wiki/log.md` with the init entry.
7. Write `second-brain.md` config in cwd:
   ```
   brain-root: {absolute-path}
   topic: {topic}
   initialized: {date}
   ```
8. Confirm: show paths created, how to drop sources into `raw/`, how to trigger ingest.
9. Ask: "Should I `git init` the brain directory for version history?"

---

### Operation: ingest

**Trigger:** User adds a source (file path or pasted content).

1. **Read** the source. Accept a file path in `raw/` or content pasted directly into chat.
2. **Discuss** key takeaways: produce 3–5 bullet points. Ask: "Anything to emphasize or
   deprioritize before I file this?"
3. **Assess impact:** read `wiki/index.md` to identify pages this source touches.
   List them to the user before writing anything.
4. **Write source summary page** at `wiki/sources/{slug}.md`:
   - Frontmatter: `title`, `date`, `source-type`, `tags`, `key-claims`
   - One-paragraph summary
   - Key claims / data points (bulleted)
   - Contradictions or tensions with existing wiki knowledge
   - Wikilinks to related entity/concept pages
5. **Update affected wiki pages** (entity pages, concept pages, overview). For each:
   - Read the page → identify where the new info fits → rewrite the relevant section
   - Preserve all existing content; only add or revise what the new source changes
   - Flag any contradiction with `> ⚠️ **Contradiction:** …`
   - Add a `[[sources/{slug}]]` backlink in the `Sources` section
   - Update `updated:` in frontmatter
   When there are 3+ pages to update, spawn parallel `task` sub-agents
   (`model: "claude-sonnet-4.6"`, `agent_type: "general-purpose"`) — one per page group.
6. **Create new pages** for any entity or concept mentioned but not yet in the wiki.
7. **Update `wiki/index.md`**: add the new source entry; update changed page summaries.
8. **Append to `wiki/log.md`**:
   ```
   ## [YYYY-MM-DD] ingest | {Source Title}
   - Pages created: {list}
   - Pages updated: {list}
   - Key new claims: {1–3 bullets}
   - Contradictions flagged: {count or "none"}
   ```
9. Report to user: pages created, pages updated, contradictions flagged.

---

### Operation: query

**Trigger:** User asks a question against the wiki.

1. Read `wiki/index.md` to identify relevant pages. For large wikis, use grep to narrow.
2. Read the relevant pages.
3. Synthesize a clear answer with inline citations: `([page-name](wiki/page-name.md))`.
4. Present the answer. Then ask: "Should I file this as a wiki page?"
   - If yes: write `wiki/syntheses/{slug}.md`, update `wiki/index.md` and `wiki/log.md`.
   - Log entry format: `## [YYYY-MM-DD] query | {Question (truncated)}`

Output formats can vary by question — prose, comparison table, bullet list, etc.
Let the question shape the format.

---

### Operation: lint

**Trigger:** "Lint my wiki" / "health-check" / periodic maintenance.

Scan all wiki pages and produce a lint report covering:

| Category | What to look for |
|----------|-----------------|
| **Contradictions** | Claims on two pages that conflict |
| **Stale content** | Pages not updated since a newer source touched their topic |
| **Orphan pages** | Pages with no inbound `[[...]]` links |
| **Missing pages** | `[[wikilinks]]` that point to pages that don't exist yet |
| **Data gaps** | Topics where the wiki is thin; suggest sources to seek |
| **Cross-reference gaps** | Pages that should link to each other but don't |

Ask: "Should I file this lint report as `wiki/lint-{date}.md`?"
Append a lint entry to `wiki/log.md`.

---

## Important Notes

- **Raw sources are immutable.** The LLM reads from `raw/` but never modifies files there.
- **The LLM owns `wiki/`.** The user reads and browses; the LLM writes and maintains.
- **SCHEMA.md is co-evolved.** After discovering new conventions mid-session, update SCHEMA.md
  so future sessions inherit them.
- **Index-first for queries.** Always read `wiki/index.md` before drilling into pages — it is
  cheaper and faster than scanning all files.
- **Log is append-only.** Never edit past log entries; only append new ones.
- **File good answers.** Any rich query response should be offered back as a wiki page in
  `wiki/syntheses/` so explorations compound in the knowledge base.
- **Obsidian-compatible.** Use `[[page-name]]` wikilinks. The `wiki/` folder is a valid Obsidian
  vault root. YAML frontmatter is compatible with Dataview.
- **Git-friendly.** The entire brain directory is a valid git repo. Recommend `git init`.
- **Index file at ~100+ pages:** if `wiki/index.md` grows unwieldy, suggest the user set up
  [qmd](https://github.com/tobi/qmd) for hybrid BM25/vector search over the wiki.
