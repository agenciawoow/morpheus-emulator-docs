# Castle Deep

Scheduled invasion of Valley of Loren (map 30): waves of monsters, ending with the boss Erohim, attack the castle area for a fixed time.

## What it is

* 5 minutes before the start, a global notice warns all servers of the group. At the start, a second notice announces the attack.
* The server picks one **scenario** at random. That scenario sends **waves** of monsters at set minutes into the event.
* When Erohim is killed, the server announces who killed him. When the time ends, the remaining monsters disappear and an end notice is sent.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `CastleDeepEvent` | `0` | `1` = the event runs on its schedule |
| `GameServerInfo - Event.dat` | `CastleDeepEventTime` | `30` | Duration of the event in **minutes** |

Monsters spawn on map 30 (Valley of Loren). Enable the event on the GameServer that hosts map 30, usually the GameServerCS.

## Files & syntax

`Data/Event/CastleDeepEvent.dat`, 4 sections.

### Section 0 - Schedule

`Year Month Day DoW Hour Minute Second`, see [Schedule syntax](schedule-syntax.md). Shipped rows (04:00, 12:00, 20:00) are commented out.

### Section 1 - Scenarios

| Column | Meaning | Values |
|---|---|---|
| Group | Scenario number, referenced as `Index` in sections 2 and 3 | number > 0 |
| SpawnRate | Chance in % to pick this scenario. The server tries the rows in order until one passes | `1`-`100` |

### Section 2 - Wave timing

| Column | Meaning | Values |
|---|---|---|
| Index | Scenario (Group of section 1) | number |
| Group | Wave number, starting at `1` and consecutive | `1`, `2`, `3`... |
| SpawnMinute | Minutes after the start when this wave spawns | `0` to `CastleDeepEventTime` - 1 |

### Section 3 - Wave monsters

| Column | Meaning | Values |
|---|---|---|
| Index | Scenario | number |
| Group | Wave number | number |
| Monster | Monster class (`Data/Monster/Monster.txt`) | class |
| Count | How many to spawn | number |
| X, Y | Top-left corner of the spawn area on map 30 | coordinates |
| TX, TY | Bottom-right corner of the spawn area | coordinates |

## Example

```
0
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     20     0        0            // every day 20:00
end

1
//Group SpawnRate
1       50                                               // scenario 1: 50%
2       100                                              // otherwise scenario 2
end

2
//Index Group SpawnMinute
1       1     0                                          // scenario 1, wave 1 at minute 0
1       2     10                                         // wave 2 at minute 10
1       3     20                                         // wave 3 at minute 20
end

3
//Index Group Monster   Count   X      Y      TX      TY
1       1     39        15      60     28     132     73   // wave 1: 15 monsters of class 39
1       3     295       1       60     58     132     101  // wave 3: Erohim
end
```

## Schedule

Section 0 uses the shared format, see [Schedule syntax](schedule-syntax.md). The shipped rows are commented, so the event never starts until you uncomment them.

```
0
//Year   Month   Day   DoW   Hour   Minute   Second
//*        *       *     *     4      0        0
//*        *       *     *     12     0        0
//*        *       *     *     20     0        0
end
```

## Rewards

There is no reward table. Rewards are monster drops. Erohim (class 295) uses bag index 33 in `Data/EventItemBagManager.txt`, file `Data/EventItemBag/033 - Erohim.txt`.

## Commands related

There is no command to start Castle Deep.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | Re-reads this file and the keys, and recalculates the next start |

## Pitfalls

* Waves are triggered in order. If wave numbers skip (1, 2, 4), wave 4 never spawns.
* A SpawnMinute equal to or above `CastleDeepEventTime` never spawns.
* SpawnRate values are tried in order with a new roll each time: a first row at 100% is always picked. Put the rare scenarios first.
* Enabling the event on a GameServer that does not host map 30 spawns nothing.

{% hint style="danger" %}
Section 1 must have at least one scenario with a SpawnRate above `0`. The server keeps rolling until a scenario is picked, so an empty section 1, or every rate at `0`, freezes the GameServer when the event starts.
{% endhint %}

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `CastleDeepEvent.dat` and recalculates the next start.

{% hint style="warning" %}
`/reload event` puts Castle Deep back to idle and removes the monsters it spawned. A running Castle Deep is cut short. Reload between events.
{% endhint %}
