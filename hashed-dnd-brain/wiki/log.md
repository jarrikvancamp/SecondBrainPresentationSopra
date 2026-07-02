# Operation Log

Append-only. Never edit past entries.

## [2026-07-02] init | Hashed D&D Books Brain
- Created `hashed-dnd-brain/` with `raw/` (gitignored), `archive/`, `wiki/hot|warm|cold`, `index.md`, `hashes.md`, `log.md`, and `SCHEMA.md`.
- Topic: Dungeons & Dragons books. Tiered memory (hot/warm/cold) + Knowledge Compression Hashing.
- Config: `hashed-second-brain.md` (coexists with the non-hashed `dnd-brain`, whose config is `second-brain.md`).

## [2026-07-02] ingest | Player's Handbook (2024), Dungeon Master's Guide (2024), Monster Manual (2025)
- Ingested the three core books into the **HOT** tier as 68 L0 pages — transformative, own-worded factual notes (no verbatim book text).
- Hot pages created: 3 sources, 35 concepts, 30 entities.
- Keys registered: 68 stable topic keys in `hashes.md` (all `tier=hot`, `level=0`, computed as `h:` + SHA-1(slug)[:8]).
- Contradictions flagged: none.
- Note: nothing compressed yet — all knowledge is on the hot path. Run **compress** to consolidate clusters (e.g. the 12 classes, the bestiary) into warm, hash-addressed artifacts; then **age** to sweep stale artifacts to cold.
