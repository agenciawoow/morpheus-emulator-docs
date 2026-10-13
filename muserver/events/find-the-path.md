# Find the Path

An NPC appears at a random spot in Lorencia or Devias. The first player to reach and click it wins a drop, and the NPC moves somewhere new until time runs out.

## What it is

1. A countdown is broadcast before the start.
2. When it starts, the server spawns the event NPC (Santa Claus, monster `465`, by default) on a random free tile. Lorencia is picked about two times in three and Devias one time in three. Everyone sees `[FINDPATH] be quick and go to Lorencia (x, y)!`, or `(?, ?)` if coordinates are hidden.
3. The first player to click the NPC wins. The NPC drops its item bag at the player's feet with fireworks, and the server announces the winner.
4. As long as event time remains, the NPC respawns at a new random spot and the hunt starts again, so one event can have several winners.
5. When the time ends, the NPC disappears.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Event.dat` | `FindPathEventToNpc` | `1` | `1` turns the event on. With `0`, `/startfindpath` does nothing and clicking the NPC gives no prize |

## Files & syntax

File: `Data/Event/FindPathNpc.dat`. It has two sections.

{% hint style="warning" %}
The shipped file has **every row commented out**, including section 0. Without section 0 the event time is 0 minutes, so the NPC appears and is removed on the same second. Uncomment section 0 before using the event.
{% endhint %}

### Section 0: settings

| Column | Meaning | Values |
|---|---|---|
| NotifyTime | Minutes before the start when the countdown begins | minutes |
| EventTime | How long the hunt lasts. The NPC keeps respawning until this runs out | minutes |
| NpcIndex | Monster class used as the NPC | monster index from `Data/Monster/Monster.txt`. Default `465` (Santa Claus) |
| HideCoord | Hide the coordinates in the announcement | `0` = show `(x, y)`, `1` = show `(?, ?)` (map name only) |

### Section 1: schedule

`Year Month Day DoW Hour Minute Second`. See [Schedule syntax](schedule-syntax.md).

## Example

```
0
//NotifyTime  EventTime  NpcIndex  HideCoord
  2           10         465       0          // 2-min countdown, 10-min hunt, Santa Claus, show x/y
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     0      30       0      // every day at 00:30
*        *       *     *     18     30       0      // every day at 18:30
end
```

## Schedule

Start times are in section 1. See [Schedule syntax](schedule-syntax.md). `/startfindpath` adds a one-off start about 2 minutes ahead. The one-off start lasts until the next `/reload event` or restart.

## Rewards

Each find drops the event item bag with **SpecialValue 62** on the ground where the player stands:

| File | Row |
|---|---|
| `Data/EventItemBagManager.txt` | `129  *  *  *  62  //Event FindPathToNpc` |
| `Data/EventItemBag/129 - FindPathToNpc.txt` | The bag itself: drop count, rates, fireworks, item list |

{% hint style="danger" %}
As shipped, `129 - FindPathToNpc.txt` has an **empty item list** (every item row is commented out). The winner gets fireworks and nothing else. Add items to the bag. The Morpheus control panel has a visual EventItemBag editor for this.
{% endhint %}

No coins are given by this event.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startfindpath` | Admin (`#GameMaster` 2) | Schedules a start about 2 minutes from now |

## Pitfalls

* The NPC class must be clickable. Use a talk NPC (the default `465` works). A monster class can't be "clicked".
* The default NPC `465` is the same class used by the **Quickly** event. If both run at once, a click on the Find the Path NPC counts for Find the Path, but see [Quickly](quickly.md) for how that event reacts to any Santa Claus.
* The spawn point is random over the whole map, so it can land far from town. Use `HideCoord 0` on servers with new players.
* `/startfindpath` around midnight can produce an invalid hour (24) and never fire. Use a schedule row instead.

## Reload module

`/reload event` re-reads `FindPathNpc.dat` and `FindPathEventToNpc`, removes the current NPC and recalculates the next start. Changes to the bag file need `/reload eventitembag`.

{% hint style="warning" %}
Reload between events. A hunt in progress ends at once on `/reload event` (the NPC is removed). `/reload monster` also deletes the event NPC, so don't use it during a hunt either.
{% endhint %}
