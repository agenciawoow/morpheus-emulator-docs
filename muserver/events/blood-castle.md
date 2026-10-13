# Blood Castle

Scheduled party event: players break the castle gate and the Saint Statue, recover the Archangel's weapon and return it to the Archangel.

## What it is

* Several minutes before each start, a global notice counts down and the **Archangel Messenger** opens the entrance. Players enter with an **Cloak of Invisibility** of the right level (item 13,18, level = castle number) or a **Blood Castle Ticket** (item 13,47).
* There are **8 castles**. The server picks the castle from the character's level and class:

| Castle (Level) | Normal classes | MG / DL / RF |
|---|---|---|
| 0 (BC1) | 15 - 80 | 10 - 60 |
| 1 (BC2) | 81 - 130 | 61 - 110 |
| 2 (BC3) | 131 - 180 | 111 - 160 |
| 3 (BC4) | 181 - 230 | 161 - 210 |
| 4 (BC5) | 231 - 280 | 211 - 260 |
| 5 (BC6) | 281 - 330 | 261 - 310 |
| 6 (BC7) | 331 - max level | 311 - max level |
| 7 (BC8) | any level after the 3rd class change | same |

The level brackets are written for a 400 max level. If `MaxLevelCharacter` in `GameServerInfo - Common.dat` is above 400, every bracket is scaled proportionally.

* Inside, players kill monsters until the gate opens, destroy the gate, kill more monsters, destroy the Saint Statue, pick up the weapon and hand it to the Archangel. The player who delivers it wins, together with his party.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `BloodCastleEvent` | `1` | `1` = event runs on its schedule. `0` = all castles stay closed |
| `GameServerInfo - Event.dat` | `BloodCastleMaxUser` | `15` | Maximum players per castle (hard limit 50) |
| `GameServerInfo - Common.dat` | `PkCanEnterBC` | `0` | `1` lets PK characters (PK level 4+) enter. Ignored when `PKLimitFree = 1` |
| `GameServerInfo - Common.dat` | `AddEventExperienceRate_AL0..AL3` | `0` | Multiplier applied to the EXP of section 2 (see Pitfalls) |
| `GameServerInfo - Common.dat` | `MaxLevelUpEvent` | `0` | Whether event EXP may give more than one level at a time |

The GameServerCS ships with `BloodCastleEvent = 0` and `BloodCastleMaxUser = 10`: Blood Castle normally runs only on the main GameServer.

## Files & syntax

`Data/Event/BloodCastle.dat`, 6 sections. In sections 2 to 5 the first column is the castle **Level** `0`-`7` (BC1 to BC8).

### Section 0 - Timings (one row, minutes)

| Column | Meaning | Values |
|---|---|---|
| WarningTime | Minutes before the scheduled time when the entrance opens and the global countdown starts | minutes, shipped `5` |
| NotifyTime | After the scheduled time: entrance closes and players inside wait this long before the fight starts | minutes, shipped `1` |
| EventTime | Duration of the fight | minutes, shipped `15` |
| CloseTime | After the fight: rewards are paid and players stay inside this long before being sent out | minutes, shipped `4` |

Timeline for a schedule row at 20:00 with the shipped values: entrance opens 19:55, closes 20:00, fight 20:01 to 20:16, closing phase until 20:20.

### Section 1 - Schedule

`Year Month Day DoW Hour Minute Second`. See [Schedule syntax](schedule-syntax.md). The time is when the **entrance closes**.

### Section 2 - Experience reward

| Column | Meaning | Values |
|---|---|---|
| Level | Castle | `0`-`7` |
| ExperienceTable1 | EXP for the winner and his party members | number |
| ExperienceTable2 | EXP for everyone else still inside, and for a player who dies during the fight | number |

### Section 3 - Zen reward

| Column | Meaning | Values |
|---|---|---|
| Level | Castle | `0`-`7` |
| MoneyTable1 | Zen for the winner and his party | number |
| MoneyTable2 | Zen for everyone else | number |

### Section 4 - Gate, statue and monster count

| Column | Meaning | Values |
|---|---|---|
| Level | Castle | `0`-`7` |
| GateNpcLife | HP of the castle gate | number |
| StatueNpcLife | HP of the Saint Statue | number |
| GateMonster | Monsters to kill before the gate can be attacked | number, `-1` = default (40 per player inside) |
| StatueMonster | Monsters to kill before the statue stage | number, `-1` = default (2 per player inside) |

### Section 5 - Coin reward

| Column | Meaning | Values |
|---|---|---|
| Level | Castle | `0`-`7` |
| WC | WCoinC given to each winner | number, `0` = none |
| WP | WCoinP given to each winner | number |
| GP | GoblinPoint given to each winner | number |

## Example

```
0
//WarningTime   NotifyTime   EventTime   CloseTime
5               1            15          4          // open 5 min before, 1 min wait, 15 min fight, 4 min close
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     12     0        0      // every day 12:00
*        *       *     *     20     0        0      // every day 20:00
end

2
//Level   ExperienceTable1   ExperienceTable2
0         20000              5000                   // BC1: winner party 20000, others 5000
7         130000             40000                  // BC8
end

4
//Level   GateNpcLife   StatueNpcLife   GateMonster   StatueMonster
0         65000         150000          -1            -1     // default monster counts
7         345000        650000          60            5      // fixed counts for BC8
end

5
//Level   WC   WP   GP
7         50   0    10                                // BC8 winners get 50 WCoinC + 10 GoblinPoint
end
```

## Schedule

Section 1 uses the shared format, see [Schedule syntax](schedule-syntax.md). Shipped: every 2 hours on even hours (00:00, 02:00 ... 22:00). One schedule drives all 8 castles.

```
1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     0      0        0  // Blood Castle
*        *       *     *     2      0        0  // Blood Castle
end
```

## Rewards

| Who | What | Configured in |
|---|---|---|
| Winner (player who returns the weapon) and his party | EXP (ExperienceTable1), Zen (MoneyTable1), item bag drop, coins | Sections 2, 3, 5 + item bag |
| Other players still inside | EXP (ExperienceTable2), Zen (MoneyTable2) | Sections 2, 3 |
| Player who dies during the fight (removed from the event) | EXP and Zen of column 2 | Sections 2, 3 |

Item reward: one drop per winner at his feet from the event item bag of his castle:

| Castle | Bag (`Data/EventItemBagManager.txt` index) | File |
|---|---|---|
| BC1 | 12 | `Data/EventItemBag/012 - Blood Castle 1.txt` |
| BC2 ... BC7 | 13 ... 18 | `013 - Blood Castle 2.txt` ... `018 - Blood Castle 7.txt` |
| BC8 | 19 | `Data/EventItemBag/019 - Blood Castle 8.txt` |

Coins (section 5) are given only when at least one of WC/WP/GP is above 0, with fireworks. Winning also counts as a Blood Castle completion for the Battle Pass and saves the score to the Blood Castle ranking.

The Morpheus control panel has a visual editor for event item bags.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startbc` | Admin (GameMaster 2) | Adds a one-off start 2 minutes from now. Because WarningTime is 5, the entrance opens immediately |
| `/reload event` | Admin | Re-reads this file and resets the schedule |

## Pitfalls

* **No EXP is paid with the shipped Common.dat.** The EXP of section 2 is multiplied by `AddEventExperienceRate_ALx`, and those keys ship as `0`. Set them to `1` (or more) to pay the table values.
* Shipped section 4 has `05000` as StatueNpcLife for Level 1 (BC2): the statue of BC2 has 5,000 HP. It is probably a typo of `205000`.
* `/startbc` does nothing while `BloodCastleEvent = 0`. The one-off start is lost on the next `/reload event` or restart.
* `/startbc` used at 23:58 or 23:59 computes hour 24 and may never fire. Use it at another time.
* The level column is 0-based: `0` is BC1, `7` is BC8. A row with Level 8 or more is out of range.
* A WarningTime larger than the gap between two schedule rows keeps the entrance open permanently.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `Data/Event/BloodCastle.dat` and recalculates the next start. Changes to `PkCanEnterBC` or `AddEventExperienceRate_ALx` need `/reload common`. Item bag changes need `/reload eventitembag`.

{% hint style="warning" %}
`/reload event` puts every castle back to its idle state. A Blood Castle that is running is cut short and nobody is rewarded. Reload between events.
{% endhint %}
