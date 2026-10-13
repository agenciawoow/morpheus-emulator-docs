# Character base stats (DefaultClassInfo)

## What it does

`DefaultClassInfo.txt` sets, for each class:

- the **starting stats** of a new character, and
- the **HP and Mana formula** used for every character of that class.

Level-up points, reset points and other character rules are in `GameServerInfo - Character.dat` and `GameServerInfo - Common.dat`.

## Where it lives

`MuServer/Data/Character/DefaultClassInfo.txt`

## Syntax

One line per class, terminated by `end`.

| Column | Values | Meaning |
|---|---|---|
| `Class` | 0–14 | 0 Dark Wizard, 1 Dark Knight, 2 Fairy Elf, 3 Magic Gladiator, 4 Dark Lord, 5 Summoner, 6 Rage Fighter, 7 Grow Lancer, 8 Rune Wizard, 9 Slayer, 10 Gun Crusher, 11 Light Wizard, 12 Lemuria Mage, 13 Illusion Knight, 14 Alchemist. |
| `Strength` | number | Starting Strength. |
| `Dexterity` | number | Starting Agility. |
| `Vitality` | number | Starting Vitality. |
| `Energy` | number | Starting Energy. |
| `Leadership` | number | Starting Command (Dark Lord). |
| `MaxLife` | decimal | Base HP at level 1. |
| `MaxMana` | decimal | Base Mana at level 1. |
| `LevelLife` | decimal | HP gained per level (and per master level). |
| `LevelMana` | decimal | Mana gained per level (and per master level). |
| `VitalityToLife` | decimal | HP per point of Vitality above the starting Vitality. |
| `EnergyToMana` | decimal | Mana per point of Energy above the starting Energy. |

### How HP and Mana are calculated

```
MaxHP   = MaxLife + LevelLife × (Level − 1) + LevelLife × MasterLevel + (Vitality − starting Vitality) × VitalityToLife
MaxMana = MaxMana + LevelMana × (Level − 1) + LevelMana × MasterLevel + (Energy − starting Energy) × EnergyToMana
```

Items, buffs and options are added on top.

## Example (real lines)

```
//Class   Strength   Dexterity   Vitality   Energy   Leadership   MaxLife   MaxMana   LevelLife   LevelMana   VitalityToLife   EnergyToMana
0         18         18          15         30       0            60        60        1.0         2.0         2.0              2.0
1         28         20          25         10       0            110       20        2.0         0.5         3.0              1.0
```

A level 100 Dark Knight with 125 Vitality has `110 + 2.0 × 99 + (125 − 25) × 3.0 = 608` base HP.

## Rules & pitfalls

- Starting stats only affect **new** characters. The HP/Mana formula applies to everyone the next time their stats are recalculated (login, level-up, stat change).
- Lowering a class's starting `Vitality`/`Energy` raises the HP/Mana of existing characters (more points count as "above the start").
- The client calculates the HP/Mana it shows with its own values. If you change the formula, the HP bar may show a different maximum until the server sends the real value.

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload character
```

Reloads `GameServerInfo - Character.dat` and `DefaultClassInfo.txt`. `/reload all` includes it.
