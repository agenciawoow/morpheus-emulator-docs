# Hide and Seek

A GM hides somewhere in the world. The first player to find the GM and send them a trade request wins.

## What it is

Players know it as **[ESCESC]** (from the Portuguese *esconde-esconde*).

1. The GM goes to a hiding place and types `/escesc`. The whole server sees "[ESCESC] *GM name* started the event!" and "Find the GM and trade!".
2. Players search the maps. A notice gives the minutes left, and the last 10 seconds are counted down.
3. The **first player who sends a trade request to the GM** wins: fireworks, a global announcement, and the coin reward. The trade window doesn't open. The request only counts as the "find".
4. If time runs out, the event ends with "Ended without winner!".

There is no registration and no teleport: anyone online can play.

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Event.dat`:

| Key | Default | Meaning |
|---|---|---|
| `EventHideAndSeekSwitch` | `1` | `1` enables the event. With `0`, `/escesc` does nothing |
| `EventHideAndSeekMaxTime` | `5` | Time players have to find the GM, in **minutes** |
| `EventHideAndSeekAutoReward1` | `0` | **WCoinC** for the winner |
| `EventHideAndSeekAutoReward2` | `0` | **WCoinP** for the winner |
| `EventHideAndSeekAutoReward3` | `0` | **GoblinPoint** for the winner |

## Files & syntax

No event file. The event is controlled entirely by the keys above and the `/escesc` command, which takes no arguments.

## Example

```
EventHideAndSeekSwitch = 1
EventHideAndSeekMaxTime = 10        ; 10 minutes to find the GM
EventHideAndSeekAutoReward1 = 20    ; 20 WCoinC
EventHideAndSeekAutoReward2 = 0
EventHideAndSeekAutoReward3 = 0
```

In game:

```
/escesc     -> starts the event (the GM who typed it is the one to find)
/escesc     -> typed again while running: cancels it
```

## Rewards

The winner gets the three `AutoReward` values as WCoinC / WCoinP / GoblinPoint, credited to the account. With all three at `0` (the shipped value) there is no prize beyond fireworks and the announcement. No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/escesc` | Admin (`#GameMaster` 2 in `Data/Command.txt`) | Starts the event with the typing GM as the hider. Typed again, it cancels the event |

## Pitfalls

* The shipped reward values are written as `0;` (with a semicolon). The server reads the number before the `;`, but keep the values plain numbers to avoid confusion.
* The GM must stay online and reachable. If the GM logs out, nobody can win and the event just times out.
* If `PKDisableTrade = 1` in `GameServerInfo - Common.dat` (shipped `0`), PK players can't send the trade request, so they can't win.
* `/escesc` is also used to cancel. A second GM typing it during the event cancels the first GM's event instead of starting a new one.

## Reload module

`/reload event` re-reads the five keys. Rewards are read at the moment someone wins, and a running Hide and Seek is not cancelled by the reload.
