# Quickly

A speed race started by a GM: an NPC appears at announced coordinates, and the first player to reach it and click it wins coins.

## What it is

Players know it as the **[SPEED]** event.

1. A GM types `/veloz`. The whole server sees `[SPEED] Started in Lorencia!` and `[SPEED] Run to the coordinates: (x:…, y:…)`.
2. A Santa Claus NPC (monster `465`) stands at that spot.
3. The first player to click the NPC wins: fireworks, a global "Winner" notice, and the coin reward. The NPC disappears.
4. Every minute the server announces how long is left, and it counts down the last 5 seconds. If nobody reaches the NPC in time, the event ends with "No winner!".

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Event.dat`:

| Key | Default | Meaning |
|---|---|---|
| `QuicklyEvent` | `1` | `1` enables the event. With `0`, `/veloz` does nothing |
| `QuicklyEventAutoReward1` | `10` | **WCoinC** given to the winner |
| `QuicklyEventAutoReward2` | `3` | **WCoinP** given to the winner |
| `QuicklyEventAutoReward3` | `0` | **GoblinPoint** given to the winner |

## Files & syntax

There is no event file. Everything is in the four keys above plus the arguments of `/veloz`:

```
/veloz [map] [x] [y] [minutes]
```

| Argument | Meaning | Values |
|---|---|---|
| map | Map where the NPC appears | map number. Only `0` (Lorencia), `2` (Devias) and `3` (Noria) are announced by name. Any other map is announced as "Secreto" (secret) |
| x | NPC X | must be greater than 0 |
| y | NPC Y | must be greater than 0 |
| minutes | Time limit | `1`-`5`. Anything else, or nothing, means `2` |

If you leave out the coordinates (or give `x`/`y` of `0`), the server picks a random free spot in **Lorencia** (about half the time), **Devias** or **Noria**.

## Example

```
/veloz                  -> random spot in Lorencia, Devias or Noria, 2 minutes
/veloz 0 180 120 3      -> Lorencia 180/120, 3 minutes
/veloz 2 220 70 5       -> Devias 220/70, 5 minutes
/veloz                  -> typed again while running: cancels the event
```

In `GameServerInfo - Event.dat`:

```
QuicklyEvent = 1
QuicklyEventAutoReward1 = 10   ; 10 WCoinC
QuicklyEventAutoReward2 = 3    ; 3 WCoinP
QuicklyEventAutoReward3 = 0    ; no GoblinPoint
```

## Rewards

The winner gets the three `QuicklyEventAutoReward` values as **WCoinC / WCoinP / GoblinPoint**, credited to the account with a "N WC added to your account!" notice. If all three are `0`, the winner only gets fireworks and the announcement. No item bag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/veloz [map] [x] [y] [minutes]` | Admin (`#GameMaster` 2 in `Data/Command.txt`) | Starts the event. Typed while it runs, it cancels it |

## Pitfalls

* While a Quickly event is running, a click on **any** Santa Claus NPC (class 465) wins it, including the permanent Santa Claus in Santa's Village. Keep players away from Santa's Village or remove that NPC while you use this event.
* The time limit is capped at 5 minutes. Larger values fall back to 2.
* The coordinates are always announced, so with a small map the race is mostly about who is closest.
* "Secreto" in the announcement for maps other than 0/2/3 is fixed text in the server. The rest of the messages are lines 660-666 of `Data/Lang/*_Message.txt`.

## Reload module

`/reload event` re-reads the four keys. Rewards are read when someone wins, so a new value applies to the next winner even during an event. A running Quickly event is **not** cancelled by `/reload event`.

{% hint style="warning" %}
`/reload monster` (or `/reload all`) deletes every NPC, including the Quickly NPC, and ends a running Quickly event without a winner. Use it between events.
{% endhint %}
