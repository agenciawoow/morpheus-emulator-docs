# Kanturu

Continuous boss cycle in Kanturu Relics: a standby phase, the Battle of Maya, the Battle of Nightmare, then the Tower of Refinement stays open until the next cycle.

## What it is

* Kanturu runs in a loop, with no clock schedule. Each phase has its own timers in `Kanturu.dat`.
* **Battle standby**: the gate NPC announces that the battle will begin. Players gather at the entrance.
* **Battle of Maya**: up to 15 players enter. They must wear a **Moonstone Pendant** (item 13,38) when `EnableCheckMoonStone = 1`. They fight Maya's left hand, then her right hand, then Maya with both hands, each scene with a time limit.
* **Battle of Nightmare**: if Maya falls, the party fights Nightmare.
* **Tower of Refinement**: if Nightmare falls, the tower opens for a long period (shipped about 23 hours). Players use it for refinery mixes. Then it closes and a new cycle starts.
* If the players fail a battle, the cycle goes back to standby.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `KanturuEvent` | `1` | `1` = the Kanturu cycle runs. `0` = the boss area stays closed |

The GameServerCS ships with `KanturuEvent = 0`. Run Kanturu on the server that hosts the Kanturu maps (37, 38, 39).

## Files & syntax

`Data/Event/Kanturu.dat`, sections 0 to 10. Most sections are **state tables** with the same three columns:

| Column | Meaning | Values |
|---|---|---|
| State | Sub-state number of that phase (fixed by the server, see each section) | number |
| Condition | `1` = this state ends after `Value` seconds. `0` = no timer: it ends when the game event happens (boss dies, all players die...) | `0` / `1` |
| Value | Duration in **seconds** when Condition is `1` | number |

### Section 0 - Main phases

| State | Phase | Shipped |
|---|---|---|
| 0 | None | `0 0` |
| 1 | Battle standby | `0 0` |
| 2 | Battle of Maya | `0 0` |
| 3 | Battle of Nightmare | `0 0` |
| 4 | Tower of Refinement | `0 0` |
| 5 | End (back to start) | `1 0` |

### Section 1 - Battle standby

| State | Meaning | Shipped |
|---|---|---|
| 0 | None | `0 0` |
| 1 | Standby: gate announces the battle | `1 200` (3 min 20 s) |
| 2 | Notify: last call before Maya | `1 60` |
| 3 | End | `0 0` |

### Section 2 - Maya scene time limits

| State | Meaning | Shipped |
|---|---|---|
| 0 | Time limit to kill Maya's first hand | `1 900` (15 min) |
| 1 | Time limit to kill the second hand | `1 900` |
| 2 | Time limit to kill Maya with both hands | `1 1200` (20 min) |

### Section 3 - Battle of Maya states

| State | Meaning | Shipped |
|---|---|---|
| 0 | None | `0 0` |
| 1 / 7 / 12 | Standby before scene 1 / 2 / 3 | `120` / `60` / `120` s |
| 2 | Notify before scene 1 | `15` s |
| 3, 4 / 8, 9 / 13, 14 | Start and fight of scene 1 / 2 / 3 (no timer: section 2 limits them) | `0 0` |
| 5 / 10 / 15 | End of scene 1 / 2 / 3 | `5` s each |
| 6 / 11 / 16 | End cycle of scene 1 / 2 / 3 | `3` / `3` / `10` s |
| 17 | End of the Maya battle | `3` s |
| 18 | End cycle | `0 0` |

### Section 4 - Maya hand AI change time

One value per row, 4 rows, in **seconds**: how long each hand AI stage lasts before switching (shipped `3`, `60`, `60`, `900`).

### Section 5 - Maya hand AI groups

One value per row: the `MonsterAIGroup.txt` group numbers used by the hands (shipped `20`, `21`, `22`).

### Section 6 - Ice Storm rate

One value: chance, out of 10000, that Maya casts Ice Storm each time the server checks during a scene (shipped `60` = 0.6% per check).

### Section 7 - Battle of Nightmare states

| State | Meaning | Shipped |
|---|---|---|
| 0 | None | `0 0` |
| 1 | Idle | `1 5` |
| 2 | Notify | `1 10` |
| 3 | Fight (time limit) | `1 1200` (20 min) |
| 4 | End | `1 5` |
| 5 | End cycle | `0 0` |

### Section 8 - Nightmare AI group

One value: the `MonsterAIGroup.txt` group of Nightmare (shipped `23`).

### Section 9 - Tower of Refinement states

| State | Meaning | Shipped |
|---|---|---|
| 0 | None | `0 0` |
| 1 | Tower open | `1 82500` (22 h 55 min) |
| 2 | Notify: tower closes soon | `1 300` |
| 3 | Tower closed | `1 2220` (37 min) |
| 4 | Notify: new battle soon | `1 180` |
| 5 | End | `1 5` |

### Section 10 - Moonstone check

One value: `1` = players need an equipped Moonstone Pendant with durability to enter the Maya battle, `0` = no check.

## Example

Shorten the tower to 12 hours and give 30 minutes for each hand:

```
2
//State   Condition   Value
0         1           1800      // first hand: 30 min
1         1           1800      // second hand: 30 min
2         1           1200      // Maya + both hands: 20 min
end

9
//State   Condition   Value
0         0           0
1         1           43200     // tower open 12 h
2         1           300       // 5 min warning
3         1           2220
4         1           180
5         1           5
end

10
//EnableCheckMoonStone
1
end
```

## Schedule

Kanturu has no clock schedule and is not listed in [Schedule syntax](schedule-syntax.md). The loop length is the sum of the timers above. With the shipped values a full cycle is about one day, most of it with the tower open.

## Rewards

Kanturu has no reward table. Rewards are monster drops, from these event item bags (matched by monster class in `Data/EventItemBagManager.txt`):

| Monster | Bag index | File |
|---|---|---|
| Maya (left hand), class 362 | 46 | `Data/EventItemBag/046 - Maya Hand Left.txt` |
| Maya (right hand), class 363 | 47 | `Data/EventItemBag/047 - Maya Hand Right.txt` |
| Nightmare, class 361 | 48 | `Data/EventItemBag/048 - Nightmare.txt` |

Monster spawns for the battles are in `Data/Monster/KanturuMonsterSetBase.txt`.

## Commands related

There is no command to start Kanturu.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | Re-reads `Kanturu.dat` |
| `/reload eventitembag` | Admin | Re-reads the boss item bags |

## Pitfalls

* All Value columns are **seconds**, not minutes.
* Do not change the State numbers, reorder or remove rows: the server stores rows in the order it reads them and looks states up by that position. Change only Condition and Value.
* Setting Condition `0` on a waiting state (for example the tower open state) makes the cycle wait forever.
* `EnableCheckMoonStone = 1` with no Moonstone Pendant available in your shops or drops makes the battle impossible to enter.
* The battle is limited to 15 players, fixed in the server.

## Reload module

`/reload event` re-reads `Kanturu.dat`. Monster spawns in `KanturuMonsterSetBase.txt` reload with `/reload monster`.

{% hint style="warning" %}
`Kanturu.dat` resets its phase timers when it is re-read. Reload while no Kanturu battle is running (during the tower phase), or restart the GameServer.
{% endhint %}
