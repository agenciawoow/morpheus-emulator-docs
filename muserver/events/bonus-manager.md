# Bonus Manager

Scheduled bonus windows: during a set time, players get extra EXP, extra drop, or extra resets, optionally limited to maps, monsters or VIP levels.

## What it is

* At the scheduled time a global message can announce the bonus. For the length of the window, the bonus is applied automatically: no NPC, no command.
* At the end, an optional message announces that the bonus is over.
* The next bonus window appears in the event time window (H key) as "Custom Bonus".

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `BonusManagerSwitch` | `1` | `1` = bonus windows follow their schedule |

The GameServerCS ships with `BonusManagerSwitch = 0`.

## Files & syntax

`Data/Event/BonusManager.dat`, 3 sections. Every row starts with **Index**, the bonus window number (`0`-`29`), which ties the sections together.

### Section 0 - Schedule

| Column | Meaning | Values |
|---|---|---|
| Index | Bonus window | `0`-`29` |
| Year Month Day DoW Hour Minute Second | Start time, see [Schedule syntax](schedule-syntax.md) | |

### Section 1 - Window settings

| Column | Meaning | Values |
|---|---|---|
| Index | Bonus window | `0`-`29` |
| StartMessage | Message number from `Data/Lang/*_Message.txt` shown to everybody at the start | number, `*` = none |
| FinalMessage | Message shown at the end | number, `*` = none |
| BonusTime | Length of the window | **seconds**, shipped `7200` (2 hours) |

### Section 2 - Bonus values

One row per bonus. A window can have several rows (for example EXP and drop together).

| Column | Meaning | Values |
|---|---|---|
| Index | Bonus window | `0`-`29` |
| BonusIndex | Type of bonus (table below) | `0`-`8` |
| BonusValue_AL0 | Value for free accounts | number |
| BonusValue_AL1 | Value for VIP 1 | number |
| BonusValue_AL2 | Value for VIP 2 | number |
| BonusValue_AL3 | Value for VIP 3 | number |
| ItemIndex | Only for this item (`section x 512 + index`). Used by BonusIndex 3 only | item code, `*` = any |
| ItemLevel | Only for this item level. Used by BonusIndex 3 only | `0`-`15`, `*` = any |
| MapNumber | Only while the **player** is on this map | map, `-1` = any |
| MonsterClass | Only for this killed monster class | class, `-1` = any |
| MonsterLevelMin | Only for killed monsters of at least this level | level, `-1` = no minimum |
| MonsterLevelMax | Only for killed monsters up to this level | level, `-1` = no maximum |

### BonusIndex values

| BonusIndex | Bonus | How the value works |
|---|---|---|
| `0` | EXP | Percent added to the EXP of each kill. `100` = double EXP, `50` = +50% |
| `1` | Master EXP | Same as 0, for master level EXP |
| `2` | Item drop rate | Percent added to the monsters' chance to drop an item. `100` = twice the drops. Always uses `BonusValue_AL0` (see Pitfalls) |
| `3` | Item drop rate per item (`Data/Item/ItemDrop.txt` entries) | Adds `value / 1,000,000` of the remaining chance. `500000` closes half the gap to 100% |
| `4` | Excellent item drop rate | Same formula as 3, on the excellent drop chance |
| `5` | Ancient (set) item drop rate | Same formula as 3, on the ancient drop chance |
| `6` | GoblinPoint | No effect in this build |
| `7` | Resets | Extra resets added on each `/reset` |
| `8` | Master resets | Extra master resets added on each `/mreset` |

Several active rows that match the same kill or reset all add up.

## Example

Double EXP on weekends 20:00-22:00, and +50% drop in Lost Tower (map 4) every day 18:00-19:00:

```
0
//Index     Year   Month   Day   DoW   Hour   Minute   Second
0           *      *       *     1     20     0        0       // Sunday 20:00
0           *      *       *     7     20     0        0       // Saturday 20:00
1           *      *       *     *     18     0        0       // every day 18:00
end

1
//Index   StartMessage   FinalMessage   BonusTime
0         *              *              7200                     // 2 hours
1         *              *              3600                     // 1 hour
end

2
//Index   BonusIndex   BonusValue_AL0   BonusValue_AL1   BonusValue_AL2   BonusValue_AL3   ItemIndex   ItemLevel   MapNumber   MonsterClass   MonsterLevelMin   MonsterLevelMax
0         0            100              100              120              150              *           *           -1          -1             -1                -1    // +100% EXP, more for VIP 2/3
0         1            100              100              100              100              *           *           -1          -1             -1                -1    // +100% master EXP
1         2            50               50               50               50               *           *           4           -1             -1                -1    // +50% drop in Lost Tower
end
```

## Schedule

Section 0 uses the shared format with a leading Index, see [Schedule syntax](schedule-syntax.md). Shipped: window 0 every day at 20:00, for 7200 seconds.

```
0
//Index     Year   Month   Day   DoW   Hour   Minute   Second
0           *      *       *     *     20     0        0
end
```

## Rewards

The bonus is the reward: section 2 values apply to every player that matches the filters while the window is open. No items, no coins.

## Commands related

There is no command to start a bonus window by hand.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | Re-reads this file and recalculates the next window |

## Pitfalls

* **The shipped row is +5000% EXP** (`5000 6000 7000 8000`): 51 to 81 times the normal EXP every day from 20:00 to 22:00. Lower it if that is not what you want.
* BonusTime is in seconds.
* ItemIndex and ItemLevel are only checked by BonusIndex 3. For any other BonusIndex they must be `*`, or the row never applies.
* For BonusIndex 7 and 8 (resets) leave MonsterClass and the monster levels at `-1`: there is no monster in a reset, so a filter there makes the row never apply.
* MapNumber is the map the player is on.
* BonusIndex 2 is checked against the monster, not the killer: the VIP columns are ignored (only `BonusValue_AL0` counts). Use BonusIndex 3 to 5 for VIP-only drop bonuses.
* BonusIndex 3 to 5 use parts per million, not percent: `100` adds almost nothing.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `BonusManager.dat` and recalculates the next window.

{% hint style="warning" %}
A bonus window that is open when you reload is closed at once (players lose the rest of it). Reload outside the bonus windows.
{% endhint %}
