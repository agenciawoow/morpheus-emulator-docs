# King of MU

Guilds fight over a throne tile in Devias: the guild that holds it longest wins coins for every online member.

## What it is

At the scheduled time the server announces that King of MU has started. A guild member walks to the throne coordinate (shipped: Devias 225, 237) and types `/rei`. Their guild "takes the castle", and the whole server sees the announcement. Any other guild can take it back the same way while the event runs.

* The event lasts `EventTime` minutes. In the last 5 minutes the server counts, once per minute, which guild is holding the throne. The guild with the most counted minutes wins.
* Each `/rei` that succeeds also counts one capture for the character who typed it.
* When the event ends, the server announces the winning guild and its "ownership time", pays every **online** member of that guild, then names the 5 characters with the most captures and pays each of them.
* The result is saved to the King of MU guild and player rankings.

While the event is running, players on the event map can't hide behind a personal PvP-off setting: it's switched off when they're attacked.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `BattleIce3Event` | `1` | `1` turns King of MU on. `0` disables both the schedule and `/rei` |
| `GameServerInfo - Event.dat` | `BattleIce3Map` | `2` | Map of the throne (2 = Devias) |
| `GameServerInfo - Event.dat` | `BattleIce3CordX` | `225` | Throne X. The player must stand on exactly this tile |
| `GameServerInfo - Event.dat` | `BattleIce3CordY` | `237` | Throne Y |
| `GameServerInfo - Event.dat` | `BattleIce3GuildMinMembers` | `2` | Minimum members the guild must have to use `/rei` |

{% hint style="info" %}
The internal name of this event is "Battle Ice 3", which is why the keys and the file are called `BattleIce3`. In game and in the event list it's **King of MU**.
{% endhint %}

## Files & syntax

File: `Data/Event/BattleIce3.dat`. Four sections.

### Section 0: timings

| Column | Meaning | Values |
|---|---|---|
| WarningTime | Minutes before the start when the countdown announcements begin | minutes |
| EventTime | Duration of the event | minutes |

### Section 1: schedule

| Column | Meaning | Values |
|---|---|---|
| Year Month Day DoW Hour Minute Second | Start time | See [schedule syntax](schedule-syntax.md). DoW: 1 = Sunday ... 7 = Saturday |

### Section 2: guild reward

One row. Paid to **every online member** of the winning guild, on any map.

| Column | Meaning | Values |
|---|---|---|
| WC | WCoinC per member | number |
| WP | WCoinP per member | number |
| GP | Goblin Points per member | number |

### Section 3: character reward

One row. Paid to **each** of the top 5 characters by number of captures (the same amount to each of them).

| Column | Meaning | Values |
|---|---|---|
| WC | WCoinC | number |
| WP | WCoinP | number |
| GP | Goblin Points | number |

## Example

```
0
//WarningTime   EventTime
5               30          // countdown from 5 minutes before, event lasts 30 minutes
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     22     5        0     // every day at 22:05
*        *       *     7     16     0        0     // and Saturdays at 16:00
end

//Reward Guild
2
//WC    WP    GP
10      0     0          // 10 WCoinC to each online member of the winning guild
end

//Reward Characters
3
//WC    WP    GP
8       0     0          // 8 WCoinC to each of the top 5 capturers
end
```

{% hint style="warning" %}
In the shipped file **every row is commented out** (each line starts with `//`). With no rows in section 0 the event has no duration, and with no rows in section 1 it never starts by itself. Uncomment the rows (or write your own) before you rely on it.
{% endhint %}

## Schedule

Section 1 uses the shared format described in [Schedule syntax](schedule-syntax.md). The countdown (`[King of MU] Starts in N minute(s)`) begins `WarningTime` minutes before each start. The next start also appears in the Event Time window (H key) as "King of Mu".

## Rewards

| Who | Configured in | Delivered as |
|---|---|---|
| Every online member of the winning guild | Section 2 (`WC WP GP`) | Coins, straight to the account |
| Top 5 characters by captures | Section 3 (`WC WP GP`) | Coins, same amount for each |

There is no item reward. Rewards are only paid when there is a winning guild. If no guild held the throne at any of the per-minute checks, the server announces `There was no winner!` and nobody is paid.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startking` | Admin | Adds a one-off start 2 minutes from now (Command.txt index 79) |
| `/rei` | Player | Claim the throne. Must be in a guild with at least `BattleIce3GuildMinMembers` members, standing on the throne tile, while the event is running (Command.txt index 27) |

## Pitfalls

* **Only the last 5 minutes decide the winner.** Guild time is counted once per minute when 5, 4, 3, 2 and 1 minute(s) are left. Holding the throne earlier in the event only adds to the character capture count, not to the guild's time. A guild can win with at most 5 counted minutes, whatever `EventTime` is.
* **Ties go to the guild that first took the throne.** The winner must have strictly more counted minutes than the others.
* **The tile must be exact.** `/rei` checks map, X and Y exactly. One tile off gives `You are not at the correct coordinates!`.
* **The same guild can't re-take the throne.** If your guild already owns it, `/rei` answers `Your guild already owns the castle!`.
* **Hard-coded prize text.** The announcement messages 722 (`10 WC!!!`) and 725 (`Each won 10 WC!`) in `Data/Lang/*_Message.txt` state a fixed amount. If you change the reward, edit those texts too, or players will be told the wrong prize.
* **Guild reward goes to online members anywhere**, not only to those who fought.
* **Limits:** up to 20 guilds and 100 characters are tracked per event.
* `/startking` used in the last two minutes before midnight produces an hour of 24, which never matches. Start it after midnight instead.
* A start added with `/startking` is lost on the next `/reload event` or restart.

## Reload module

| Change | Command |
|---|---|
| `BattleIce3.dat` (timings, schedule, rewards) | `/reload event` |
| `BattleIce3*` keys in `GameServerInfo - Event.dat` | `/reload event` |
| Announcement texts | `/reload lang` |

`/reload event` re-reads the file and resets the event to idle with a fresh next start. A King of MU that is running when you reload is cut short, so reload between events.
