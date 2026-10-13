# TvT Event

Team versus Team: players register one by one at an NPC, are split into a Blue team and a Red team, and the team with more kills wins.

## What it is

1. Before the start, a notice announces how many minutes are left to register.
2. While registration is open, players talk to the event NPC (Lorencia 130/121 by default). Each player pays the entry item if one is set and is sent to the waiting area.
3. At the start time registration closes and everyone waits `StandTime` minutes.
4. Players are split into **Blue** and **Red**, alternating in registration order, and teleported to their team gate. If `SetSkin` is on, Blue players look like Illusion Sorcerers and Red players look like MU Allies (the Illusion Temple team skins).
5. Each kill of an enemy adds a point to the killer's team. Everyone sees the live score, plus their own kill and death count. Dead players respawn at their team gate. Teammates can't hurt each other.
6. When the time ends, the team with more points wins and its members get coins. On a tie nobody is rewarded. Everyone is sent back to Lorencia and the skins are removed.

To join you can't be in a party, a guild war, a duel or a personal shop.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Event.dat` | `EventTvtSwitch` | `0` | `1` turns the event on. **Off as shipped** |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventTvtNpc` | `380` | Monster class of the registration NPC |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventTvtNpcMap` | `0` | NPC map (0 = Lorencia) |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventTvtNpcX` | `130` | NPC X |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventTvtNpcY` | `121` | NPC Y |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventTvtMinUsers` | `2` | Fewer registered players than this at the start (or during the wait) cancels the event |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventTvtMaxUsers` | `20` | Registration refuses players past this number. The server caps it at **40** |

The NPC is **not** spawned by the event. Class, map, X and Y must match an NPC in `Data/MonsterSetBase`. The shipped `000 - Lorencia.txt` has no NPC 380 at 130/121, so add one:

```
//Monster      MapNumber      Range      PositionX      PositionY      Direction      Comment
380            00             00         130            121            03             //TvT Event NPC
```

## Files & syntax

File: `Data/Event/TvTEvent.dat`. It has four sections.

{% hint style="warning" %}
The shipped file has **every row commented out**. Uncomment and fill sections 0, 2 and 3 before turning the event on.
{% endhint %}

### Section 0: timings

| Column | Meaning | Values |
|---|---|---|
| AlarmTime | Registration window before the start | minutes |
| StandTime | Waiting time after registration closes | minutes |
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
| 1 | ReqItemCount | How many of the entry item **each** player must have. Removed on entry | `0` = free |
| 2 | ReqItemIndex | Entry item, as `Category*512 + Index` | for example `7181` = Jewel of Bless |
| 3 | ReqItemLevel | Required item level | `-1` / `*` = any level |
| 4 | EventMap | Map where the event happens | map number |
| 5 | WaitingGate | Gate for the waiting area | gate index on EventMap |
| 6 | BlueTeamGate | Blue team start and respawn gate | gate index on EventMap |
| 7 | RedTeamGate | Red team start and respawn gate | gate index on EventMap |
| 8 | SetSkin | Team skins | `1` = Blue looks like monster 405, Red like monster 404. `0` = no skin |
| 9 | MinLevel | Minimum level | `*` = no limit |
| 10 | MaxLevel | Maximum level | `*` = no limit |
| 11 | MinReset | Minimum resets | `*` = no limit |
| 12 | MaxReset | Maximum resets | `*` = no limit |
| 13 | MinMasterReset | Minimum master resets | `*` = no limit |
| 14 | MaxMasterReset | Maximum master resets | `*` = no limit |

### Section 3: reward

The header says `WX WP GP` (a typo for WC WP GP):

| Column | Meaning | Values |
|---|---|---|
| Coin1 | WCoinC for each member of the winning team still in the event | number |
| Coin2 | WCoinP, same | number |
| Coin3 | GoblinPoint, same | number |

## Example

```
0
//AlarmTime   StandTime   EventTime   CloseTime
3             1           10          0            // 3-min registration, 1-min wait, 10-min fight
end

1
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     14     5        0      // every day at 14:05
*        *       *     *     22     5        0      // every day at 22:05
end

2
//ReqItemCount ReqItemIndex ReqItemLevel EventMap WaitingGate BlueTeamGate RedTeamGate SetSkin MinLevel MaxLevel MinReset MaxReset MinMasterReset MaxMasterReset
0              0            0            84       600         601          602         1       400      *        10       *        *              *
end

//Reward Characters
3
//WC   WP   GP
10     0    0
end
```

This example reuses gates 600-602 on map 84 from the shipped `Gate.txt` (also used by GvG). Free entry, team skins on, 10 WC per winner.

## Schedule

Section 1, standard format: see [Schedule syntax](schedule-syntax.md). Registration opens `AlarmTime` minutes before each start time. `/starttvt` adds a one-off start about 2 minutes ahead.

## Rewards

* Winner: the team with the higher score. **A tie pays nobody.**
* Every member of the winning team still on the event map gets the section 3 coins (WC/WP/GP), credited to the account. Their kills and deaths are saved to the TvT ranking.
* No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/starttvt` | Admin (`#GameMaster` 2) | Schedules a start about 2 minutes from now |

## Pitfalls

* `EventTvtSwitch` ships as `0`. Turning it on is not enough while every row of `TvTEvent.dat` is commented out.
* The commented example row in the shipped file uses `EventMap 6` with gates `347`/`348`/`349`, but those gates are on map **84**. `EventMap` must be the map the gates point to: anyone found on another map is dropped from the event, so a mismatch empties the event the moment players are teleported.
* NPC missing from `MonsterSetBase`, or not on the exact tile in `EventTvtNpcX`/`Y`: clicking does nothing special.
* Teams are split alternately by registration order, not by strength.
* The fight ends early if only one participant is left on the map. One team being wiped out does **not** end it, since dead players respawn.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `Data/Event/TvTEvent.dat` and recalculates the next start.

{% hint style="warning" %}
Reload between events. A reload during registration or the fight wipes the participant list **without teleporting anyone out or removing skins**: players are left on the event map and nobody is rewarded.
{% endhint %}
