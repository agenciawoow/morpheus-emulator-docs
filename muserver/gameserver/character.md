# GameServerInfo - Character

## What it does

Controls character balance: how much damage each class deals in PvP and PvM, class-against-class damage, the formulas that turn stats into damage, defense, attack rate and speed, HP/MP/BP/SD regeneration, and the bonuses of pets and guardians.

{% hint style="info" %}
If [Balance Class](../customs/balance-class.md) is on (`CustomBalanceClassSwitch = 1` in Custom.dat), PvP damage between players is calculated by that system instead of the PvP rates on this page. The PvM rates, recovery rates and stat formulas here still apply.
{% endhint %}

## Where it lives

- `GameServer/Data/GameServerInfo - Character.dat`
- `GameServerCS/Data/GameServerInfo - Character.dat` (separate copy, see [Castle Siege server](castle-siege-server.md))

INI format, section `[GameServerInfo]`. Class codes (`DW`, `DK`, `FE`, `MG`, `DL`, `SU`, `RF`, `GL`, `RW`, `SL`, `GC`, `KUN`, `LEM`, `KIN`) are listed in the [GameServer overview](README.md#class-abbreviations). In the tables, `<C>` stands for any class code.

## How to read the formulas

Most keys in the lower half of the file are **divisors**: a stat is divided by the constant. A **bigger** constant gives a **smaller** result. Example: `DKPhysiDamageMinConstA = 8` means minimum damage = Strength / 8. Changing it to 6 raises a Dark Knight's base minimum damage by a third.

Rates (keys ending in `Rate`) are **percentages**: `100` = unchanged, `120` = +20 %, `80` = −20 %.

{% hint style="danger" %}
Never set a divisor to `0`. A missing divisor key is also 0. Keep every `Const` key present with a value of at least 1.
{% endhint %}

---

## Character Damage Rate Settings

Global multipliers. All default to 0 if missing.

| Key | Default | Values | Description |
|---|---|---|---|
| `GeneralDamageRatePvP` | 0 | percent | All player-versus-player damage. Shipped: 100. |
| `GeneralDamageRatePvM` | 0 | percent | All player-versus-monster damage. Shipped: 105. |
| `DuelDamageRate` | 0 | percent | Extra multiplier in duels. |
| `GensDamageRate` | 0 | percent | Extra multiplier in Gens battles. |
| `CustomArenaDamageRate` | 0 | percent | Extra multiplier inside [Custom Arena](../customs/custom-arena.md) maps. |
| `ChaosCastleDamageRate` | 0 | percent | Extra multiplier inside Chaos Castle. Shipped: 85. |
| `IllusionTempleDamageRate` | 0 | percent | Extra multiplier inside Illusion Temple. Shipped: 85. |
| `CastleSiegeDamageRate1` | 0 | percent | During Castle Siege, damage between **enemies** (different sides). Shipped: 60. |
| `CastleSiegeDamageRate2` | 0 | percent | During Castle Siege, damage between **allies** (same side). `0` = no friendly fire. |
| `CastleSiegeDamageRate3` | 0 | percent | Damage to the castle gate and guardian statues. Shipped: 5. |
| `ReflectDamageRatePvP` | 0 | percent | Damage reflected back to a player attacker (reflect options and skills). Shipped: 16. |
| `ReflectDamageRatePvM` | 0 | percent | Damage reflected back to a monster attacker. Shipped: 100. |

Only one of the event/duel/Gens/arena multipliers applies to a hit, in this order: duel, Gens, Custom Arena, Chaos Castle, Illusion Temple, Castle Siege.

## Character Class Damage Rate Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>DamageRatePvP` | 0 | percent | PvP damage dealt by this class, applied after `GeneralDamageRatePvP`. |
| `<C>DamageRatePvM` | 0 | percent | PvM damage dealt by this class, applied after `GeneralDamageRatePvM`. |

The shipped file has `DamageRatePvP` for DW, DK, FE, MG, DL, SU and RF, and `DamageRatePvM` for all 14 classes.

{% hint style="warning" %}
`GLDamageRatePvP`, `RWDamageRatePvP`, `SLDamageRatePvP`, `GCDamageRatePvP`, `KUNDamageRatePvP`, `LEMDamageRatePvP` and `KINDamageRatePvP` are **not in the shipped file**, so they are 0. If your client has these classes and Balance Class is off, add them with `100`, or those classes deal no PvP damage.
{% endhint %}

## Character Class x Class Damage Rate Settings

One key per attacker/defender pair:

| Key | Default | Values | Description |
|---|---|---|---|
| `<Attacker>DamageRateTo<Defender>` | 0 | percent | PvP damage when a character of class *Attacker* hits a character of class *Defender*. Applied after the two rates above. |

Examples: `DWDamageRateToDK = 140` (Dark Wizard deals 140 % to Dark Knights), `RFDamageRateToDW = 25`.

The shipped file has all 14 × 14 pairs. One key in it is misspelled: `LEMDamageRateToKUM` is ignored; the real key is `LEMDamageRateToKUN`. Fix the name if you use those classes.

Final PvP damage (without Balance Class) = base damage × `GeneralDamageRatePvP` × `<A>DamageRatePvP` × `<A>DamageRateTo<D>` × (one event/duel rate), each as a percentage.

## Character Stuck Rate Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>DamageStuckRate` | 0 | 0 to 100 | Chance (percent) that a hit received by a character of this class interrupts them with the hit-stun animation. Not applied while riding Uniria, Dinorant, Dark Horse or Fenrir. Shipped: 0 for all classes. |

## Character Damage Multiplier Settings

The class "damage multiplier" bonus shown in the character window.

| Key | Default | Values | Description |
|---|---|---|---|
| `DKDamageMultiplierConstA` | 0 | divisor | Dark Knight skill damage multiplier = 200 % + Energy / A. |
| `DLDamageMultiplierConstA` | 0 | divisor | Dark Lord: 200 % + Energy / A. |
| `GLDamageMultiplierConstA`, `SLDamageMultiplierConstA` | 1 | divisor | Grow Lancer / Slayer: 200 % + Energy / A. |
| `RFDamageMultiplierConstA` | 0 | divisor | Rage Fighter, third multiplier: 100 % + Agility / A + Energy / C. |
| `RFDamageMultiplierConstB` | 0 | divisor | Rage Fighter, first multiplier: 50 % + Vitality / B. |
| `RFDamageMultiplierConstC` | 0 | divisor | Rage Fighter, second multiplier: 50 % + Energy / C. |
| `DKDamageMultiplierMaxRate`, `DLDamageMultiplierMaxRate`, `RFDamageMultiplierMaxRate` | 0 | percent | Cap of the multiplier. Shipped: 500. |
| `GLDamageMultiplierMaxRate`, `SLDamageMultiplierMaxRate` | 1 | percent | Cap for Grow Lancer and Slayer. |

## Character Dark Spirit Settings

The Dark Lord's Dark Raven.

| Key | Default | Values | Description |
|---|---|---|---|
| `DarkSpiritRangeAttackRate` | 0 | percent | Chance that the raven's attack hits an area instead of one target. |
| `DarkSpiritCriticalDamageRate` | 0 | percent | Chance of critical damage (master skills add to it). |
| `DarkSpiritExcellentDamageRate` | 0 | percent | Chance of excellent damage (master skills add to it). |
| `DarkSpiritAttackDamageMinConstA`, `...MinConstB`, `...MinConstC` | 0 | | Minimum damage = A + raven level × B + Command / C. |
| `DarkSpiritAttackDamageMaxConstA`, `...MaxConstB`, `...MaxConstC` | 0 | | Maximum damage = A + raven level × B + Command / C. |
| `DarkSpiritAttackSpeedConstA` to `...ConstD` | 0 | | Attack speed = A + (raven level × B) / C + Command / D. |
| `DarkSpiritAttackSuccessRateConstA` to `...ConstC` | 0 | | Attack rate = A + (raven level × B) / C. |

## Character Guardian Inc Damage Settings

Damage bonus (percent) while the guardian or pet is equipped.

| Key | Default | Values | Description |
|---|---|---|---|
| `SatanIncDamageConstA` | 0 | percent | Satan. Shipped: 4. |
| `DinorantIncDamageConstA` | 0 | percent | Dinorant. Shipped: 7. |
| `BlackFenrirIncDamageConstA` | 0 | percent | Black Fenrir (destroy). Shipped: 7. |
| `DemonIncDamageConstA` | 0 | percent | Demon. Shipped: 6. |
| `SkeletonIncDamageConstA` | 0 | percent | Pet Skeleton. Shipped: 4. |

## Character Guardian Dec Damage Settings

Damage reduction (percent) while the guardian or pet is equipped.

| Key | Default | Values | Description |
|---|---|---|---|
| `AngelDecDamageConstA` | 0 | percent | Guardian Angel. |
| `DinorantDecDamageConstA` | 0 | percent | Dinorant. |
| `DinorantDecDamageConstB` | 0 | percent | Extra reduction when the Dinorant has the damage-absorb option. |
| `DarkHorseDecDamageConstA` / `...ConstB` | 0 | | Dark Horse absorb = (A + horse level) / B percent. |
| `BlueFenrirDecDamageConstA` | 0 | percent | Blue Fenrir (protect). |
| `MariaDecDamageConstA` | 0 | percent | Maria (pet). |

## Character Combo Skill Damage Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `ComboDamageConstA` | 1 | divisor | Combo bonus = ((Strength + Agility + Energy) / A) × B. |
| `ComboDamageConstB` | 1 | multiplier | See above. |

## Character Earthquake Skill Damage Settings

Dark Lord's Earthquake (needs Dark Horse).

| Key | Default | Values | Description |
|---|---|---|---|
| `EarthquakeDamageConstA` | 1 | divisor | Damage = Strength / A + Command / B + horse level × C. |
| `EarthquakeDamageConstB` | 1 | divisor | See above. |
| `EarthquakeDamageConstC` | 1 | multiplier | See above. |

## Character Electric Spark Skill Damage Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `ElectricSparkDamageConstA` | 0 | divisor | Electric Spark bonus = Command / A + B. |
| `ElectricSparkDamageConstB` | 0 | flat | See above. |

## Character Dark Lord Skill Damage Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `DLSkillDamageConstA` | 0 | divisor | Dark Lord skill bonus = Strength / A + Energy / B. |
| `DLSkillDamageConstB` | 0 | divisor | See above. |

## Character Nova Skill Damage Settings

Dark Wizard's Nova (charged).

| Key | Default | Values | Description |
|---|---|---|---|
| `NovaDamageConstA` | 0 | multiplier | Bonus = A × (charge stage × B) + Strength / C. |
| `NovaDamageConstB` | 0 | multiplier | See above. |
| `NovaDamageConstC` | 0 | divisor | See above. |

## Character HP / MP / BP / SD Recovery Settings

Four blocks with one key per class.

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>HPRecoveryRate` | 0 | percent per tick | Base HP regeneration. Item options and buffs add to it, and +5 is added after 5 seconds without taking damage. |
| `<C>MPRecoveryRate` | 0 | percent per tick | Base mana regeneration. Shipped: 4. |
| `<C>BPRecoveryRate` | 0 | percent per tick | Base AG (stamina) regeneration. Shipped: 3 to 5. |
| `<C>SDRecoveryRate` | 0 | percent per tick | Base SD regeneration. Shipped: 4. |

## Character Plasma Storm Skill Damage Settings

Fenrir's Plasma Storm skill.

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>PlasmaStormDamageConstA` | 0 | divisor | Base damage = Strength / A + Agility / B + Vitality / C + Energy / D. Read for DW to GC. |
| `<C>PlasmaStormDamageConstB` | 0 | divisor | See above. |
| `<C>PlasmaStormDamageConstC` | 0 | divisor | See above. |
| `<C>PlasmaStormDamageConstD` | 0 | divisor | See above. |
| `DLPlasmaStormDamageConstE` | 0 | divisor | Dark Lord only: + Command / E. |

The `KUN`, `LEM` and `KIN` keys of this block are in the file but not read.

## Character Damage Settings

Base damage from stats. All are divisors.

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>PhysiDamageMinConstA` | 0 | divisor | Minimum physical damage = Strength / A. For Elf without a bow and for Summoner: (Strength + Agility) / A. |
| `<C>PhysiDamageMaxConstA` | 0 | divisor | Maximum physical damage, same pattern. |
| `<C>MagicDamageMinConstA` | 0 | divisor | Minimum wizardry/curse damage = Energy / A. |
| `<C>MagicDamageMaxConstA` | 0 | divisor | Maximum wizardry/curse damage = Energy / A. |
| `FEPhysiDamageMinBowConstA` / `...MinBowConstB` | 0 | divisor | Elf with bow, minimum = Strength / A + Agility / B. |
| `FEPhysiDamageMaxBowConstA` / `...MaxBowConstB` | 0 | divisor | Elf with bow, maximum. |
| `MGPhysiDamageMinConstB`, `MGPhysiDamageMaxConstB` | 0 | divisor | Magic Gladiator: adds Energy / B to physical damage. |
| `DLPhysiDamageMinConstB`, `DLPhysiDamageMaxConstB` | 0 | divisor | Dark Lord: adds Energy / B. |
| `RFPhysiDamageMinConstB`, `RFPhysiDamageMaxConstB` | 0 | divisor | Rage Fighter: adds Vitality / B. |

Read for DW to GC. The `KUN`, `LEM` and `KIN` keys, and `GLPhysiDamageMinConstB` / `GLPhysiDamageMaxConstB`, are in the file but not read.

## Character Attack Success Rate Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>AttackSuccessRateConstA` | 0 | multiplier | Attack rate (PvM) = (level + master level) × A + (Agility × B) / C + Strength / D. |
| `<C>AttackSuccessRateConstB` | 0 | multiplier | See above. |
| `<C>AttackSuccessRateConstC` | 0 | divisor | See above. |
| `<C>AttackSuccessRateConstD` | 0 | divisor | See above. |
| `DLAttackSuccessRateConstE` | 0 | divisor | Dark Lord only: + Command / E. |

## Character Attack Success Rate PvP Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>AttackSuccessRatePvPConstA` | 0 | multiplier | Attack rate (PvP) = ((level + master level) × A) / B + (Agility × C) / D. |
| `<C>AttackSuccessRatePvPConstB` | 0 | divisor | See above. |
| `<C>AttackSuccessRatePvPConstC` | 0 | multiplier | See above. |
| `<C>AttackSuccessRatePvPConstD` | 0 | divisor | See above. |

## Character Speed Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>PhysiSpeedConstA` | 0 (GL to GC: 1) | divisor | Attack speed = Agility / A. |
| `<C>MagicSpeedConstA` | 0 (GL to GC: 1) | divisor | Magic speed = Agility / A. |

{% hint style="warning" %}
Lowering speed divisors makes characters attack faster. If you also use `CheckSpeedHack` in Common.dat or the [offline helper](common.md#mu-helper-offline), test the new speeds first: very high speeds can be flagged as hacks or desync the client.
{% endhint %}

## Character Defense Success Rate Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>DefenseSuccessRateConstA` | 0 | divisor | Defense rate (PvM) = Agility / A. |

## Character Defense Success Rate PvP Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>DefenseSuccessRatePvPConstA` | 0 | multiplier | Defense rate (PvP) = ((level + master level) × A) / B + Agility / C. |
| `<C>DefenseSuccessRatePvPConstB` | 0 | divisor | See above. |
| `<C>DefenseSuccessRatePvPConstC` | 0 | divisor | See above. |

## Character Defense Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `<C>DefenseConstA` | 0 (RF to GC: 1) | divisor | Base defense = Agility / A. |

The `KUN`, `LEM` and `KIN` keys of the Attack Success Rate, Speed, Defense Success Rate and Defense blocks are in the file but not read.

---

## Example

Make Dark Wizards hit Dark Knights less in PvP and let Elves regenerate mana faster:

```ini
DWDamageRateToDK = 120
FEMPRecoveryRate = 6
```

## Rules and pitfalls

- **Test balance changes on a test server first.** Small divisor changes have large effects at high stats.
- **Divisors can never be 0.** A missing `Const` key is 0.
- **PvP rates are ignored when Balance Class is on**, except the event/duel rates when `CustomBalanceClassKeepEventRate = 1`.
- **GameServerCS has its own copy.** Castle Siege balance is set in `GameServerCS/Data/GameServerInfo - Character.dat`; the shipped CS copy has every class-vs-class rate at 100.
- **Newer-class keys** (GL to KIN) only matter if your client supports those classes; several of them are not read (listed above).

## Reload

`/reload character`. Stats are recalculated for each character the next time their attributes are refreshed (equip/unequip, level up, relog).
