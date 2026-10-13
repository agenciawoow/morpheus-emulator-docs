# PvP 1x1

A GM-refereed duel: the GM picks two players, they fight where they stand, and the first to reach the target score wins.

## What it is

1. The GM types `/pvp`. Only the GM sees "[PvP] Started!", because this event is set up by hand.
2. The GM **sends a trade request** to the first fighter, then to the second (no trade window opens). The order sets who is fighter 1 and who is fighter 2.
3. As soon as both are picked, the server announces "[PvP] *A* x *B* started!", both characters charge at each other, and each sees the other with a red (outlaw) name.
4. Each kill gives the killer 1 point, with the score announced to the whole server. The dead fighter respawns **on the spot**, with no PK penalty for either side.
5. The first to reach `EventPvPMaxScore` points wins: fireworks, announcement, coin reward.
6. If one fighter disconnects, the other wins at once. If the time runs out, the duel ends with no winner.

There is no teleport: the duel happens wherever the two players are.

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Event.dat`:

| Key | Default | Meaning |
|---|---|---|
| `EventPvPSwitch` | `1` | `1` enables the event |
| `EventPvPMaxScore` | `3` | Kills needed to win. Server default if missing: `3` |
| `EventPvPMaxTime` | `3` | Total time from `/pvp`, including the time the GM takes to pick the fighters, in **minutes** |
| `EventPvPAutoReward1` | `0` | **WCoinC** for the winner |
| `EventPvPAutoReward2` | `0` | **WCoinP** for the winner |
| `EventPvPAutoReward3` | `0` | **GoblinPoint** for the winner |

## Files & syntax

No event file. The keys above and the `/pvp` command (no arguments) are all there is.

## Example

```
EventPvPSwitch = 1
EventPvPMaxScore = 5        ; first to 5 kills
EventPvPMaxTime = 10        ; 10 minutes from /pvp
EventPvPAutoReward1 = 25    ; 25 WCoinC to the winner
EventPvPAutoReward2 = 0
EventPvPAutoReward3 = 0
```

In game: `/pvp`, then send a trade request to player A, then to player B.

## Rewards

The winner gets the three `AutoReward` values as WCoinC / WCoinP / GoblinPoint, credited to the account. Shipped values are all `0`. A win because the opponent disconnected also pays. No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/pvp` | Admin (`#GameMaster` 2) | Starts the event (the GM then picks fighters by trade request). Typed again while it runs, it cancels it |

## Pitfalls

* The 3-minute default includes the time the GM needs to pick both fighters. Raise `EventPvPMaxTime` if your fights take longer.
* There is no arena or teleport. Bring both fighters to a quiet, non-safe-zone spot before you pick them.
* `/offpvp` doesn't protect a player who has been picked as a fighter.
* A fighter who disconnects gives the win to the other one, so a player can lose by closing the game.

## Reload module

`/reload event` re-reads the keys. A running duel isn't cancelled, and the new `EventPvPMaxScore` and rewards apply from the next kill.
