# GvG Event

Guild versus Guild: guilds register at an NPC, fight on a closed map, and the guild with the most kills wins coins for its members.

## What it is

1. Before the start, a notice says registration is open and counts down the minutes.
2. While registration is open, the **guild master** talks to the event NPC (Lorencia 130/122 by default), pays the entry item if one is set, and is sent to the waiting area. After that, other members of the same guild can talk to the NPC and join for free.
3. At the start time registration closes and everyone waits `StandTime` minutes in the waiting area.
4. The fight starts: each guild is teleported to its own gate. A kill gives **1 point to the killer's guild**, announced to all participants. Dead players respawn at their guild's gate. Members of the same guild can't hurt each other.
5. When the time ends, the guild with the most points wins. Its members still in the event get coins. Everyone is sent back to Lorencia.

To join you can't be in a party, a guild war, a duel or a personal shop, and you must be in a guild.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Event.dat` | `EventGvGSwitch` | `1` | `1` turns the event on |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventGvGNpc` | `380` | Monster class of the registration NPC |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventGvGNpcMap` | `0` | NPC map (0 = Lorencia) |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventGvGNpcX` | `130` | NPC X |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventGvGNpcY` | `122` | NPC Y |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventGvGMinGuild` | `2` | **Not read by the server** (see the warning below) |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventGvGMaxGuild` | `20` | **Not read by the server** (see the warning below) |

{% hint style="warning" %}
The shipped file uses `EventGvGMinGuild` / `EventGvGMaxGuild`, but the GameServer reads **`EventGvGMinUsers`** and **`EventGvGMaxUsers`**. These count **players**, not guilds, and default to `2` and `20` when missing. To change the limits, add the real keys:

```
EventGvGMinUsers = 4     ; event is cancelled with fewer players than this
EventGvGMaxUsers = 40    ; registration refuses players past this number (hard cap 100)
```

At least **2 guilds** are always required. A maximum of **20 guilds** is fixed in the server.
{% endhint %}

The NPC is **not** spawned by the event. Class, map, X and Y must match an NPC that exists in `Data/MonsterSetBase`, or clicking it opens nothing. The shipped `000 - Lorencia.txt` has no NPC 380 at 130/122, so add one:

```
//Monster      MapNumber      Range      PositionX      PositionY      Direction      Comment
380            00             00         130            122            03             //GvG Event NPC
```

## Files & syntax

File: `Data/Event/GvGEvent.dat`. It has four sections.

{% hint style="warning" %}
The shipped file has **every row commented out**. Uncomment and fill sections 0, 2 and 3 before using the event. With section 0 empty every phase lasts 0 minutes.
{% endhint %}

### Section 0: timings

| Column | Meaning | Values |
|---|---|---|
| AlarmTime | Registration window before the start. The NPC accepts players only during these minutes | minutes |
| StandTime | Waiting time after registration closes, before the fight | minutes |
| EventTime | Fight duration | minutes |
| CloseTime | Time after the result before everyone is sent to Lorencia | minutes (`0` = immediately) |

### Section 1: schedule

`Year Month Day DoW Hour Minute Second`. See [Schedule syntax](schedule-syntax.md).

### Section 2: entry rules and arena

{% hint style="danger" %}
The file header lists `ReqItemIndex ReqItemLevel ReqItemCount`, but the server reads the first three columns as **`ReqItemCount ReqItemIndex ReqItemLevel`**. Follow the table below.
{% endhint %}

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | ReqItemCount | How many of the entry item the **guild master** must have. Removed on entry | `0` = free |
| 2 | ReqItemIndex | Entry item, as `Category*512 + Index` | for example `7181` = 14/13 Jewel of Bless |
| 3 | ReqItemLevel | Required item level | `-1` / `*` = any level |
| 4 | EventMap | Map where the event happens | map number. Shipped example: `84` |
| 5 | WaitingGate | Gate (in `Data/Move/Gate.txt`) for the waiting area | gate index, must be on EventMap |
| 6 | StartGate | Gate of the **first** guild. Guild N uses `StartGate + N - 1` | gate index |
| 7 | MinLevel | Minimum character level | `*` = no limit |
| 8 | MaxLevel | Maximum level | `*` = no limit |
| 9 | MinReset | Minimum resets | `*` = no limit |
| 10 | MaxReset | Maximum resets | `*` = no limit |
| 11 | MinMasterReset | Minimum master resets | `*` = no limit |
| 12 | MaxMasterReset | Maximum master resets | `*` = no limit |

The shipped `Gate.txt` has gate `600` (waiting area) and gates `601`-`604` (guilds 1-4) on map 84.

### Section 3: reward

The header says `Cash Gold PcCoint`. They are the standard coins:

| Column | Meaning | Values |
|---|---|---|
| Coin1 | WCoinC for each member of the winning guild still in the event | number |
| Coin2 | WCoinP, same | number |
| Coin3 | GoblinPoint, same | number |

## Example

```
0
//AlarmTime   StandTime   EventTime   CloseTime
5             1           15          1            // 5-min registration, 1-min wait, 15-min fight
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     3     20     30       0      // Tuesdays at 20:30
end

2
//ReqItemCount ReqItemIndex ReqItemLevel EventMap WaitingGate StartGate MinLevel MaxLevel MinReset MaxReset MinMasterReset MaxMasterReset
1              7181         0            84       600         601       100      *        5        *        *              *
end

//Reward Guild
3
//WC   WP   GP
50     0    0
end
```

Here the guild master pays one Jewel of Bless (+0) for the whole guild.

## Schedule

Section 1, standard format: see [Schedule syntax](schedule-syntax.md). Registration opens `AlarmTime` minutes **before** each start time. The countdown shows in the event timer window. `/startgvg` adds a one-off start about 2 minutes ahead, so registration is only open for those 2 minutes.

## Rewards

* Winner: the guild with the most kill points. On a tie, the guild whose master registered first wins.
* Every member of the winning guild **who is still on the event map** when the fight ends gets the section 3 coins (WC/WP/GP), credited to the account.
* No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startgvg` | Admin (`#GameMaster` 2) | Schedules a start about 2 minutes from now |

## Pitfalls

* NPC not spawned, or class/map/X/Y not matching exactly: the NPC behaves like a normal NPC and the event looks dead.
* Each registered guild needs its own gate. With more guilds than gates (`601`-`604` shipped), guild 5 and later aren't teleported to a start point. Add consecutive gates to `Gate.txt` or limit the number of players.
* Members can only join **after** their guild master has registered ("The Guild's GameMaster must enter first!" means the guild master).
* Leaving the event map (teleport, warp, disconnect) removes the player from the event. If only one player is left, the fight ends early.
* The fight is cancelled if, during the waiting phase, fewer than 2 guilds or fewer than `EventGvGMinUsers` players are still on the map.
* `EventMap` must be the map the gates point to. Players found on any other map are dropped from the event.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `Data/Event/GvGEvent.dat` and recalculates the next start.

{% hint style="warning" %}
Reload between events. A reload during registration or the fight wipes the participant list **without teleporting anyone out**: players are left on the event map and must leave on their own, and nobody is rewarded.
{% endhint %}
