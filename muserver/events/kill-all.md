# Kill All

A free-for-all opened by a GM: players gather at the GM's spot, fight everyone against everyone for points, and the top 3 when time runs out are rewarded.

## What it is

Players know it as **[MataMata]**.

1. The GM stands at the venue and types `/matamata`. The server announces "Opened (Max Participants: N)!" and asks players to type `/queroir`.
2. For `EventKillAllTimeToEnter` **seconds**, players type `/queroir` and are teleported to the GM. Characters at level 10 or lower can't join.
3. When the window closes, the fight starts if at least 2 players joined. Otherwise it is cancelled.
4. Every participant can attack every other participant. A kill gives the killer `EventKillAllKillIncrease` points. A death costs the victim `EventKillAllDeadDecrease` points, never below 0. Each change is shown to the player.
5. Dead participants respawn on a random free tile within about 20 tiles of the venue, so they stay in the fight.
6. When the total time runs out, the ranking is computed: 1st, 2nd and 3rd are announced to the server and rewarded. Every participant gets a private message with their position and score, and all participants are sent back to Lorencia.

Participants who leave the venue map or disconnect are dropped from the ranking.

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Event.dat`:

| Key | Default | Meaning |
|---|---|---|
| `EventKillAllSwitch` | `1` | `1` enables the event |
| `EventKillAllTimeToEnter` | `20` | Join window after `/matamata`, in **seconds** |
| `EventKillAllDeadDecrease` | `1` | Points lost per death (score never goes below 0) |
| `EventKillAllKillIncrease` | `2` | Points gained per kill |
| `EventKillAllMaxScore` | `20` | **Not used** by the current GameServer. The fight always lasts until time runs out |
| `EventKillAllMinPlayers` | `2` | **Not used**. The minimum is fixed at 2 participants |
| `EventKillAllMaxPlayers` | `20` | Maximum participants announced and accepted (see Pitfalls). Hard cap 100 |
| `EventKillAllMaxTime` | `2` | Total time, join window included, in **minutes** |
| `EventKillAllAutoReward1Rank1` | `0` | **WCoinC** for 1st place |
| `EventKillAllAutoReward1Rank2` | `0` | **WCoinC** for 2nd place |
| `EventKillAllAutoReward1Rank3` | `0` | **WCoinC** for 3rd place |
| `EventKillAllAutoReward2Rank1` | `0` | **WCoinP** for 1st place |
| `EventKillAllAutoReward2Rank2` | `0` | **WCoinP** for 2nd place |
| `EventKillAllAutoReward2Rank3` | `0` | **WCoinP** for 3rd place |
| `EventKillAllAutoReward3Rank1` | `0` | **GoblinPoint** for 1st place |
| `EventKillAllAutoReward3Rank2` | `0` | **GoblinPoint** for 2nd place |
| `EventKillAllAutoReward3Rank3` | `0` | **GoblinPoint** for 3rd place |

The key naming is `EventKillAllAutoReward<coin>Rank<place>`: coin `1` = WC, `2` = WP, `3` = GP.

## Files & syntax

No event file. The venue is the tile next to where the GM stood (same X, Y - 1) when typing `/matamata`, and respawns scatter around it.

## Example

```
EventKillAllSwitch = 1
EventKillAllTimeToEnter = 60        ; 60 s to join
EventKillAllDeadDecrease = 1
EventKillAllKillIncrease = 2
EventKillAllMaxPlayers = 30
EventKillAllMaxTime = 8             ; 8 min total -> 7 min of fighting
EventKillAllAutoReward1Rank1 = 30   ; 1st: 30 WC
EventKillAllAutoReward1Rank2 = 20   ; 2nd: 20 WC
EventKillAllAutoReward1Rank3 = 10   ; 3rd: 10 WC
EventKillAllAutoReward2Rank1 = 0
EventKillAllAutoReward2Rank2 = 0
EventKillAllAutoReward2Rank3 = 0
EventKillAllAutoReward3Rank1 = 0
EventKillAllAutoReward3Rank2 = 0
EventKillAllAutoReward3Rank3 = 0
```

## Rewards

At the end, ranks 1-3 by score receive their WC / WP / GP values, credited to the account. Ties are broken by join order (whoever joined first ranks higher). Only participants still on the venue map at the end are ranked. Shipped values are all `0`. No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/matamata` | Admin (`#GameMaster` 2) | Opens the event at the GM's position. Typed again while it runs, it cancels it (everyone is sent to Lorencia) |
| `/queroir` | Player (`#GameMaster` 0) | Joins during the join window and teleports to the venue |

## Pitfalls

* Shipped timing is very short: 2 minutes total minus 20 seconds to join leaves about 100 seconds of fighting.
* `TimeToEnter` is in **seconds**, `MaxTime` in **minutes**.
* The limit check lets **one extra player** in: with `MaxPlayers = 20`, a 21st player can still join.
* `/queroir` also joins an open [Run and Catch](run-and-catch.md) or [Russian Roulette](russian-roulette.md). Run only one of these games at a time.
* Pick a venue with open walkable ground around it. Respawn points are random tiles within about 20 tiles, and a venue in a narrow corridor can fail to find one.
* `/offpvp` doesn't protect participants.

## Reload module

`/reload event` re-reads the keys. A running Kill All isn't cancelled, and the score and reward values in effect at the end of the fight are the ones used.
