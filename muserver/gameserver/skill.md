# GameServerInfo - Skill

## What it does

Sets the formulas of buffs, debuffs and special skills: how strong they are, how long they last, and how much of the effect each class receives. Skill damage, mana cost, range and requirements are in `Data/Skill/Skill.txt` (see [Skills](../data/skills.md)); this file covers the effect values.

## Where it lives

- `GameServer/Data/GameServerInfo - Skill.dat`
- `GameServerCS/Data/GameServerInfo - Skill.dat` (separate copy)

INI format, section `[GameServerInfo]`. Every key defaults to **0** when missing.

## How to read the formulas

| Kind of key | Meaning |
|---|---|
| `...ConstA`, `...ConstB`... | Parts of the formula. A constant that **divides** a stat works backwards: a bigger number gives a weaker effect. |
| `...TimeConstA`, `...TimeConstB` | Duration. Usually **seconds = TimeConstA + Energy / TimeConstB**, or just TimeConstA. |
| `...MaxRate` | Cap of the effect, in percent. |
| `...Rate<C>` | Percentage of the effect received by a **target** of class `<C>` (`100` = full effect). Lets you make a buff weaker on some classes. |

In the formulas, *Energy*, *Agility*, *Vitality*, *Strength* and *Command* are the caster's total stats (base + items).

{% hint style="danger" %}
Never leave a divisor at 0 (a missing key is 0). Keep every key of a skill present.
{% endhint %}

## Reload

`/reload skill`. Buffs already active keep their old values until they are recast.

---

## Mana Shield Skill Settings

Dark Wizard's Mana Shield (Soul Barrier).

| Key | Default | Values | Description |
|---|---|---|---|
| `ManaShieldConstA`, `ManaShieldConstB`, `ManaShieldConstC` | 0 | | Damage absorbed by mana (%) = A + Agility / B + Energy / C. |
| `ManaShieldMaxRate` | 0 | percent | Cap. Shipped: 50. |
| `ManaShieldRate<C>` (14 keys) | 0 | percent | Share of the effect for each target class. Shipped: DW 100, DK 24, others 25; newer classes 100. |
| `ManaShieldTimeConstA`, `ManaShieldTimeConstB` | 0 | | Seconds = A + Energy / B. |

## Defense Skill Settings

The Defense skill (shield block).

| Key | Default | Values | Description |
|---|---|---|---|
| `DefenseConstA` | 0 | percent | Damage reduction while blocking. Shipped: 30. |
| `DefenseTimeConstA` | 0 | seconds | How long the block lasts. Shipped: 4. |

## Heal Skill Settings

Fairy Elf's Heal.

| Key | Default | Values | Description |
|---|---|---|---|
| `HealConstA`, `HealConstB` | 0 | | HP healed = A + Energy / B. |

## Greater Defense Skill Settings

Fairy Elf's Greater Defense.

| Key | Default | Values | Description |
|---|---|---|---|
| `GreaterDefenseConstA`, `GreaterDefenseConstB` | 0 | | Defense added = A + Energy / B. |
| `GreaterDefenseRate<C>` (14 keys) | 0 | percent | Share of the effect for each target class. |
| `GreaterDefenseTimeConstA` | 0 | seconds | Duration. Shipped: 7200 (2 hours). |

## Greater Damage Skill Settings

Fairy Elf's Greater Damage.

| Key | Default | Values | Description |
|---|---|---|---|
| `GreaterDamageConstA`, `GreaterDamageConstB` | 0 | | Damage added = A + Energy / B. |
| `GreaterDamageRate<C>` (14 keys) | 0 | percent | Share of the effect for each target class. |
| `GreaterDamageTimeConstA` | 0 | seconds | Duration. |

## Summon Monster Skill Settings

Fairy Elf summons.

| Key | Default | Values | Description |
|---|---|---|---|
| `SummonMonster1` to `SummonMonster7` | 0 | monster index | Monster created by summon skills 1 to 7 (Goblin, Stone Golem, Assassin, Elite Yeti, Dark Knight, Bali, Soldier in the original game). Shipped: 26, 32, 21, 20, 10, 150, 151. |
| `SummonMonster8` | 0 | monster index | Monster of the Summon Satyros skill. Shipped: 353. |

## Greater Life Skill Settings

Dark Knight's Swell Life.

| Key | Default | Values | Description |
|---|---|---|---|
| `GreaterLifeConstA`, `GreaterLifeConstB`, `GreaterLifeConstC` | 0 | | Max HP bonus (%) = A + Vitality / B + Energy / C. |
| `GreaterLifeMaxRate` | 0 | percent | Cap. |
| `GreaterLifeRate<C>` (14 keys) | 0 | percent | Share of the effect for each target class. |
| `GreaterLifeTimeConstA`, `GreaterLifeTimeConstB` | 0 | | Seconds = A + Energy / B. |

## Fire Slash Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `FireSlashConstA`, `FireSlashConstB` | 0 | | Target defense reduction (%) = A + Strength / B. |
| `FireSlashMaxRate` | 0 | percent | Cap. |
| `FireSlashTimeConstA` | 0 | seconds | Duration. |

## Greater Critical Damage Skill Settings

Dark Lord's Critical Damage.

| Key | Default | Values | Description |
|---|---|---|---|
| `GreaterCriticalDamageConstA`, `GreaterCriticalDamageConstB` | 0 | | Critical damage bonus = Energy / A + Command / B. |
| `GreaterCriticalDamageTimeConstA`, `GreaterCriticalDamageTimeConstB` | 0 | | Seconds = A + Energy / B. |

## Stern Skill Settings

Stun.

| Key | Default | Values | Description |
|---|---|---|---|
| `SternTimeConstA` | 0 | seconds | Stun duration. |

## Greater Mana Skill Settings

Swell Mana.

| Key | Default | Values | Description |
|---|---|---|---|
| `GreaterManaConstA` | 0 | percent | Max mana bonus. |
| `GreaterManaTimeConstA` | 0 | seconds | Duration. |

## Invisibility Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `InvisibilityTimeConstA` | 0 | seconds | Duration (Castle Siege invisibility). |

## Brand Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `BrandTimeConstA` | 0 | seconds | Duration of the Brand effect. |

## Infinity Arrow Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `InfinityArrowTimeConstA` | 0 | seconds | Duration. Shipped: 86640 (about 24 hours). See also `InfinityArrow_AL*` in [Common](common.md#infinity-arrow-switch). |

## Magic Damage Immunity Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MagicDamageImmunityTimeConstA` | 0 | seconds | Duration of immunity to magic damage. |

## Physi Damage Immunity Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `PhysiDamageImmunityTimeConstA` | 0 | seconds | Duration of immunity to physical damage. |

## Drain Life Skill Settings

Summoner's Drain Life.

| Key | Default | Values | Description |
|---|---|---|---|
| `DrainLifeConstA`, `DrainLifeConstB` | 0 | | HP drained = damage × A / 100 + Energy / B. |

## Reflect Damage Skill Settings

Summoner's Damage Reflection.

| Key | Default | Values | Description |
|---|---|---|---|
| `ReflectDamageConstA`, `ReflectDamageConstB` | 0 | | Reflect (%) = A + Energy / B. |
| `ReflectDamageMaxRate` | 0 | percent | Cap. |
| `ReflectDamageRate<C>` (14 keys) | 0 | percent | Share of the effect for each target class. |
| `ReflectDamageTimeConstA`, `ReflectDamageTimeConstB` | 0 | | Seconds = A + Energy / B. |

The reflected damage is then multiplied by `ReflectDamageRatePvP` / `ReflectDamageRatePvM` of [Character](character.md#character-damage-rate-settings).

## Sword Power Skill Settings

Summoner's Berserker.

| Key | Default | Values | Description |
|---|---|---|---|
| `SwordPowerConstA` | 0 | divisor | Damage and wizardry bonus (%) = Energy / A. |
| `SwordPowerConstB` | 0 | divisor | Defense and life penalty (%) = 40 − Energy / B, never below 10. |
| `SwordPowerMaxRate` | 0 | percent | Cap of the bonus. |
| `SwordPowerTimeConstA`, `SwordPowerTimeConstB` | 0 | | Seconds = A + Energy / B. |

## Sleep Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `SleepConstA`, `SleepConstB` | 0 | | Success chance (%) = A + Energy / B. |
| `SleepTimeConstA`, `SleepTimeConstB` | 0 | | Seconds = A + Energy / B. |
| `SleepMaxTime` | 0 | seconds | Cap of the duration. |

## Lesser Defense Skill Settings

Summoner's Innovation (defense debuff).

| Key | Default | Values | Description |
|---|---|---|---|
| `LesserDefenseConstA`, `LesserDefenseConstB` | 0 | | Success chance (%) = A + Energy / B. |
| `LesserDefenseConstC`, `LesserDefenseConstD` | 0 | | Defense reduction (%) = C + Energy / D. |
| `LesserDefenseMaxRate` | 0 | percent | Cap of the reduction. |
| `LesserDefenseTimeConstA`, `LesserDefenseTimeConstB` | 0 | | Seconds = A + Energy / B. |

## Lesser Damage Skill Settings

Summoner's Weakness (damage debuff).

| Key | Default | Values | Description |
|---|---|---|---|
| `LesserDamageConstA`, `LesserDamageConstB` | 0 | | Success chance (%) = A + Energy / B. |
| `LesserDamageConstC`, `LesserDamageConstD` | 0 | | Damage reduction (%) = C + Energy / D. |
| `LesserDamageMaxRate` | 0 | percent | Cap of the reduction. |
| `LesserDamageTimeConstA`, `LesserDamageTimeConstB` | 0 | | Seconds = A + Energy / B. |

## Sahamutt Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `SahamuttConstA` | 0 | percent | Damage of the Sahamutt (Explosion) follow-up hit, as a percentage of the base hit. |

## Neil Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `NeilConstA` | 0 | percent | Damage of the Neil (Requiem) follow-up hit, as a percentage of the base hit. |

## Magic Circle Skill Settings

Dark Wizard's Expansion of Wizardry.

| Key | Default | Values | Description |
|---|---|---|---|
| `MagicCircleConstA` | 0 | percent | Wizardry bonus = (Energy / `DWMagicDamageMinConstA`) × A / 100. |
| `MagicCircleTimeConstA` | 0 | seconds | Duration. |

## Shield Recover Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `ShieldRecoverConstA`, `ShieldRecoverConstB` | 0 | | SD restored = (skill level + 1) × A + Energy / B. |

## Dragon Slayer Skill Settings

Rage Fighter's Dragon Slayer.

| Key | Default | Values | Description |
|---|---|---|---|
| `DragonSlayerConstA`, `DragonSlayerConstB` | 0 | | Chance (%) of the special effect = A + Energy / B. |
| `DragonSlayerConstC`, `DragonSlayerConstD` | 0 | | Effect value (%) = C + Energy / D. |
| `DragonSlayerMaxRate` | 0 | percent | Cap. |

## Greater Ignore Defense Rate Skill Settings

Rage Fighter's Ignore Defense.

| Key | Default | Values | Description |
|---|---|---|---|
| `GreaterIgnoreDefenseRateConstA`, `GreaterIgnoreDefenseRateConstB` | 0 | | Chance (%) to ignore defense = A + Energy / B. |
| `GreaterIgnoreDefenseRateTimeConstA`, `...TimeConstB` | 0 | | Seconds = A + Energy / B. |

## Fitness Skill Settings

Rage Fighter's Increase Health.

| Key | Default | Values | Description |
|---|---|---|---|
| `FitnessConstA`, `FitnessConstB` | 0 | | Vitality bonus = A + Energy / B. |
| `FitnessTimeConstA`, `FitnessTimeConstB` | 0 | | Seconds = A + Energy / B. |

## Greater Defense Success Rate Skill Settings

Rage Fighter's Increase Block.

| Key | Default | Values | Description |
|---|---|---|---|
| `GreaterDefenseSuccessRateConstA`, `...ConstB` | 0 | | Defense rate bonus = A + Energy / B. |
| `GreaterDefenseSuccessRateTimeConstA`, `...TimeConstB` | 0 | | Seconds = A + Energy / B. |

## Party Heal Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `PartyHealConstA`, `PartyHealConstB` | 0 | | HP healed per party member = A + Energy / B. |

## Bless Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `BlessConstA` | 0 | divisor | Bonus = Energy / A. |
| `BlessTimeConstA` | 0 | seconds | Duration. |

## Blind Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `BlindConstA`, `BlindConstB` | 0 | | Success chance (%) = A + Energy / B. |
| `BlindConstC` | 0 | percent | Strength of the blind effect (attack rate reduction). |
| `BlindTimeConstA` | 0 | seconds | Duration. |

## Earth Prison Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `EarthPrisonConstA` | 0 | percent | Chance to immobilize the target. |

## Iron Defense Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `IronDefenseConstA`, `IronDefenseConstB` | 0 | | The two values of the Iron Defense buff (defense and HP). Shipped: 800 / 800. |
| `IronDefenseTimeConstA` | 0 | seconds | Duration. |

## Blood Howling Skill Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `BloodHowlingConstA` | 0 | percent | Chance of the bleeding effect on hit. |
| `BloodHowlingConstB` | 0 | damage | Damage of the bleeding effect. |
| `BloodHowlingTimeConstA` | 0 | seconds | Duration of the buff. |

---

## Example

Make the Elf's Greater Damage weaker on Magic Gladiators and Dark Knights and last 1 hour:

```ini
GreaterDamageConstA = 3
GreaterDamageConstB = 7
GreaterDamageRateDK = 9
GreaterDamageRateMG = 9
GreaterDamageTimeConstA = 3600
```

## Rules and pitfalls

- **Durations are in seconds.** The shipped buffs last 7200 seconds (2 hours); the original game used much shorter times.
- **Class rates apply to the target**, not the caster. `ManaShieldRateDK = 24` means a Dark Knight receiving a Wizard's Mana Shield gets 24 % of the effect.
- **Caps first, rates after**: the `MaxRate` cap applies to the formula, then the class rate reduces it.
- **The newer-class keys** (GL to KIN) are only in the normal GameServer copy. The shipped GameServerCS copy has the Season 6 classes only and also lacks the Party Heal, Bless, Blind, Earth Prison, Iron Defense and Blood Howling blocks, so those skills have 0 values on Castle Siege maps unless you add them.
