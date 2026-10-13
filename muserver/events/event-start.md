# Event Start

A gathering tool for manual events: the GM opens a short window, and players type `/participar` to be teleported to the GM.

## What it is

Event Start doesn't run a game by itself. It is the "everyone come here" step for any event a GM runs by hand (a boss hunt, a quiz on stage, a PvP show).

1. The GM stands at the meeting point and types `/abrirevento`. The server announces "[EVENT] *GM name* started an event!" and tells players how to join.
2. For `EventStartTime` **seconds**, any player who types `/participar` is announced ("Participant *name* added!") and teleported to the tile next to where the GM stood (same X, Y - 1).
3. The last 10 seconds are counted down. Then "Time to participate is over!" and `/participar` stops working.

There are no rewards, level checks or player limits. What happens after the gathering is up to the GM.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Event.dat` | `EventStartSwitch` | `1` | `1` enables `/abrirevento` and `/participar` |
| `GameServer/Data/GameServerInfo - Event.dat` | `EventStartTime` | `15` | How long players can join, in **seconds** |

## Files & syntax

No event file. The meeting point is taken from the GM's position when `/abrirevento` is typed.

## Example

```
EventStartSwitch = 1
EventStartTime   = 60     ; players have 60 seconds to type /participar
```

In game:

```
/abrirevento     -> GM opens the gathering at their current position
/participar      -> player is teleported to the GM (while the window is open)
```

## Rewards

None. Reward participants with `/premiar`, `/premiarall` or the coins of the game you run afterwards.

## Commands related

| Command | Who | What |
|---|---|---|
| `/abrirevento` | Admin (`#GameMaster` 2) | Opens the join window at the GM's position. Typed again while open, it cancels it |
| `/participar` | Player (`#GameMaster` 0) | Teleports to the GM while the window is open |

## Pitfalls

* `EventStartTime` is in **seconds**: the shipped 15 seconds is very short. Use 60 or more for a real event.
* **Don't cancel with a second `/abrirevento`.** Cancelling stops the countdown but leaves `/participar` working, and from then on it teleports players to Lorencia 0/0 (the map corner). To recover, type `/abrirevento` again and let the window run out normally.
* The English notice says "Type /participate to enter!", but the real command is `/participar` (the name in `Data/Command.txt`). Edit line 674 of `Data/Lang/000 - Eng_Message.txt` (and the other language files) or rename the command so both match.
* There is no level or map check: anyone, including brand-new characters, can be pulled to the meeting point.

## Reload module

`/reload event` re-reads both keys. An open window isn't cancelled, and the new `EventStartTime` applies to the next `/abrirevento`.
