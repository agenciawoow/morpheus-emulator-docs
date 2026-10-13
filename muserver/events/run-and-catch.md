# Run and Catch

A tag game led by a GM: players join at the GM's position, the GM hunts them down, and the last player left wins.

## What it is

Players know it as **[TAG]** (from the Portuguese *pega-pega*).

1. The GM stands where the game should happen and types `/pegapega`. The whole server sees "[TAG] Opened!" and an invitation to join.
2. For `EventRunAndCatchTimeToEnter` **seconds**, players type `/queroir` and are teleported next to the GM. Characters at level 10 or lower can't join.
3. When the join window closes, "[TAG] Event started" is announced and the chase begins.
4. Each time the GM **attacks** a participant, that player is caught: announced to everyone and sent back to Lorencia.
5. When only one participant is left, the GM's next hit on that player ends the game, and that player is the **winner** (fireworks, global announcement, coin reward).
6. If the total time runs out first, the event ends with no winner.

While a player is in the game, `/offpvp` stops protecting them.

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Event.dat`:

| Key | Default | Meaning |
|---|---|---|
| `EventRunAndCatchSwitch` | `1` | `1` enables the event |
| `EventRunAndCatchTimeToEnter` | `10` | Join window after `/pegapega`, in **seconds**. Server default if the key is missing: `30` |
| `EventRunAndCatchMaxTime` | `10` | Total event time, join window included, in **minutes** |
| `EventRunAndCatchAutoReward1` | `0` | **WCoinC** for the winner |
| `EventRunAndCatchAutoReward2` | `0` | **WCoinP** for the winner |
| `EventRunAndCatchAutoReward3` | `0` | **GoblinPoint** for the winner |

## Files & syntax

No event file. The keys above and two commands are all there is. The meeting point is the tile next to where the GM stood (same X, Y - 1) when typing `/pegapega`.

## Example

```
EventRunAndCatchSwitch = 1
EventRunAndCatchTimeToEnter = 60     ; players have 60 seconds to type /queroir
EventRunAndCatchMaxTime = 10         ; whole game, including those 60 s, lasts 10 minutes
EventRunAndCatchAutoReward1 = 15     ; 15 WCoinC to the winner
EventRunAndCatchAutoReward2 = 0
EventRunAndCatchAutoReward3 = 0
```

## Rewards

The winner gets the three `AutoReward` values as WCoinC / WCoinP / GoblinPoint, credited to the account. Shipped values are all `0`. No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/pegapega` | Admin (`#GameMaster` 2) | Opens the game at the GM's position. Typed again while it runs, it cancels it |
| `/queroir` | Player (`#GameMaster` 0) | Joins during the join window and teleports to the GM. Characters at level 10 or below can't join |

## Pitfalls

* `TimeToEnter` is in **seconds** and `MaxTime` is in **minutes**. The shipped 10 seconds is a very short window, so announce the event before you type the command.
* `/queroir` also joins an open [Russian Roulette](russian-roulette.md) or [Kill All](kill-all.md). Don't open two of these games at once, or players end up in all of them.
* There is no player limit and no minimum. With nobody joined, the game simply runs until time is up.
* Only hits from the GM who started the game count. Other players attacking each other do nothing.
* The winner is only decided when the GM catches the last player. If time runs out before that, nobody is rewarded.

## Reload module

`/reload event` re-reads the keys. A running game isn't cancelled. Rewards are read at the moment of the win.
