# Castle Siege

Weekly guild war for Valley of Loren on the Castle Siege server: guilds register, collect marks, and fight for the crown.

## What it is

The siege runs in a fixed **cycle** of days. Each state of the cycle starts at a day/hour/minute offset from the cycle start:

| State | Name | What players do |
|---|---|---|
| 1 | Registration | Guild masters register their guild for the siege |
| 2 | Idle | Registration closed |
| 3 | Mark registration | Guilds hand in Signs of Lord. The guilds with the most marks become attackers |
| 4 | Idle | Marks closed |
| 5 | Notify | Attackers and defenders are announced |
| 6 | Ready | Preparation before the battle |
| 7 | Siege | The battle: attackers break the gates and statues and try to register the seal at the crown |
| 8 | End of siege | Result applied, the winner owns the castle |
| 9 | End of cycle | The cycle ends and a new one starts |

The castle owner collects taxes, manages gates and statues, and the owner guild master can use the Senior Mix.

## Enable it

Castle Siege runs **only on the GameServerCS**. Its keys are in `GameServerCS/DATA/GameServerInfo - Event.dat`.

| File | Key | Default (CS) | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `CastleSiegeEvent` | `1` | `1` = siege cycle runs. The main GameServer ships with `0` |
| `GameServerInfo - Event.dat` | `CastleSiegeCycleStartYear` / `Month` / `Day` | `0` | Forces a new cycle to start on this date. `0` = keep the date stored in the database |
| `GameServerInfo - Event.dat` | `CastleSiegeSpecificState` | `0` | Jump to this state (`1`-`9`, except `8`) at the date below. `0` = off |
| `GameServerInfo - Event.dat` | `CastleSiegeStateStartYear` / `Month` / `Day` / `Hour` / `Minute` | `0` | When the forced state starts. Until then, the server waits in the state before it |
| `GameServerInfo - Event.dat` | `CastleSiegeOffensiveWeaponDamage` | `300` | Damage of the attackers' slingshot (siege weapon) |
| `GameServerInfo - Event.dat` | `CastleSiegeDefensiveWeaponDamage` | `200` | Damage of the defenders' slingshot |
| `GameServerInfo - Event.dat` | `CastleSiegeLowerAccumulatedTimeValue` | `0` | Milliseconds removed from a player's crown registration time when his registration is interrupted |
| `GameServerInfo - Event.dat` | `CastleSiegeDecayAccumulatedTimeValue` | `0` | Milliseconds removed per check from that time while the player is away from the crown area during the siege |
| `GameServerInfo - Event.dat` | `CastleSiegeSeniorMixLimitType` | `1` | `0` = Senior Mix unlimited. `1` (or any positive value) = at most `CastleSiegeSeniorMixQuantityLimit` mixes per day. If the key is missing the server uses `-1` |
| `GameServerInfo - Event.dat` | `CastleSiegeSeniorMixQuantityLimit` | `5` | Daily Senior Mix limit when the type is positive |
| `GameServerInfo - Event.dat` | `CastleSiegeGetCrownUserGuildLevel` | `128` | Present in the CS file but **not read** by this build |

{% hint style="warning" %}
The GameServerCS reads the `CastleSiegeCycleStart*`, `CastleSiegeSpecificState` and `CastleSiegeStateStart*` keys once and then **writes them back to `0`** in your file. They are one-shot overrides: set them, start (or reload) the server, and they reset themselves. A date that is already in the past is rejected with an error box.
{% endhint %}

## Files & syntax

`Data/Event/MuCastleData.dat`. Sections 0 to 2 are shipped. Sections 3 to 9 are optional: when missing, the server uses its built-in upgrade tables.

### Section 0 - Cycle

| Column | Meaning | Values |
|---|---|---|
| Cycle | Length of a full siege cycle in days | number > 0, shipped `7` |

### Section 1 - State start times

| Column | Meaning | Values |
|---|---|---|
| State | State number (table above) | `1`-`9`, each once |
| StartDay | Days after the cycle start | `0` to Cycle |
| StartHour | Hour | `0`-`23` |
| StartMinute | Minute | `0`-`59` |

States must be in increasing time order. Shipped:

| State | Starts at | |
|---|---|---|
| 1 Registration | day 0, 00:00 | |
| 2 Idle | day 1, 23:59 | |
| 3 Marks | day 2, 00:00 | |
| 4 Idle | day 3, 00:00 | |
| 5 Notify | day 4, 12:00 | |
| 6 Ready | day 4, 19:00 | |
| 7 **Siege** | day 6, 20:00 | battle lasts until state 8 |
| 8 End of siege | day 6, 22:00 | |
| 9 End of cycle | day 7, 00:00 | |

### Section 2 - Castle NPCs

{% hint style="warning" %}
The comment header of the shipped file says `Monster Index Side DBSave`. The server actually reads **DBSave before Side**. Use the order below.
{% endhint %}

| Column | Meaning | Values |
|---|---|---|
| Monster | NPC class: `277` Castle Gate, `283` Guardian Statue, `288` Canon Tower, `215` Shield, `221` Slingshot (attack), `222` Slingshot (defense) | class |
| Index | Number of this NPC within its class | `1`... unique per class |
| DBSave | `1` = HP and upgrades are saved in the database (gates, statues). `0` = respawned fresh each siege | `0` / `1` |
| Side | `1` = defenders (castle owner), `2` = attackers | `1` / `2` |
| DFLevel | Starting defense upgrade level | `0`-`3` |
| RGLevel | Starting HP regeneration upgrade level | `0`-`3` |
| MaxLife | Starting max HP | number |
| X, Y | Position on map 30 | coordinates |
| TX, TY | Destination (unused for fixed NPCs) | `-1` |
| Dir | Facing direction | `0`-`7` |

### Sections 3 to 9 - Upgrade tables (optional)

| Section | Content | Columns | Built-in default |
|---|---|---|---|
| 3 | Gate defense upgrades | `Level JewelCount Zen` (3 rows) | 1/2, 2/3, 3/4 jewels, 3,000,000 Zen each |
| 4 | Statue defense upgrades | `Level JewelCount Zen` (3 rows) | 1/3, 2/5, 3/7 jewels, 3,000,000 Zen |
| 5 | Gate HP upgrades | `HP JewelCount Zen` | 2,500,000 / 3,500,000 / 5,200,000 HP |
| 6 | Statue HP upgrades | `HP JewelCount Zen` | 2,200,000 / 3,400,000 / 5,000,000 HP |
| 7 | Statue regeneration upgrades | `Level JewelCount Zen` | 1/3, 2/5, 3/7 jewels, 5,000,000 Zen |
| 8 | Gate defense value per level | one value per row (4 rows) | 100, 180, 300, 520 |
| 9 | Statue defense value per level | one value per row (4 rows) | 80, 180, 340, 550 |

## Example

Move the battle to day 5 of a 7-day cycle, 21:00 to 23:00:

```
0
//Cycle
7
end

1
//State StartDay StartHour StartMinute
1       0        0         0
2       1        23        59
3       2        0         0
4       3        0         0
5       4        12        0
6       4        19        0
7       5        21        0        // siege starts day 5 at 21:00
8       5        23        0        // ends 23:00
9       7        0         0
end
```

To force the next siege to happen on 2026-11-15 at 20:00, set in `GameServerInfo - Event.dat` of the GameServerCS:

```
CastleSiegeSpecificState   = 7
CastleSiegeStateStartYear  = 2026
CastleSiegeStateStartMonth = 11
CastleSiegeStateStartDay   = 15
CastleSiegeStateStartHour  = 20
CastleSiegeStateStartMinute= 0
```

## Schedule

Castle Siege does **not** use the shared format of [Schedule syntax](schedule-syntax.md). Its timing is the cycle of sections 0 and 1, counted from the cycle start date stored in the database (or forced by `CastleSiegeCycleStart*`).

## Rewards

There is no reward table in `MuCastleData.dat`. Owning the castle gives:

* Tax income and control of the castle NPCs.
* The **Senior Mix** for the owner guild master: item bag index 34, file `Data/EventItemBag/034 - Senior Mix.txt`, limited by `CastleSiegeSeniorMixLimitType` / `QuantityLimit`.

## Commands related

There is no command to start a siege. Use the one-shot `CastleSiegeSpecificState` keys above.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | On the GameServerCS, re-reads the siege data (see Reload module) |

## Pitfalls

* Columns 3 and 4 of section 2 are DBSave then Side, not the order of the file's own comment.
* Two NPCs with the same Monster + Index, two rows for the same state, or states out of time order make the file fail to load: the siege does not run.
* `CastleSiegeSpecificState = 8` is not allowed. A forced state that is not listed in section 1 is ignored with an error.
* The one-shot keys reset to `0` after being read, so editing them again requires another start or reload.
* Changing `Cycle` without moving the state rows can push states past the end of the cycle.

## Reload module

On the GameServerCS, `/reload event` resets the siege data, clears the castle NPCs and reloads `MuCastleData.dat` and the one-shot keys.

{% hint style="danger" %}
Never reload during states 6 to 8 (ready, siege, end of siege). The safest way to apply Castle Siege changes is to restart the GameServerCS outside the siege window.
{% endhint %}
