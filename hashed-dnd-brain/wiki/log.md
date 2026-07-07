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

## [2026-07-02] compress | 8 clusters consolidated
- Artifacts written (warm/L1): classes (h:1adaa84d), character-options (h:e2afd6e0), magic (h:b40981aa), dungeon-mastering (h:83ea1e3f), magic-items (h:c9d5277c), cosmology (h:fae762e7), monster-groups (h:c5a456a1), iconic-monsters (h:cad6c53b)
- Hot pages folded -> archive/: 57 (kept immutable, rehydratable)
- Hot path reduced: 68 -> 11 pages; addressable units 68 -> 19 (11 hot + 8 warm)
- Keys: 57 topic keys repointed to their warm artifact (keys UNCHANGED, still resolve); 6 new cluster keys added
- Key claims preserved; redundancy/restated detail dropped. Compression up to ~14:1 (iconic-monsters), ~13.6:1 (classes)

## [2026-07-02] age | swept 2 warm artifacts -> cold (L2 gist)
- Demoted (low-hit deep reference): iconic-monsters (h:cad6c53b), cosmology (h:fae762e7)
- Compressed L1 -> L2 gist; warm artifacts preserved in archive/warm/ (rehydratable); original pages remain in archive/
- Keys repointed to cold/ (UNCHANGED keys, still resolve); 23 topic keys now cold
- Tiers now: hot=11, warm=40, cold=23
