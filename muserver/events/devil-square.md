# Devil Square

Scheduled wave arena: players kill monster waves for points and the top 10 of each square are rewarded by rank.

## What it is

* A few minutes before each start a global countdown opens the entrance at **Charon**. Players enter with a **Devil's Invitation** of the right level (item 14,19, level = square number) or a **Devil Square Ticket** (item 13,46).
* There are **7 squares**, picked from the character's level:

| Square (Level) | Normal classes | MG / DL / RF |
|---|---|---|
| 0 (DS1) | 15 - 130 | 10 - 110 |
| 1 (DS2) | 131 - 180 | 111 - 160 |
| 2 (DS3) | 181 - 230 | 161 - 210 |
| 3 (DS4) | 231 - 280 | 211 - 260 |
| 4 (DS5) | 281 - 330 | 261 - 310 |
| 5 (DS6) | 331 - max level | 311 - max level |
| 6 (DS7) | any level after the 3rd class change | same |

Brackets are for a 400 max level and scale with `MaxLevelCharacter` above 400.

* New monster waves join at 75%, 50% and 25% of the remaining time. Each kill gives points. At the end players are ranked by score and the top 10 get rewards.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `DevilSquareEvent` | `1` | `1` = event runs on its schedule |
| `GameServerInfo - Event.dat` | `DevilSquareMaxUser` | `15` | Maximum players per square (hard limit 50) |
| `GameServerInfo - Common.dat` | `PkCanEnterDS` | `0` | `1` lets PK characters (PK level 4+) enter. Ignored when `PKLimitFree = 1` |
| `GameServerInfo - Common.dat` | `AddEventExperienceRate_AL0..AL3` | `0` | Multiplier applied to the EXP of section 2 (see Pitfalls) |

The GameServerCS ships with `DevilSquareEvent = 0`.

## Files & syntax

`Data/Event/DevilSquare.dat`, 5 sections. In sections 2 to 4 the first column is the square **Level** `0`-`6` (DS1 to DS7).

### Section 0 - Timings (one row, minutes)

| Column | Meaning | Values |
|---|---|---|
| WarningTime | Minutes before the scheduled time when the entrance opens and the countdown starts | minutes, shipped `5` |
| NotifyTime | After the scheduled time: entrance closed, players wait inside | minutes, shipped `1` |
| EventTime | Duration of the fight. The 3 extra waves come at 75/50/25% of it | minutes, shipped `20` |
| CloseTime | After the fight: rewards and score window, then players are sent out | minutes, shipped `4` |

### Section 1 - Schedule

`Year Month Day DoW Hour Minute Second`, see [Schedule syntax](schedule-syntax.md). The time is when the entrance closes.

### Section 2 - Experience by rank

| Column | Meaning | Values |
|---|---|---|
| Level | Square | `0`-`6` |
| RewardExperience1 ... RewardExperience10 | EXP for rank 1 ... rank 10 | number |

### Section 3 - Zen by rank

| Column | Meaning | Values |
|---|---|---|
| Level | Square | `0`-`6` |
| RewardMoney1 ... RewardMoney10 | Zen for rank 1 ... rank 10 | number |

### Section 4 - Coin reward (rank 1 only)

| Column | Meaning | Values |
|---|---|---|
| Level | Square | `0`-`6` |
| WC | WCoinC | number, `0` = none |
| WP | WCoinP | number |
| GP | GoblinPoint | number |

## Example

```
0
//WarningTime   NotifyTime   EventTime   CloseTime
5               1            20          4
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     21     30       0      // every day, entrance closes 21:30
end

2
//Level   RewardExperience1 ... RewardExperience10
0         6000   4000   4000   2000   2000   2000   1000   1000   1000   1000
end

3
//Level   RewardMoney1 ... RewardMoney10
0         30000  25000  25000  20000  20000  20000  15000  15000  15000  15000
end

4
//Level   WC   WP   GP
6         30   0    0                              // DS7 winner gets 30 WCoinC
end
```

## Schedule

Shared format, see [Schedule syntax](schedule-syntax.md). Shipped: every 2 hours at minute 30 on even hours (00:30, 02:30 ... 22:30), so it never overlaps Blood Castle.

```
1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     0      30       0  // Devil Square
*        *       *     *     2      30       0  // Devil Square
end
```

## Rewards

| Who | What | Configured in |
|---|---|---|
| Ranks 1 to 10 | EXP and Zen of their rank column | Sections 2 and 3 |
| Rank 1 only | Coins + one item bag drop at his feet | Section 4 + item bag |
| Rank 11 and below | Nothing | |

Item bag of rank 1, by square:

| Square | Bag index (`Data/EventItemBagManager.txt`) | File |
|---|---|---|
| DS1 | 147 | `Data/EventItemBag/147 - Devil Square 1.txt` |
| DS2 | 148 | `148 - Devil Square 2.txt` |
| DS3 | 149 | `149 - Devil Square 3.txt` |
| DS4 | 150 | `150 - Devil Square 4.txt` |
| DS5 ... DS7 | 151 ... 153 | `151 - Devil Square 5.txt` ... `153 - Devil Square 7.txt` |

Players in the top 10 also get a Devil Square completion for the Battle Pass.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startds` | Admin (GameMaster 2) | One-off start 2 minutes from now. Only works with `DevilSquareEvent = 1` |
| `/reload event` | Admin | Re-reads this file and resets the schedule |

## Pitfalls

* **No EXP is paid with the shipped Common.dat**: rank EXP is multiplied by `AddEventExperienceRate_ALx`, shipped as `0`. Set it to `1` or more.
* The shipped `Data/EventItemBag` folder has two files numbered `149` (`149 - Devil Square 3.txt`, `149 - Devil Square 4.txt`) and two numbered `150` (`150 - Devil Square 3.txt`, `150 - Devil Square 4.txt`). The server loads only the first file it finds for each number (alphabetical), so bag 150 (DS4) loads `150 - Devil Square 3.txt`. Keep exactly one file per number.
* Section 4 rows exist for all 7 squares, but only rank 1 gets coins and the item.
* The Level column is 0-based: `0` = DS1, `6` = DS7.
* `/startds` near midnight (23:58-23:59) may never fire.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and this file and recalculates the next start. `PkCanEnterDS` and the EXP rate keys need `/reload common`. Bag files need `/reload eventitembag`.

{% hint style="warning" %}
`/reload event` resets every square to idle. A running Devil Square ends with no rewards. Reload between events.
{% endhint %}
