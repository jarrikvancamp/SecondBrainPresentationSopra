---
key: h:23323c83
topic: monster-stat-blocks
tier: hot
level: 0
last-accessed: 2026-07-02
access-count: 0
title: "Monster Stat Blocks (2024)"
tags: [monsters, stat-blocks, rules, 5e]
sources: [monster-manual-2025]
edition: "5e (2024)"
updated: 2026-07-02
---

A field-by-field guide to reading a 5e (2024) monster stat block. The 2024 format was redesigned for clarity and play speed compared to the 2014 layout. Fields only appear when they apply — if a monster has no special senses, that line is omitted.

See [[monsters-overview]] for creature types and Challenge Rating context.

---

## (1) Name, Size, Type, Alignment

The header line identifies the monster and gives three descriptors:

- **Size** — one of six categories: Tiny, Small, Medium, Large, Huge, or Gargantuan. Size determines which Hit Die the monster uses and affects space occupied in combat.
- **Creature type** — one of 14 types (Aberration, Beast, Celestial, Construct, Dragon, Elemental, Fey, Fiend, Giant, Humanoid, Monstrosity, Ooze, Plant, Undead). Optional descriptive tags in parentheses (e.g., "Undead (Vampire)") narrow the category for spells and features that care about subgroups.
- **Alignment** — a default suggestion for roleplaying the monster; the DM may change it freely.

---

## (2) Combat Highlights

### Armor Class (AC)
Combines the monster's natural armor rating, Dexterity modifier, any worn equipment, and other innate defenses. A higher AC means attacks miss more often.

### Initiative
Two values appear: a **modifier** (used when rolling) and a **score** in parentheses (can substitute for rolling entirely). The modifier is usually the Dexterity modifier; some monsters add their Proficiency Bonus on top.

### Hit Points (HP)
Shown as a flat number followed by a die expression in parentheses. Use the flat number for speed, or roll the expression for variety — never both.

The die size depends on creature size:

| Size | Hit Die | Avg HP per Die |
|------|---------|----------------|
| Tiny | d4 | 2½ |
| Small | d6 | 3½ |
| Medium | d8 | 4½ |
| Large | d10 | 5½ |
| Huge | d12 | 6½ |
| Gargantuan | d20 | 10½ |

The Constitution modifier is multiplied by the number of dice and added to the total.

### Speed
Walking speed in feet, plus any additional movement modes the monster has: **Burrow** (moves through earth), **Climb** (moves up surfaces), **Fly** (moves through the air; may note whether it can hover), **Swim** (moves through water).

---

## (3) Ability Scores

All six scores are listed with their modifiers:

| Score | Governs |
|-------|---------|
| **Strength (STR)** | Melee attacks, Athletics, carrying capacity |
| **Dexterity (DEX)** | Ranged attacks, AC, Acrobatics, Stealth, Initiative |
| **Constitution (CON)** | HP pool, Constitution saving throws |
| **Intelligence (INT)** | Arcana, History, Investigation, other knowledge skills |
| **Wisdom (WIS)** | Perception, Insight, Survival; many casters use WIS |
| **Charisma (CHA)** | Persuasion, Deception, Intimidation; some casters use CHA |

**Saving throws** appear only when the monster has proficiency that improves them beyond the raw modifier.

---

## (4) Other Details

### Skills
Each listed skill shows its total bonus (ability modifier + Proficiency Bonus, or doubled PB for expertise). Only skills where the monster has meaningful proficiency are listed.

### Resistances and Vulnerabilities
- **Resistance** — the monster takes half damage from the listed damage types.
- **Vulnerability** — the monster takes double damage from the listed types.

### Immunities
Lists damage types and conditions that have no effect on the monster. Damage immunities appear before condition immunities when both are present.

### Gear
Equipment the monster carries that a character can pick up and use after the monster is defeated; uses the *Player's Handbook* rules for that item when held by someone else. Equipment not listed here is considered supernatural or highly specialized and unusable once the monster is defeated.

### Senses
Passive Perception score plus any special senses:

| Sense | What It Allows |
|-------|---------------|
| **Darkvision** | Treats dim light as bright light, and darkness as dim light, within the stated range |
| **Blindsight** | Perceives surroundings without relying on sight at all, within range |
| **Truesight** | Sees through illusions, magical darkness, invisibility, and into the Ethereal Plane, within range |
| **Tremorsense** | Detects creatures and movement through vibrations in a solid surface, within range |

### Languages
The languages the monster can speak and/or understand. When a monster understands a language but cannot speak it, the entry notes that distinction. "None" means the monster has no language comprehension.

**Telepathy** — a magical form of mental communication; if a monster has it, the stat block specifies the maximum range.

### Challenge Rating and Proficiency Bonus
CR is a rough difficulty estimate for encounter planning; it also determines the monster's XP value and Proficiency Bonus (PB).

| CR Range | PB |
|----------|----|
| 0–4 | +2 |
| 5–8 | +3 |
| 9–12 | +4 |
| 13–16 | +5 |
| 17–20 | +6 |
| 21–24 | +7 |
| 25–28 | +8 |
| 29–30 | +9 |

---

## (5) Traits

Passive characteristics that are always active or activate under specific conditions (examples: Spider Climb, Pack Tactics, Magic Resistance). These are not actions; they just modify how the monster functions.

---

## (6) Actions

What the monster can do when it takes the Attack action or another action on its turn. Common sub-entries:

- **Melee / Ranged Attack** — lists the attack bonus, reach or range, and what happens on a hit (damage type and amount). Some attacks note effects on a miss or "hit or miss."
- **Multiattack** — allows the monster to make multiple attacks in one Attack action. Described as a discrete entry.
- **Saving Throw Effects** — specify the save type, DC, which creatures roll, and outcomes on failure vs. success. "Half damage only" on a success means half damage and no other riders.
- **Spellcasting** — lists spells the monster knows, its spellcasting ability, spell save DC, and spell attack bonus. Spells are cast at their minimum level unless otherwise noted.

### Limited-Use Notation
Three patterns govern how often an ability can be used:

| Notation | Meaning |
|----------|---------|
| **X/Day** | Usable X times; resets on a Long Rest |
| **Recharge X–Y** | Usable once; roll 1d6 at the start of each of the monster's turns — regained on X–Y; also recharges on a Short or Long Rest |
| **Recharge after Short or Long Rest** | Single use; reset requires a Short or Long Rest |

---

## (7) Bonus Actions

Optional additional actions available on the monster's turn, beyond its main action. Listed only when the monster has them.

---

## (8) Reactions and Legendary Actions

### Reactions
Triggered responses to specific events (e.g., reducing incoming damage, making an opportunity attack). Each reaction lists its trigger condition.

### Legendary Actions
Available only to powerful, typically solo monsters. A Legendary Action can be taken immediately after *another creature's* turn ends. The monster has a stated number of uses per round; all uses refresh at the start of the monster's own turn. The monster cannot use Legendary Actions while Incapacitated.

### Lair Actions and Regional Effects
Certain high-tier monsters have a lair that amplifies their power. **Lair Actions** occur on initiative count 20 (before all creatures with that initiative). **Regional Effects** alter the environment within a radius of the lair, persisting until the monster dies and fading over time afterward.

---

## Sources

[[monster-manual-2025]]
