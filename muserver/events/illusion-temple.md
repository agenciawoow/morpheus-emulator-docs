# Illusion Temple

Scheduled team event: two teams fight inside the temple for the sacred item, and the winning team is rewarded.

## What it is

* During the entry window players enter with a **Scroll of Blood** of the right level (item 13,51, level = temple number) or an **Illusion Temple Ticket** (item 13,61).
* Up to 10 players per temple are split into two teams. The team that scores more by delivering the sacred item wins. On a tie, the team with more kill points wins, then the team with the lower total level.
* There are **6 temples**:

| Temple (Level) | Character level |
|---|---|
| 0 (IT1) | 220 - 270 |
| 1 (IT2) | 271 - 320 |
| 2 (IT3) | 321 - 350 |
| 3 (IT4) | 351 - 380 |
| 4 (IT5) | 381 - max level |
| 5 (IT6) | any level after the 3rd class change |

Brackets are for a 400 max level and scale with `MaxLevelCharacter` above 400.

* After the fight, players of the winning team can press the reward button to get an extra prize, with a small chance of a Fenrir horn.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `IllusionTempleEvent` | `0` | `1` = event runs on its schedule |
| `GameServerInfo - Event.dat` | `IllusionTempleMinUser` | `1` | Minimum players for the temple to start. Each team needs at least `MinUser / 2` (integer division), otherwise that temple is cancelled |
| `GameServerInfo - Event.dat` | `IllusionTempleRewardFenrirRate` | `5` | Chance, out of 10000, that the reward button gives a Horn of Fenrir instead of the item bag. `5` = 0.05% |

The GameServerCS ships with `IllusionTempleMinUser = 2`.

## Files & syntax

`Data/Event/IllusionTemple.dat`, 4 sections. **Every row of the shipped file is commented out**, so the event has no schedule and no reward values until you uncomment them.

### Section 0 - Timings (one row, minutes)

| Column | Meaning | Values |
|---|---|---|
| WarningTime | Minutes before the scheduled time when entry opens and the countdown starts | minutes, suggested `5` |
| NotifyTime | After the scheduled time: entry closed, teams wait inside. `IllusionTempleMinUser` is checked here | minutes, suggested `1` |
| EventTime | Duration of the fight | minutes, suggested `15` |
| CloseTime | After the fight: rewards, reward button active, then players are sent out | minutes, suggested `4` |

### Section 1 - Schedule

`Year Month Day DoW Hour Minute Second`, see [Schedule syntax](schedule-syntax.md). The time is when entry closes.

### Section 2 - Experience

| Column | Meaning | Values |
|---|---|---|
| Level | Temple | `0`-`5` |
| ExperienceTable1 | Base team EXP | number |
| ExperienceTable2 | EXP per point of score | number |

{% hint style="warning" %}
This build runs the Season 8 temple rules. Section 2 is read, but the team EXP is only calculated by the older (Season 6) scoring code, so in practice the temple pays **no EXP**. Use sections 3 and the item bags to reward players.
{% endhint %}

### Section 3 - Coin reward (winning team)

| Column | Meaning | Values |
|---|---|---|
| Level | Temple | `0`-`5` |
| WC | WCoinC per winner | number, `0` = none |
| WP | WCoinP per winner | number |
| GP | GoblinPoint per winner | number |

## Example

Uncomment and adjust the shipped rows:

```
0
//WarningTime   NotifyTime   EventTime   CloseTime
5               1            15          4
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     13     30       0      // every day 13:30
*        *       *     *     21     30       0      // every day 21:30
end

2
//Level   ExperienceTable1   ExperienceTable2
0         300000             50000
end

3
//Level   WC   WP   GP
0         10   0    0                               // each IT1 winner gets 10 WCoinC
5         10   0    0
end
```

## Schedule

Shared format, see [Schedule syntax](schedule-syntax.md). The shipped file has these rows commented: 01:30, 05:30, 09:30, 13:30, 17:30, 21:30.

```
1
//Year   Month   Day   DoW   Hour   Minute   Second
//*        *       *     *     1      30       0
//*        *       *     *     5      30       0
end
```

## Rewards

| When | Who | What |
|---|---|---|
| End of the fight | Each player of the winning team | One drop from the temple's item bag + coins of section 3 |
| Reward button (closing phase) | Each player of the winning team | Horn of Fenrir with `IllusionTempleRewardFenrirRate`/10000 chance, otherwise one more drop from the temple's bag. Coins of section 3 again |

Item bags, by temple:

| Temple | Bag index (`Data/EventItemBagManager.txt`) | File |
|---|---|---|
| IT1 | 56 | `Data/EventItemBag/056 - Illusion Temple 1.txt` |
| IT2 ... IT5 | 57 ... 60 | `057 - Illusion Temple 2.txt` ... `060 - Illusion Temple 5.txt` |
| IT6 | 61 | `Data/EventItemBag/061 - Illusion Temple 6.txt` |

## Commands related

| Command | Who | What |
|---|---|---|
| `/startit` | Admin (GameMaster 2) | One-off start 2 minutes from now. Only works with `IllusionTempleEvent = 1` |
| `/reload event` | Admin | Re-reads this file and resets the schedule |

## Pitfalls

* Turning on `IllusionTempleEvent` is not enough: with every schedule row commented out the temple never opens on its own.
* `IllusionTempleMinUser = 1` gives `1 / 2 = 0` per team, so a single player can start a temple alone. Use `2` or more.
* Section 2 EXP is not paid in this build (see the warning above). Even on older code, it is multiplied by `AddEventExperienceRate_ALx` in `GameServerInfo - Common.dat`, shipped as `0`.
* Coins of section 3 are paid at the end and again on the reward button, so the real coin prize is at least twice the value you set.
* The Level column is 0-based: `0` = IT1, `5` = IT6.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and this file and recalculates the next start. Bag files need `/reload eventitembag`.

{% hint style="warning" %}
`/reload event` resets every temple to idle. A running Illusion Temple ends with no rewards. Reload between events.
{% endhint %}
