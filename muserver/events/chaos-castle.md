# Chaos Castle

Scheduled free-for-all: players fight monsters and each other in a castle whose floor shrinks, and the best survivor wins.

## What it is

* Players enter by wearing the **Armor of Guardsman** (item 13,29) during the entry window. They must not be in a party, must not ride a Uniria, Dinorant or Fenrir, and pay a Zen fee that depends on the castle:

| Castle (Level) | Normal classes | MG / DL / RF | Entry fee (Zen) |
|---|---|---|---|
| 0 (CC1) | 10 - 49 | 10 - 29 | 25,000 |
| 1 (CC2) | 50 - 119 | 30 - 99 | 80,000 |
| 2 (CC3) | 120 - 179 | 100 - 159 | 150,000 |
| 3 (CC4) | 180 - 239 | 160 - 219 | 250,000 |
| 4 (CC5) | 240 - 299 | 220 - 279 | 400,000 |
| 5 (CC6) | 300 - max level | 280 - max level | 650,000 |
| 6 (CC7) | any level after the 3rd class change | same | 1,000,000 |

Brackets are for a 400 max level and scale with `MaxLevelCharacter` above 400. The entry fees are fixed in the server.

* Inside, killing a monster gives 2 points and killing a player 1 point. Killed monsters can blow nearby players back. At the end, the player with the highest score among those still inside wins.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `ChaosCastleEvent` | `1` | `1` = event runs on its schedule |
| `GameServerInfo - Event.dat` | `ChaosCastleMinUser` | `1` | Minimum players inside when the entrance closes. With fewer, that castle is cancelled |
| `GameServerInfo - Event.dat` | `ChaosCastleBlowUserRate` | `50` | Chance in % that a dying monster knocks back the players near it |
| `GameServerInfo - Common.dat` | `PkCanEnterCC` | `0` | `1` lets PK characters (PK level 4+) enter. Ignored when `PKLimitFree = 1` |
| `GameServerInfo - Common.dat` | `AddEventExperienceRate_AL0..AL3` | `0` | Multiplier applied to the EXP of section 2 (see Pitfalls) |

There is no max-user key: each castle holds up to 70 players. The GameServerCS ships with `ChaosCastleEvent = 0`.

## Files & syntax

`Data/Event/ChaosCastle.dat`, 4 sections. In sections 2 and 3 the first column is the castle **Level** `0`-`6` (CC1 to CC7).

### Section 0 - Timings (one row, minutes)

| Column | Meaning | Values |
|---|---|---|
| WarningTime | Minutes before the scheduled time when the entry opens and the countdown starts | minutes, shipped `5` |
| NotifyTime | After the scheduled time: entry closed, players wait inside. `ChaosCastleMinUser` is checked here | minutes, shipped `1` |
| EventTime | Duration of the fight | minutes, shipped `10` |
| CloseTime | After the fight: rewards, then players are sent out | minutes, shipped `4` |

### Section 1 - Schedule

`Year Month Day DoW Hour Minute Second`, see [Schedule syntax](schedule-syntax.md). The time is when the entry closes.

### Section 2 - Experience

EXP = (players killed x ExperienceTable1) + (monsters killed x ExperienceTable2). Every participant gets it, winner or not.

| Column | Meaning | Values |
|---|---|---|
| Level | Castle | `0`-`6` |
| ExperienceTable1 | EXP per player killed | number |
| ExperienceTable2 | EXP per monster killed | number |

### Section 3 - Coin reward (winner only)

| Column | Meaning | Values |
|---|---|---|
| Level | Castle | `0`-`6` |
| WC | WCoinC | number, `0` = none |
| WP | WCoinP | number |
| GP | GoblinPoint | number |

## Example

```
0
//WarningTime   NotifyTime   EventTime   CloseTime
5               1            10          4
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     19     30       0      // every day, entry closes 19:30
*        *       *     7     15     30       0      // extra run on Saturdays 15:30
end

2
//Level   ExperienceTable1   ExperienceTable2
0         1000               500                    // CC1: 1000 per player kill, 500 per monster kill
end

3
//Level   WC   WP   GP
6         20   0    5                               // CC7 winner: 20 WCoinC + 5 GoblinPoint
end
```

## Schedule

Shared format, see [Schedule syntax](schedule-syntax.md). Shipped: every 2 hours at minute 30 on odd hours (01:30, 03:30 ... 23:30).

```
1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     1      30       0  // Chaos Castle
*        *       *     *     3      30       0  // Chaos Castle
end
```

## Rewards

| Who | What | Configured in |
|---|---|---|
| Every participant | EXP from kills | Section 2 |
| Winner | Coins + one item bag drop at his feet | Section 3 + item bag |

Item bag of the winner, by castle:

| Castle | Bag index (`Data/EventItemBagManager.txt`) | File |
|---|---|---|
| CC1 | 20 | `Data/EventItemBag/020 - Chaos Castle 1.txt` |
| CC2 ... CC6 | 21 ... 25 | `021 - Chaos Castle 2.txt` ... `025 - Chaos Castle 6.txt` |
| CC7 | 26 | `Data/EventItemBag/026 - Chaos Castle 7.txt` |

The winner also gets a Chaos Castle completion for the Battle Pass.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startcc` | Admin (GameMaster 2) | One-off start 2 minutes from now. Only works with `ChaosCastleEvent = 1` |
| `/reload event` | Admin | Re-reads this file and resets the schedule |

## Pitfalls

* **No EXP is paid with the shipped Common.dat**: kill EXP is multiplied by `AddEventExperienceRate_ALx`, shipped as `0`. Set it to `1` or more.
* Shipped section 2 has `100` as ExperienceTable2 for Level 1 (CC2), between `500` and `1500`. It is probably a typo of `1000`.
* `ChaosCastleMinUser = 1` lets a single player win alone. Raise it for a real fight.
* `ChaosCastleBlowUserRate = 100` throws players off the shrinking floor very often.
* `/startcc` near midnight (23:58-23:59) may never fire.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and this file and recalculates the next start. `PkCanEnterCC` and the EXP rate keys need `/reload common`. Bag files need `/reload eventitembag`.

{% hint style="warning" %}
`/reload event` resets every castle to idle. A running Chaos Castle ends with no rewards. Reload between events.
{% endhint %}
