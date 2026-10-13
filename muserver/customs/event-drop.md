# Event Drop

A scheduled "item rain": for a few minutes the server drops items on the ground around a spot of your choice, with fireworks.

## What it is

The server announces that the drop event is about to start in a town or field. When it starts, items fall one by one around the configured spot, each with a fireworks effect, following the timing you set. Anyone nearby can pick them up. When the time is over the server announces the end. In the in-game event time window (**H**) it shows as "Custom Drop".

The shipped file runs three drops (Lorencia, Noria, Devias) six times a day, each dropping jewels (Chaos, Bless, Soul, Life) every 5 seconds for 1 minute.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomEventDropSwitch` | `1` | `1` = drops follow their schedule. `0` = no drop runs, and `/startdrop` does nothing. |
| `GameServerInfo - Custom.dat` | `CustomEventDropText1` | `[%s] will start in %d minute(s)` | Countdown, sent to everyone every minute during `AlarmTime`. `%s` = drop name, `%d` = minutes. |
| `GameServerInfo - Custom.dat` | `CustomEventDropText2` | `[%s] started` | Start announcement. |
| `GameServerInfo - Custom.dat` | `CustomEventDropText3` | `[%s] finished` | End announcement. |

## Files & syntax

File: `Data/Custom/CustomEventDrop.txt`. Three sections. Up to 10 drops (`Index` 0..9).

### Section 0: schedule

`Index Year Month Day DoW Hour Minute Second`, one row per start time. `Index` is the drop from section 1. See [schedule syntax](../events/schedule-syntax.md).

### Section 1: drop rules

| Column | Meaning | Values |
|---|---|---|
| `Index` | Drop number | 0..9 |
| `Name` | Name used in the announcements | Text in quotes |
| `DropMap` | Map where items fall | Map number (0 = Lorencia, 2 = Devias, 3 = Noria, ...) |
| `DropX` / `DropY` | Centre of the drop area | Map coordinates |
| `DropRange` | Items fall at a random spot up to this many tiles from the centre | Tiles. If no free tile is found, the item falls on the centre |
| `AlarmTime` | Minutes of countdown before the start | Minutes |
| `EventTime` | Duration of the drop | Minutes |

### Section 2: items

One row per drop. All rows with the same `Index` belong to that drop.

| Column | Meaning | Values |
|---|---|---|
| `Index` | Drop this row belongs to | 0..9 |
| `ItemIndex` | Item code | `Type * 512 + Index` (Jewel of Bless = 7181) |
| `ItemLevel` | Item level | 0..15 |
| `DropCount` | How many copies fall at once | Number |
| `DropDelay` | When this row drops, counted from the start | **Seconds** |

Each row drops only once per run. To drop the same item several times, repeat the row with different `DropDelay` values.

## Example

```
0
//Index   Year   Month   Day   DoW   Hour   Minute   Second
0         *      *       *     *     20     00       0      // every day at 20:00
0         *      *       *     1     15     00       0      // and on Sundays at 15:00
end

1
//Index   Name                  DropMap   DropX   DropY   DropRange   AlarmTime   EventTime
0         "Event Drop Lorencia" 0         145     135     7           5           2
end

2
//Index   ItemIndex   ItemLevel   DropCount   DropDelay
0         7181        0           3           0          // 3 Jewels of Bless at the start
0         7182        0           3           30         // 3 Jewels of Soul after 30 seconds
0         6159        0           1           60         // 1 Jewel of Chaos after 1 minute
0         7184        0           2           110        // 2 Jewels of Life near the end
end
```

## Schedule

Event Drop uses the shared format in [Schedule syntax](../events/schedule-syntax.md). The scheduled time is the moment the items start falling; the countdown starts `AlarmTime` minutes before. The shipped schedule:

```
0
//Index   Year   Month   Day   DoW   Hour   Minute   Second
// Lorencia - 6 times a day
0         *      *       *     *     1      40       0
0         *      *       *     *     5      40       0
0         *      *       *     *     9      40       0
...
end
```

Each drop index has its own list of times. When several drops are scheduled, the H window shows the one starting soonest.

## Rewards

The items in section 2, dropped on the ground. They have no owner: any player can pick them up. Set the drop spot away from safe zones you do not want crowded, and use `DropRange` to spread the items.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startdrop` | Admin (`GameMaster = 2`, index 78 in `Data/Command.txt`) | Starts drop **index 0** two minutes from now. |
| `/reload custom`, `/reload event` | Admin | See Reload module. |

## Pitfalls

* `/startdrop` always starts drop **0**. Put the drop you want to start by hand at index 0.
* `/startdrop` re-initialises every drop schedule. With `CustomEventDropSwitch = 0` it stops everything instead of starting.
* `DropDelay` is in **seconds** and `EventTime` in **minutes**. A row with `DropDelay` larger than `EventTime * 60` never drops.
* Keep `Index` between 0 and 9 in every section. The file does not check the range, and a higher index can crash the GameServer on load.
* Items fall only on walkable tiles. If the area around `DropX`/`DropY` is all walls, every item falls on the centre tile.

## Reload module

* `/reload custom` re-reads `CustomEventDrop.txt` and the `CustomEventDrop*` keys.
* `/reload event` puts every drop back into its waiting state, recalculates the next start time and applies a changed `CustomEventDropSwitch`. It interrupts a drop that is running.
* Safe order: edit, `/reload custom`, then `/reload event` while no drop is running.
