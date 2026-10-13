# Online Lottery

A scheduled draw among the players who are online on a given map: the winner gets an item from the Lottery item bag and optional coins.

## What it is

At the scheduled time the server announces the countdown ("Lorencia Lottery Online in 5 minute(s)"), then picks one random character among everyone online on the draw map. The winner gets fireworks, an item dropped at their feet from the Lottery item bag and, if configured, WCoinC/WCoinP/GoblinPoint. The whole server sees the winner's name. If nobody is on the map, a "no winner" notice is sent. The next draw time also appears in the Event Time window (H key).

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomOnlineLotterySwitch` | `1` | `1` = draws run on schedule. `0` = disabled. |
| same | `CustomOnlineLotteryText1` | `%s em %d minuto(s)` | Countdown notice. `%s` = lottery name, `%d` = minutes left. |
| same | `CustomOnlineLotteryText2` | `%s foi o vencedor do sorteio. Parabéns!` | Winner notice. `%s` = winner's character name. |
| same | `CustomOnlineLotteryText3` | `%s não teve vencedor` | No-winner notice. `%s` = lottery name. |
| same | `CustomOnlineLotteryNoOff` | `1` | **Not read by the current GameServer** (see Pitfalls). |

The shipped texts are in Portuguese; translate them, for example `%s in %d minute(s)`, `%s won the lottery. Congratulations!`, `%s had no winner`.

## Files & syntax

File: `Data/Custom/CustomOnlineLottery.txt`. Three sections, each closed by `end`. Up to 24 lotteries (index `0..23`).

### Section 0 - Schedule

| Column | Meaning | Values |
|---|---|---|
| Index | Lottery index | `0..23` |
| Year, Month, Day | Date | `*` = any |
| DoW | Day of week | `*` = any, `1` = Sunday ... `7` = Saturday |
| Hour, Minute, Second | Time | |

### Section 1 - Lottery rules

| Column | Meaning | Values |
|---|---|---|
| Index | Lottery index | `0..23` |
| Name | Name used in the notices, in quotes | `"Lorencia Lottery Online"` |
| DropMap | Map whose online players take part | map number, `*` = every map |
| AlarmTime | Minutes of countdown notices before the draw | `0` = no countdown |

### Section 2 - Coin reward

| Column | Meaning |
|---|---|
| Index | Lottery index |
| WC | WCoinC given to the winner |
| WP | WCoinP given |
| GP | GoblinPoint given |

## Example

```
0
//Index Year Month Day DoW Hour Minute Second
0       *    *     *   *   20   0      0      // Lorencia lottery every day at 20:00
1       *    *     *   1   21   0      0      // Noria lottery on Sundays at 21:00
end

1
//Index Name                       DropMap AlarmTime
0       "Lorencia Lottery Online"  0       5
1       "Noria Lottery Online"     3       10
end

2
//Index WC WP GP
0       0  0  500
1       50 0  0
end
```

## Schedule

Section 0 uses the shared schedule format; see [schedule syntax](../events/schedule-syntax.md). Each lottery index has its own list of times; the server picks the nearest future match. The draw happens immediately when the time is reached (there is no "running" phase).

As shipped: three lotteries (Lorencia, Noria, Devias), six draws per day each, at minute 52 of different hours.

## Rewards

- **Item**: one item from `Data/EventItemBag/127 - Lottery.txt`, dropped at the winner's position (mapped through `Data/EventItemBagManager.txt`, special value 60). Edit that bag to change the prizes.
- **Coins**: Section 2 values for that lottery index, added to the winner's account.

## Commands related

| Command | Who | What |
|---|---|---|
| (none) | | There is no chat command. The GameServer window menu has "Start Online Lottery" entries (Lorencia, Devias, Noria, Elbeland) that draw immediately on that map, always using the coin reward of index `0`. |

## Pitfalls

- `CustomOnlineLotteryNoOff` is ignored: characters in offline store, offline attack or offline MU Helper **can** win. If you want only active players to win, choose a DropMap where offline systems are not allowed (MapManager `CustomAttack`/`CustomStore`/`HelperEnable` columns).
- The winner must have room around them: the prize is dropped on the ground, not placed in the inventory.
- Lottery indexes in Section 0 without a matching Section 1 row draw on map 0 with an empty name.
- Lang/Text strings with special characters may show broken accents in the shipped file; retype them in your editor's encoding.

## Reload module

- `/reload custom` re-reads `CustomOnlineLottery.txt` and the four keys in `GameServerInfo - Custom.dat`, but does **not** recompute the next draw time and does not apply an on/off change of the switch.
- `/reload event` runs the lottery initialisation: it applies the switch and recalculates the next draw from the schedule.

Recommended order after editing: `/reload custom`, then `/reload event`, between draws (`/reload event` also re-initialises every other scheduled event).
