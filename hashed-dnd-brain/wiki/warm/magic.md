---
key: h:b40981aa
topic: magic
tier: warm
level: 1
sources: [players-handbook-2024]
consolidates: [archive/concepts/spellcasting.md, archive/concepts/spells-overview.md]
edition: "5e (2024)"
last-compressed: 2026-07-02
last-accessed: 2026-07-02
access-count: 0
updated: 2026-07-02
---

# Magic (Spellcasting & Spells)

All spellcasting [[classes]] use the same core rules unless a class feature overrides them.

## Spell Slots & Cantrips

- **Levels 0–9**: level 0 = cantrips (free, unlimited, scale at PC levels 5/11/17); levels 1–9 require a slot.
- **Slot expenditure**: casting a level 1–9 spell consumes one slot of that spell's level or higher.
- **Recovery**: all slots restore on a **Long Rest**.
- Casting limit: one slot per turn — cannot use the Magic action and a Bonus Action slot in the same turn.

## Prepared vs Known

| Class | When List Changes | Spells Swapped |
|---|---|---|
| Bard, Sorcerer, Warlock | On level gain | 1 spell |
| Cleric, Druid, Wizard | After Long Rest | Any number |
| Paladin, Ranger | After Long Rest | 1 spell |

Always-prepared spells from class features don't count against the prepared total. Wizards ritual-cast from their spellbook without preparation.

## Ritual Casting

Spells tagged **Ritual** may be cast without a slot by adding **10 minutes** to the casting time. Caster must have the spell prepared (Wizard exception: ritual from spellbook only).

## Upcasting

Casting a spell using a higher-level slot than its base level improves it — extra damage dice, targets, duration, or other effects as noted in the spell's description.

## Components

| Type | Requirement |
|---|---|
| **V** (Verbal) | Must speak aloud; blocked by silence / gag |
| **S** (Somatic) | Free hand required for gestures |
| **M** (Material) | Specific item; a **Component Pouch** or **Spellcasting Focus** substitutes for materials with no listed gold cost |

Costly or consumed materials can never be replaced by a pouch or focus.

## Concentration

- Only **one** Concentration spell active at a time; starting a new one ends the previous instantly.
- Taking damage → **Con save** (DC = max(10, ½ damage taken)) or lose Concentration.
- Incapacitation or death breaks Concentration automatically.
- War Caster feat grants Advantage on Concentration saves.

## Spell Save DC & Attack Modifier

| Stat | Formula |
|---|---|
| **Spell Save DC** | 8 + Proficiency Bonus + Spellcasting Ability mod |
| **Spell Attack Mod** | Proficiency Bonus + Spellcasting Ability mod |

Spellcasting ability: Int (Wizard) · Wis (Cleric, Druid) · Cha (Sorcerer, Warlock, Bard).

## Casting Time & Duration

- **Typical**: Magic action (equivalent to Attack action for casters).
- **Bonus Action / Reaction**: some spells specify this.
- **1 min+**: requires Magic action each turn + Concentration; cancelled (no slot lost) if Concentration breaks.
- **Duration types**: Instantaneous · Time-span (rounds/min/hr, dismissible) · Concentration.

## Range & Targeting

Self / Touch / Distance (ft). Total cover blocks targeting. Combining spells: different spells stack; same spell cast twice on same target does not — only most potent applies.

---

## Spells Overview

### The Eight Schools

| School | Typical Effect |
|---|---|
| **Abjuration** | Wards, barriers, harm prevention |
| **Conjuration** | Summoning, teleportation, object movement |
| **Divination** | Detection, foresight, hidden info |
| **Enchantment** | Mind influence and control |
| **Evocation** | Raw energy — damage or healing |
| **Illusion** | False images, sensory deception |
| **Necromancy** | Life/death boundary, undead, vitality drain |
| **Transmutation** | Physical transformation of creatures or objects |

Schools are mostly descriptive; some class features key off them mechanically (e.g., Abjuration Wizard subclass).

### Class Spell Lists

Each spellcasting class owns a fixed list; preparation is restricted to that list. Magic Initiate and similar features grant cross-list access without changing class membership.

### Iconic Spells by Level

| Lvl | Spell | School | Effect |
|---|---|---|---|
| 0 | Eldritch Blast | Evocation | 1d10 force beam; extra beams at higher PC levels (Warlock) |
| 0 | Fire Bolt | Evocation | 1d10 fire at range (Sorcerer, Wizard) |
| 0 | Guidance | Divination | +1d4 to one ability check (Cleric, Druid) |
| 0 | Prestidigitation | Transmutation | Minor tricks: clean, light, produce trinket (Bard, Sorc, Wlk, Wiz) |
| 1 | Magic Missile | Evocation | 3 unerring 1d4+1 force bolts; more when upcast (Sorc, Wiz) |
| 1 | Mage Armor | Abjuration | AC = 13+Dex for 8 hr, unarmored target (Sorc, Wiz) |
| 1 | Cure Wounds | Evocation | 2d8+mod HP restored on touch; upcast improves (Bard, Clr, Drd, Pal, Rgr) |
| 1 | Sleep | Enchantment | Knocks out creatures by HP total, lowest first; no save (Bard, Sorc, Wiz) |
| 2 | Misty Step | Conjuration | Bonus Action teleport 30 ft (Sorc, Wlk, Wiz) |
| 2 | Invisibility | Illusion | Invisible until attack/cast; Concentration (Bard, Sorc, Wlk, Wiz) |
| 2 | Shatter | Evocation | 10-ft sphere, 3d8 thunder (Bard, Sorc, Wlk, Wiz) |
| 3 | Fireball | Evocation | 20-ft radius 8d6 fire, Dex save half (Sorc, Wiz) |
| 3 | Counterspell | Abjuration | Reaction; auto-stops spells ≤ lvl 3 (Sorc, Wlk, Wiz) |
| 4 | Polymorph | Transmutation | Transform creature into beast, uses beast stats; Concentration (Brd, Drd, Sorc, Wiz) |
| 4 | Greater Invisibility | Illusion | Stays invisible through attacking/casting; Concentration (Brd, Sorc, Wiz) |
| 5 | Hold Monster | Enchantment | Paralyze any creature type, Wis save; Concentration (Brd, Sorc, Wlk, Wiz) |
| 5 | Mass Cure Wounds | Evocation | 6 creatures within 30 ft heal 5d8+mod each (Brd, Clr, Drd) |
| 6 | Disintegrate | Transmutation | 10d6+40 force; 0 HP = dust (Sorc, Wiz) |
| 6 | Chain Lightning | Evocation | 10d8 to primary + arcs to 3 more targets (Sorc, Wiz) |
| 7 | Teleport | Conjuration | Instantly move 8 willing creatures to known destination (Brd, Sorc, Wiz) |
| 8 | Dominate Monster | Enchantment | Control any creature type, Wis save each turn; Concentration (Brd, Sorc, Wlk, Wiz) |
| 9 | Wish | Conjuration | Replicate any spell ≤ lvl 8 or attempt any effect; potential permanent cost (Sorc, Wiz) |
| 9 | Meteor Swarm | Evocation | 4 × 40-ft blasts, each 20d6 fire+20d6 bludgeoning, Dex save half (Sorc, Wiz) |
| 9 | Time Stop | Transmutation | Take 1d4+1 extra turns while all others freeze (Sorc, Wiz) |

---

## Consolidates
Absorbs (archived, rehydratable): spellcasting, spells-overview.

## Sources
- [[players-handbook-2024]]
