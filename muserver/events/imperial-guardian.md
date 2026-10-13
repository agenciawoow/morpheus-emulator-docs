# Imperial Guardian

On-demand party dungeon (Imperial Fort): one party at a time clears timed zones and kills the fort's bosses, with monsters scaled to the party's level.

## What it is

* A party enters through the Imperial Guardian NPC with an entry item: **Gaion's Order** (item 14,102) or the **Access Ticket to Varka** (item 13,126) from Monday to Saturday, and the **Complete Secromicon** (item 14,109) or the **Access Ticket to Varka Map 7** (item 13,127) on Sunday. One item is consumed per entry.
* The day of the week picks the fort: Monday/Thursday, Tuesday/Friday and Wednesday/Saturday each have their own fort with 3 zones, and Sunday is the full 4-zone run.
* Each zone has a wait phase and a play phase with a time limit. Kill the monsters, break the gates, and kill the side boss and the main boss.
* Only one party can be inside at a time. Up to 5 players.
* Monster strength is scaled from the **highest level in the party** (normal level + master level, capped at 600).

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `ImperialGuardianEvent` | `1` | `1` = the NPC lets parties in. `0` = closed |
| `GameServerInfo - Event.dat` | `ImperialGuardianDifficultRate` | `50` | Global difficulty in %, applied on top of section 2: `100` = table values, `50` = half HP/damage/defense |

The GameServerCS ships with `ImperialGuardianEvent = 0` and `ImperialGuardianDifficultRate = 100`.

## Files & syntax

`Data/Event/ImperialGuardian.dat`, 3 sections.

### Section 0 - Timings (one row, minutes)

| Column | Meaning | Values |
|---|---|---|
| NotifyTime | Wait phase before each zone starts | minutes, suggested `1` |
| EventTime | Time limit of each zone's play phase | minutes, suggested `15` |
| CloseTime | Closing phase after the run (success or failure) before players are sent out | minutes, suggested `1` |

{% hint style="danger" %}
In the shipped file this row is **commented out**, so the three times are `0` and the zone timers run out at once. Uncomment it (`1 15 1`) before you enable the event.
{% endhint %}

### Section 1 - Monster AI

| Column | Meaning | Values |
|---|---|---|
| DefaultMonsterAIUnitNumber | `MonsterAIUnit.txt` unit used by the fort monsters | shipped `72` |
| SupportMonsterAIUnitNumber | AI unit used by the support monster (Medic, class 519) | shipped `73` |

### Section 2 - Difficulty by party level

The row used is `Index = (highest party level - 1) / 10`, where "level" is normal level + master level, capped at 600. So Index `0` covers levels 1-10, Index `40` covers 401-410, Index `59` covers 591-600.

| Column | Meaning | Values |
|---|---|---|
| Index | Level bracket | `0`-`59` |
| Level | Level given to the fort monsters | number |
| MaxLife | Multiplier on the monsters' base HP | decimal, e.g. `10.00` = x10 |
| DamageMin | Multiplier on min damage | decimal |
| DamageMax | Multiplier on max damage | decimal |
| Defense | Multiplier on defense | decimal |
| AttackRate | Multiplier on attack success rate | decimal |
| DefenseRate | Multiplier on defense success rate | decimal |

Final stat = base stat x multiplier x `ImperialGuardianDifficultRate` / 100 (the Level column is used as is).

## Example

```
0
//NotifyTime   EventTime   CloseTime
1              15          1                  // 1 min wait, 15 min per zone, 1 min closing
end

1
//DefaultMonsterAIUnitNumber   SupportMonsterAIUnitNumber
72                             73
end

2
//Index   Level   MaxLife   DamageMin   DamageMax   Defense   AttackRate   DefenseRate
0         45      10.00     3.00        3.00        3.00      5.00         5.00     // party top level 1-10
39        105     1800.00   42.00       42.00       25.00     20.00        20.00    // party top level 391-400
59        145     2000.00   53.00       53.00       45.00     30.00        30.00    // party top level 591-600
end
```

## Schedule

Imperial Guardian has no schedule: a party can enter whenever the fort is empty. It is not listed in [Schedule syntax](schedule-syntax.md).

## Rewards

| When | What | Configured in |
|---|---|---|
| Run completed | EXP to each member: 10,000 x Index (15,000 x Index on Sunday), multiplied by `AddEventExperienceRate_ALx` | Fixed formula, `GameServerInfo - Common.dat` |
| Side boss killed (Mon-Sat) | One Secromicon fragment per party member (item 14,103 to 14,108, by weekday) + one bag drop | Item bag |
| Main boss killed | One bag drop | Item bag |

The bag depends on the highest party level (normal + master):

| Highest level | Side boss bag | Main boss bag |
|---|---|---|
| 1 - 100 | `088 - Imperial Guardian Side Boss 1.txt` | `093 - Imperial Guardian Main Boss 1.txt` |
| 101 - 200 | `089 - ... Side Boss 2.txt` | `094 - ... Main Boss 2.txt` |
| 201 - 300 | `090 - ... Side Boss 3.txt` | `095 - ... Main Boss 3.txt` |
| 301 - 400 | `091 - ... Side Boss 4.txt` | `096 - ... Main Boss 4.txt` |
| 401 and above | `092 - ... Side Boss 5.txt` | `097 - ... Main Boss 5.txt` |

All files are in `Data/EventItemBag/` (indices 88 to 97 in `Data/EventItemBagManager.txt`).

## Commands related

There is no command to start Imperial Guardian.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | Re-reads `ImperialGuardian.dat` and the keys |
| `/reload eventitembag` | Admin | Re-reads the boss bags |

## Pitfalls

* The shipped section 0 is commented out while the event is enabled: uncomment it, or parties fail immediately.
* EXP is multiplied by `AddEventExperienceRate_ALx` in `GameServerInfo - Common.dat`, shipped as `0`, so no EXP is paid by default.
* `ImperialGuardianDifficultRate` scales everything at once. Use section 2 to tune single level brackets.
* Keep all 60 rows of section 2. A missing Index leaves that level bracket with zero stats.
* PK characters (PK level 4+) cannot enter unless `PKLimitFree = 1`.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `ImperialGuardian.dat`. A run in progress keeps going; new values apply to the next monsters spawned and the next run. Bags reload with `/reload eventitembag`.

{% hint style="warning" %}
Reload when no party is inside the fort, so one run does not mix old and new values.
{% endhint %}
