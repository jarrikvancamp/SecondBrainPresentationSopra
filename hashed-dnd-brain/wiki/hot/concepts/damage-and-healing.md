---
key: h:53aebfa4
topic: damage-and-healing
tier: hot
level: 0
last-accessed: 2026-07-02
access-count: 0
title: "Damage and Healing"
tags: [rules, damage, healing, hit-points, death-saving-throw, temporary-hp, resistance, vulnerability]
sources: [players-handbook-2024]
edition: "5e (2024)"
updated: 2026-07-02
---

Hit Points represent a creature's durability and will to survive; damage reduces them and healing restores them.

## Hit Points

- Every creature has a **Hit Point maximum** (HP max) and current HP ranging from 0 to that maximum.
- Damage subtracts from current HP; losing HP has no mechanical effect until the creature reaches 0.
- A creature at half HP or fewer is **Bloodied** — a new 2024 status marker with no automatic mechanical effect on its own, but some features reference it.

## Damage Types

The game defines the following damage types; they have no inherent rules but are used by Resistance, Vulnerability, and Immunity:

| Physical | Elemental | Energy / Other |
|---------|-----------|---------------|
| Bludgeoning | Acid | Force |
| Piercing | Cold | Necrotic |
| Slashing | Fire | Psychic |
| | Lightning | Radiant |
| | Thunder | Poison |

## Resistance, Vulnerability, and Immunity

| Modifier | Effect |
|----------|--------|
| **Resistance** | Damage of that type is halved (round down). |
| **Vulnerability** | Damage of that type is doubled. |
| **Immunity** | No damage taken from that type; immune creatures also can't be affected by a condition they're immune to. |

- Multiple instances of the same modifier don't stack.
- Order of operations: other adjustments first → Resistance second → Vulnerability third.

## Rolling Damage

- Roll the weapon's or spell's damage dice and add the same ability modifier used for the attack roll.
- **Critical Hit** (natural 20 on an attack roll): Roll all damage dice twice, then add modifiers once.
- If a penalty would reduce damage below 0, the result is 0 (never negative).

## Dropping to 0 HP

### Instant Death

A character dies immediately if:
- Remaining damage (beyond 0 HP) equals or exceeds their HP maximum (Massive Damage rule).
- Their HP maximum is reduced to 0 by an effect.
- Monsters die instantly at 0 HP (DM may ignore this for named monsters).

### Falling Unconscious

If a character reaches 0 HP without dying instantly, they gain the [[conditions#Unconscious|Unconscious]] condition and must begin making **Death Saving Throws**.

### Death Saving Throws

- At the start of each turn at 0 HP: roll 1d20 (no modifiers).
  - **10 or higher** → 1 success.
  - **9 or lower** → 1 failure.
  - **Natural 1** → 2 failures.
  - **Natural 20** → Regain 1 HP immediately (ends the process).
- **3 successes** → Stable (no longer makes saves but remains Unconscious).
- **3 failures** → Dead.
- Successes and failures need not be consecutive; both totals reset to 0 when the creature regains any HP.
- Taking any damage at 0 HP counts as a failure (Critical Hit damage = 2 failures).

### Stabilizing a Downed Creature

- Use the Help action and succeed on a DC 10 Wisdom (Medicine) check.
- A Stable creature doesn't roll Death Saving Throws; it remains Unconscious but regains 1 HP after 1d4 hours if not otherwise healed.

## Healing

- Add restored HP to current HP; cannot exceed HP maximum.
- HP can be restored by magic (spells, potions), or by resting (see [[resting-and-exhaustion]]).

## Temporary Hit Points

- Act as a buffer: damage depletes Temp HP first; leftover damage carries to real HP.
- Cannot be healed or added to real HP; receiving new Temp HP replaces the old total (no stacking).
- Duration: until depleted or until the end of a Long Rest.
- Being at 0 HP and receiving Temp HP does not restore consciousness — only true healing does.

## Related Pages

- [[combat]]
- [[conditions]]
- [[resting-and-exhaustion]]

## Sources
- [[players-handbook-2024]]
