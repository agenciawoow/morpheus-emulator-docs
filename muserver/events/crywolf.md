# Crywolf

Scheduled fortress defense on the Castle Siege server: players protect the Wolf statue and the altars against Balgass's army, and the result gives the whole server a bonus or a penalty.

## What it is

* At the scheduled time, notices announce the attack. Elves can sign a contract at the five altars to protect the Wolf statue.
* Monsters and finally **Balgass** attack the fortress for a fixed time. Players earn MVP points for every kill.
* If the players hold, Crywolf is **protected**: the whole server gets the benefit (weaker monsters, a Chaos Machine bonus shown to players). If they fail, Crywolf is **occupied**: the penalty applies (less jewel drop, less EXP) until the next win.
* At the end, the top 5 players get a rank, EXP by rank and an item drop.

## Enable it

Crywolf runs **only on the GameServerCS** (Castle Siege server). Set its keys in `GameServerCS/DATA/GameServerInfo - Event.dat`.

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` (CS) | `CrywolfEvent` | `0` | `1` = the event runs on the schedule of `Crywolf.dat` |
| `GameServerInfo - Event.dat` | `CrywolfBenefitSwitch` | `1` | `1` = apply the benefit while Crywolf is protected |
| `GameServerInfo - Event.dat` | `CrywolfBenefitChaosRate` | `10` | Chaos Machine bonus (%) sent to the client for display while protected |
| `GameServerInfo - Event.dat` | `CrywolfBenefitMonsterLife` | `80` | While protected, monsters spawn with this % of their HP (`80` = 20% weaker). Not applied on the Crywolf map during the battle |
| `GameServerInfo - Event.dat` | `CrywolfPenaltySwitch` | `0` | `1` = apply the penalty while Crywolf is occupied |
| `GameServerInfo - Event.dat` | `CrywolfPenaltyJewelDrop` | `70` | While occupied, monster drops of Jewel of Bless, Soul, Life, Creation, Guardian (and item 12,25) survive only this % of the time |
| `GameServerInfo - Event.dat` | `CrywolfPenaltyExperience` | `70` | While occupied, monster EXP is multiplied by this % |
| `GameServerInfo - Common.dat` | `AddEventExperienceRate_AL0..AL3` | `0` | Multiplier applied to the rank EXP of section 7 |

The benefit and penalty keys are read by **every** GameServer, and the protected/occupied result is shared across the server group. Set them the same on the GameServer and the GameServerCS.

## Files & syntax

`Data/Event/Crywolf.dat`, 8 sections. Only the GameServerCS reads this file.

### Section 0 - Start schedule

This section has its own format (not the shared schedule format):

| Column | Meaning | Values |
|---|---|---|
| Mode | `0` = run once at each start time. `1` = loop: after the end, go straight back to the first notice | `0` / `1` (the last row's value applies to all rows) |
| State | State to start in. Use `1` (first notice) | `1`-`6` |
| Month | Month | `*` or `1`-`12` |
| Day | Day of month | `*` or `1`-`31` |
| DayOfWeek | Day of week | `*`, `1` = Sunday ... `7` = Saturday |
| Hour | Hour | `*` or `0`-`23` |
| Minute | Minute | `*` or `0`-`59` |
| ContinuanceTime | Tolerance in seconds after the start time during which the start is still accepted (useful after a restart) | seconds, `*` = must match the exact minute |

There is no Year and no Second column.

### Section 1 - State durations

| Column | Meaning | Values |
|---|---|---|
| State | `1` first notice, `2` second notice, `3` ready, `4` battle, `5` end, `6` end cycle | `1`-`6` |
| Hour | Duration, hours part | number |
| Minute | Duration, minutes part | number |
| Second | Duration, seconds part | number |

Shipped: notice 1 = 300 s, notice 2 = 60 s, ready = 120 s, **battle = 900 s (15 min)**, end = 15 s, end cycle = 10 s.

### Section 2 - Monster AI groups

One value per row: the `MonsterAIGroup.txt` groups spawned for the attack (shipped 1-4 and 6-13).

### Section 3 - Boss

| Column | Meaning | Values |
|---|---|---|
| Group | `MonsterAIGroup.txt` group of Balgass | shipped `5` |
| TurnUpTime | Seconds after the battle starts when Balgass appears | shipped `600` |

### Section 4 - AI change time

One value: seconds after the battle starts when the monster groups switch to their attack AI (shipped `300`).

### Section 5 - MVP points per action

| Column | Meaning | Shipped |
|---|---|---|
| MVPScoreTable1 | Killing Balgass (class 349) | `7000` |
| MVPScoreTable2 | Killing a Dark Elf (340) | `3000` |
| MVPScoreTable3 | Killing a Balram (344) | `600` |
| MVPScoreTable4 | Killing a Soram (341) | `700` |
| MVPScoreTable5 | Killing a Death Spirit (345) | `600` |
| MVPScoreTable6 | Killing a Tanker (348) | `1000` |
| MVPScoreTable7 | Bonus for an Elf still holding an altar contract at the end | `6000` |

### Section 6 - Rank thresholds

Five values: the MVP points needed for rank 1 to rank 5 (shipped `0 1001 3001 5001 10001`). A player gets the highest rank whose threshold he reaches.

### Section 7 - EXP per rank

Five values: the EXP given for rank 1 to rank 5 (shipped `0 200000 800000 1200000 1800000`). When Crywolf is occupied, players get only 10% of it.

## Example

```
0
//Mode   State   Month   Day   DayOfWeek   Hour   Minute   ContinuanceTime
0        1       *       *     4           20     30       *     // Wednesdays 20:30
0        1       *       *     7           20     30       *     // Saturdays 20:30
end

1
//State  Hour  Minute  Second
1        0     5       0          // first notice 5 min
2        0     1       0
3        0     2       0
4        0     20      0          // battle 20 min
5        0     0       15
6        0     0       10
end
```

## Schedule

Crywolf does **not** use the shared format of [Schedule syntax](schedule-syntax.md); see section 0 above. The shipped rows (Wednesday and Saturday 20:30) are **commented out**, so Crywolf never starts until you uncomment them and set `CrywolfEvent = 1` on the GameServerCS.

## Rewards

| Who | What | Configured in |
|---|---|---|
| Top 5 MVP players (hero list) | EXP of their rank + one item drop | Sections 6, 7 + item bag |
| Elves holding an altar contract at the end | Bonus MVP points + one item drop | Section 5 (column 7) + item bag |
| Whole server | Benefit or penalty until the next Crywolf | Benefit/Penalty keys |

Item bag: index 107 in `Data/EventItemBagManager.txt` (SpecialValue 57), file `Data/EventItemBag/107 - Crywolf.txt`. No item is given when the fortress falls.

## Commands related

There is no command to start Crywolf.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | On the GameServerCS, re-reads `Crywolf.dat` and the keys |

## Pitfalls

* Setting `CrywolfEvent = 1` on the normal GameServer does nothing: the event code and `Crywolf.dat` are only used by the GameServerCS.
* With every row of section 0 commented out (as shipped) Crywolf never starts.
* `CrywolfBenefitChaosRate` is only displayed to players. This build does not add it to Chaos Machine success rates.
* The penalty ships disabled (`CrywolfPenaltySwitch = 0`): losing Crywolf has no cost unless you turn it on.
* Rank EXP is multiplied by `AddEventExperienceRate_ALx` in `GameServerInfo - Common.dat`, shipped as `0`.
* Section 1 durations are hours/minutes/seconds; section 3 and 4 values are seconds.

## Reload module

`/reload event` on the GameServerCS re-reads `Crywolf.dat` and the Crywolf keys. Benefit/penalty keys on the main GameServer reload with `/reload event` there.

{% hint style="warning" %}
Re-reading `Crywolf.dat` clears its schedule and timers. Reload when no Crywolf battle is running, or restart the GameServerCS.
{% endhint %}
