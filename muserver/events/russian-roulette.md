# Russian Roulette

A luck game run by a GM: participants gather, the GM "spins the cylinder" on one player at a time, and the last survivor wins.

## What it is

1. The GM stands at the venue and types `/roletarussa`. The server announces "[RussianRoulette] Opened (Max Participants: N)!" and asks players to type `/queroir`.
2. For `EventRussianRouletteTimeToEnter` **seconds**, players type `/queroir` and are teleported next to the GM. Each one is announced with a number. Characters at level 10 or lower can't join.
3. When the window closes, the game starts if at least 2 players joined. Otherwise it is cancelled.
4. The GM picks a victim by **sending them a trade request** (no trade window opens). An 8-second sequence follows: "Loading the bullet…", "Spinning the chamber…", "Cocking the gun…", "FIRE!".
5. The shot hits about **1 time in 4** (26%). On a hit, "HeadShot *name* was eliminated!" and the character drops dead. On a miss, "*name* escaped unscathed!" and the player stays in.
6. When only one participant is left, that player is the **winner**: fireworks, announcement, coin reward.
7. If the total time runs out first, the game ends with no winner.

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Event.dat`:

| Key | Default | Meaning |
|---|---|---|
| `EventRussianRouletteSwitch` | `1` | `1` enables the event |
| `EventRussianRouletteTimeToEnter` | `10` | Join window, in **seconds**. Server default if missing: `30` |
| `EventRussianRouletteMaxTime` | `10` | Total game time, join window included, in **minutes** |
| `EventRussianRouletteMaxPlayer` | `8` | Maximum participants announced and accepted (see Pitfalls) |
| `EventRussianRouletteAutoReward1` | `0` | **WCoinC** for the winner |
| `EventRussianRouletteAutoReward2` | `0` | **WCoinP** for the winner |
| `EventRussianRouletteAutoReward3` | `0` | **GoblinPoint** for the winner |

## Files & syntax

No event file. The venue is the tile next to where the GM stood (same X, Y - 1) when typing `/roletarussa`.

## Example

```
EventRussianRouletteSwitch = 1
EventRussianRouletteTimeToEnter = 45   ; 45 seconds to join
EventRussianRouletteMaxTime = 15       ; whole game lasts 15 minutes
EventRussianRouletteMaxPlayer = 6
EventRussianRouletteAutoReward1 = 0
EventRussianRouletteAutoReward2 = 10   ; 10 WCoinP to the survivor
EventRussianRouletteAutoReward3 = 0
```

## Rewards

The survivor gets the three `AutoReward` values as WCoinC / WCoinP / GoblinPoint, credited to the account. Shipped values are all `0`. No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/roletarussa` | Admin (`#GameMaster` 2) | Opens the game at the GM's position. Typed again while it runs, it cancels it |
| `/queroir` | Player (`#GameMaster` 0) | Joins during the join window and teleports to the GM |

## Pitfalls

* `TimeToEnter` is in **seconds**, `MaxTime` in **minutes**.
* The limit check lets **one extra player** in: with `MaxPlayer = 8`, a 9th player can still join.
* `/queroir` also joins an open [Run and Catch](run-and-catch.md) or [Kill All](kill-all.md). Run only one of these games at a time.
* On a hit the character falls dead and respawns at the map's normal respawn point, away from the venue.
* The game only advances when the GM sends trade requests. If the GM stops, the game just runs out of time with no winner.
* Wait for the 8-second sequence to finish before sending the next trade request, or the new request replaces the current victim.

## Reload module

`/reload event` re-reads the keys. A running game isn't cancelled, and rewards are read at the moment of the win.
