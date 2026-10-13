# Event names

`EventName.dat` holds the names of the events, invasions and arenas that the server sends to the in-game event time window (H key) and to the "starts in 5 minutes" / "has started" notices.

## What it is

* Players press **H** to open the event time window. It lists each event and invasion with its next start time, or shows it as running.
* About 5 minutes before an event or invasion, and again when it starts, players get a notice with its name.
* Those names come from this file. The window's icons and the client-side names come from the client file `Local/EventNameDisplay.txt`.

This file holds only names. The real start times are in each event's own file (`BloodCastle.dat`, `InvasionManager.dat`, ...).

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Custom.dat` | `CustomEventTimeSwitch` | `1` | `1` = the H window and the event notices are active |

## Files & syntax

`Data/Event/EventName.dat`, 3 sections. Each row is `Key "Name"`. Keys go from `0` to `49`.

| Column | Meaning | Values |
|---|---|---|
| Key | Index of the event in that section | `0`-`49` |
| Name | Name shown to players | text in double quotes |

### Section 0 - Global events (fixed indexes)

These indexes are fixed by the server. Only rename them, never renumber.

| Key | Event | Page |
|---|---|---|
| 0 | Blood Castle | [Blood Castle](blood-castle.md) |
| 1 | Devil Square | [Devil Square](devil-square.md) |
| 2 | Chaos Castle | [Chaos Castle](chaos-castle.md) |
| 3 | Illusion Temple | [Illusion Temple](illusion-temple.md) |
| 4 | Online Lottery | [Online Lottery](../customs/online-lottery.md) |
| 5 | Quiz | [Quiz](../customs/quiz.md) |
| 6 | Bonus Manager window | [Bonus Manager](bonus-manager.md) |
| 7 | Event Drop | [Event Drop](../customs/event-drop.md) |
| 8 | King of MU | [King of MU](king-of-mu.md) |
| 9 | TvT Event | [TvT Event](tvt-event.md) |
| 10 | GvG Event | [GvG Event](gvg-event.md) |
| 11 | Moss Merchant | [Moss Merchant](moss-merchant.md) |
| 12 | Morpheus Tournament | [Morpheus Tournament](morpheus-tournament.md) |

### Section 1 - Invasions

Key = the **Index** of the invasion in `Data/Event/InvasionManager.dat`. See [Invasions](invasions.md).

### Section 2 - Arenas

Key = the index of the arena in `Data/Custom/CustomArena.txt`. See [Custom Arena](../customs/custom-arena.md).

## Example

```
0
//Key   Name
0       "Blood Castle"
1       "Devil Square"
6       "Double EXP"              // Bonus Manager window, renamed
11      "Moss the Gambler"
end

1
//Key   Name   (== Index in InvasionManager.dat)
0       "Skeleton King"
1       "Red Dragon"
16      "Golden Goblin"           // new invasion 16
end

2
//Key   Name
0       "Survivor"
1       "DM vs DM"
end
```

## Schedule

This file has no schedule. Each event's start times are in its own file, in the format described in [Schedule syntax](schedule-syntax.md).

## Rewards

None. This file only names things.

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin (GameMaster 2) | Re-reads `EventName.dat` with all event files |

## Pitfalls

* **Server and client must use the same indexes.** The client file `Local/EventNameDisplay.txt` (in your MainInfo customs) has the same section 0 and section 1 with `Index "Name" "IconName"`. If an index is missing or different on one side, the H window shows the wrong name or icon. See [Client custom files](../client/custom-files.md).
* The shipped files do not match for invasions: the server lists invasions 0 to 15, the client lists only 0 to 9 and its 9 is "Medusa2" (server: "Mini Boss Balrog"). Add 9 to 15 to the client file with the server's names.
* A global event (section 0) with no row here gets no "5 minutes" / "started" notice, even if it is scheduled.
* An invasion with schedule rows but no name here still shows in the window, named "No Name" and without its countdown.
* Keys of 50 or more are ignored.
* Client-side changes need the client to be repacked and sent to players. Server-side changes do not.

## Reload module

`/reload event` re-reads `EventName.dat`. Changes to `CustomEventTimeSwitch` need `/reload custom`. The client file `EventNameDisplay.txt` never reloads: rebuild and redistribute the client.

{% hint style="warning" %}
`/reload event` also resets every event's schedule and cuts short any running event. Reload between events.
{% endhint %}
