# Operation Log

Append-only. Never edit past entries.

## [2026-07-02] init | Dungeons & Dragons Books Brain
- Created `dnd-brain/` with `raw/`, `wiki/` (`index.md`, `log.md`), and `SCHEMA.md`.
- Topic: Dungeons & Dragons books (rulebooks, sourcebooks, adventures, lore).
- Ready for first ingest — drop a book/excerpt into `raw/` or paste content.

## [2026-07-02] ingest | Player's Handbook (2024)
- Source page: sources/players-handbook-2024
- Pages created: playing-the-game, ability-scores-and-skills, combat, conditions, damage-and-healing, resting-and-exhaustion; classes-overview + 12 class pages; species, backgrounds, feats, equipment, spellcasting, spells-overview
- Key new claims: 2024 revision of 5e; species decoupled from ability score increases (granted by backgrounds); weapon mastery introduced
- Contradictions flagged: none
- Note: factual/transformative notes only; no verbatim book prose reproduced

## [2026-07-02] ingest | Dungeon Master's Guide (2024)
- Source page: sources/dungeon-masters-guide-2024
- Pages created: running-the-game, encounter-building, dm-toolbox, creating-adventures, creating-campaigns, bastions, magic-items, treasure, cosmology; entities for 8 planes (material-plane, feywild, shadowfell, ethereal-plane, astral-plane, nine-hells, the-abyss, elemental-planes); bag-of-holding, deck-of-many-things
- Key new claims: 2024 Bastion system; six magic-item rarities + 3-item attunement limit
- Contradictions flagged: none

## [2026-07-02] ingest | Monster Manual (2025)
- Source page: sources/monster-manual-2025
- Pages created: monsters-overview, monster-stat-blocks; iconic monsters (aboleth, beholder, lich, mind-flayers, tarrasque, vampires, dragons, dragon-turtle, mimic, owlbears, kobolds, goblins, gnolls); monster groups (demons, devils, giants, hags, goblinoids, undead-overview)
- Key new claims: creature types + Challenge Rating framework; stat blocks summarized, never copied verbatim
- Contradictions flagged: none

## [2026-07-07] lint | Wiki health-check
- Pages scanned: 70
- Contradictions flagged: none
- Broken/missing pages from wikilinks: none
- Orphan pages (no inbound links): none
- Stale pages by source-date comparison: none
- Findings:
  - 1 unresolved section anchor: `[[concepts/conditions#Unconscious]]` (target section does not exist).
  - Cross-reference gap: `concepts/classes-overview` links to only 6/12 class pages; `paladin`, `ranger`, `rogue`, `sorcerer`, `warlock`, and `wizard` appear as plain text in the class table.
  - Knowledge coverage is currently core-rulebook-heavy (PHB/DMG/MM) with no filed syntheses yet.

## [2026-07-07] lint | Findings fixed
- Updated `concepts/conditions` with a dedicated `## Unconscious` section to resolve the anchor from `concepts/damage-and-healing`.
- Updated `concepts/classes-overview` class table to wikilink all 12 classes (added `paladin`, `ranger`, `rogue`, `sorcerer`, `warlock`, `wizard`).
- Refreshed `updated:` frontmatter dates on both edited pages.
