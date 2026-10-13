# Schedule syntax

Every scheduled event in Morpheus uses the same start-time format: one row per start time, seven columns.

## What it is

When you see a section headed `//Year Month Day DoW Hour Minute Second` in an event file, each row in it is a start time. The server reads every row, works out the next moment each one matches, and arms the event for whichever of those comes first. After the event runs, the server works out the next start again.

That one format is shared by:

| File | Section with the schedule | Extra leading column |
|---|---|---|
| `Data/Event/BloodCastle.dat` | 1 | none |
| `Data/Event/DevilSquare.dat` | 1 | none |
| `Data/Event/ChaosCastle.dat` | 1 | none |
| `Data/Event/IllusionTemple.dat` | see page | none |
| `Data/Event/CastleDeepEvent.dat` | 0 | none |
| `Data/Event/MossMerchant.dat` | 0 | none |
| `Data/Event/InvasionManager.dat` | 0 | `Index` (which invasion) |
| `Data/Event/BonusManager.dat` | 0 | `Index` (which bonus) |
| `Data/Event/BattleIce3.dat`, `TvTEvent.dat`, `GvGEvent.dat`, `FindPathNpc.dat`, `MorpheusTournament.dat` | see each page | see each page |
| `Data/Custom/CustomArena.txt`, `CustomEventDrop.txt`, `CustomOnlineLottery.txt`, `CustomQuiz.txt` | see each page | `Index` |

## Files & syntax

```
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     20     0        0
```

| Column | Meaning | Values |
|---|---|---|
| Year | Calendar year | `*` = every year, or for example `2026` |
| Month | Month of the year | `*` = every month, `1`-`12` |
| Day | Day of the month | `*` = every day, `1`-`31` |
| DoW | Day of the week | `*` = any day, `1` = Sunday, `2` = Monday, `3` = Tuesday, `4` = Wednesday, `5` = Thursday, `6` = Friday, `7` = Saturday |
| Hour | Hour, 24-hour clock | `*` = every hour, `0`-`23` |
| Minute | Minute | `*` = every minute, `0`-`59` |
| Second | Second | `*` = every second, `0`-`59` (normally `0`) |

The rules:

* `*` is a wildcard: that field matches any value, and the server moves it forward to find the next start.
* A fixed number pins that field. The server never changes a pinned field. If a pinned date is already in the past, that row never fires again.
* Times are the **server machine's local clock**, not the player's.
* A file can hold as many rows as you need. The event starts at whichever row comes up next.
* A row that is commented out (`//*  *  *  *  20  0  0`) is ignored. If every row is commented out, the event never starts by itself, but a GM command can still start it.

{% hint style="info" %}
The day-of-week numbers start at **1 = Sunday**, not 0. `7` is Saturday.
{% endhint %}

## Example

```
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     0      0        0     // every day at 00:00
*        *       *     *     12     30       0     // every day at 12:30
*        *       *     7     21     0        0     // every Saturday at 21:00
*        *       *     1     15     0        0     // every Sunday at 15:00
*        *       1     *     20     0        0     // the 1st of every month at 20:00
*        12      25    *     19     0        0     // every Christmas at 19:00
2026     11      15    *     20     0        0     // once, 15 Nov 2026 at 20:00
*        *       *     *     *      0        0     // every hour, on the hour
```

Files with an `Index` column put it first. The index says which entry in the same file the row starts:

```
//Index   Year   Month   Day   DoW   Hour   Minute   Second
0         *      *       *     *     0      40       0      // invasion 0 at 00:40
0         *      *       *     *     12     40       0      // invasion 0 at 12:40
3         *      *       *     6     21     10       0      // invasion 3 every Friday 21:10
```

## Schedule

### Countdown and announcement times

Most classic events have a section 0 with timing values used together with the schedule:

```
//WarningTime   NotifyTime   EventTime   CloseTime
5               1            15          4
```

Each event's page explains those values. They are usually minutes: how long before the start players are warned, how long the entry stays open, how long the event lasts, and how long the closing phase takes.

### Event Time window (H key)

The in-game event time window, opened with **H**, lists the next start of each scheduled event. It is turned on by `CustomEventTimeSwitch = 1` in `GameServerInfo - Custom.dat`. The names shown there come from [`Data/Event/EventName.dat`](event-name.md).

## Pitfalls

* **`*` in Minute or Second** makes the row match every minute or every second. Put `0` there unless that's really what you want.
* **Pinned past dates** (`2025 1 1 * 20 0 0`) never fire again. Nothing tells you about it.
* **DoW with a fixed Day**: both must match, so `* * 13 6 20 0 0` only fires on Friday the 13th.
* **Overlap**: two rows close together, or an `EventTime` longer than the gap between starts, can cause a start to be skipped while the previous run is still active.
* **Time zone**: changing the clock or time zone on the server machine moves every event. Reload or restart after you change it.
* Column separators can be spaces or tabs. Keep exactly seven numbers per row (eight with `Index`). A missing column shifts the following row and corrupts the table.

## Reload module

| What you changed | How to apply |
|---|---|
| Any `Data/Event/*.dat` schedule | `/reload event` |
| Schedules in `Data/Custom/CustomArena.txt`, `CustomEventDrop.txt`, `CustomOnlineLottery.txt`, `CustomQuiz.txt` | `/reload custom` to read the file, then `/reload event` to recalculate the next start |

`/reload event` re-reads every event file, puts every scheduled event back into its idle state, and recalculates each next start time. That includes Blood Castle, Devil Square, Chaos Castle, Illusion Temple, Custom Arena, Event Drop, Online Lottery, Quiz, TvT, GvG, King of MU, Invasions, Moss Merchant and Find the Path. An on/off switch you changed with `/reload custom` (for example `CustomArenaSwitch`) also takes effect only after `/reload event`.

{% hint style="warning" %}
Because `/reload event` resets every event's state, an event that is running when you reload is cut short. Reload between events.
{% endhint %}
