# Invasions

Scheduled monster invasions: bosses and their escorts spawn on chosen maps for a limited time, with global announcements.

## What it is

* At the scheduled time, a global notice announces the invasion (for example "The Red Dragon invasion has begun!") and the monsters appear on one or more maps.
* When a player kills the invasion boss, everybody sees who killed it. The boss drops its item bag.
* When the invasion time ends, the remaining invasion monsters disappear and an end notice is sent.
* The shipped file has 16 invasions: Skeleton King, Red Dragon, Golden monsters, White Wizard, New Year, Rabbits, Summer, Cursed Santa, Medusa, five Mini Bosses (Balrog, Ice Queen, Gorgon, Zaikan, Hydra), Selupan and Kundun.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `InvasionManagerSwitch` | `1` | `1` = every invasion follows its schedule. `0` = no invasion starts on its own |

The GameServerCS ships with `InvasionManagerSwitch = 0`.

## Files & syntax

`Data/Event/InvasionManager.dat`, 4 sections. Every row starts with **Index**, the invasion number (`0`-`49`). The same Index ties the 4 sections together, and must match section 1 of [`EventName.dat`](event-name.md).

### Section 0 - Schedule

| Column | Meaning | Values |
|---|---|---|
| Index | Invasion | `0`-`49` |
| Year Month Day DoW Hour Minute Second | Start time, see [Schedule syntax](schedule-syntax.md) | |

### Section 1 - Invasion settings

| Column | Meaning | Values |
|---|---|---|
| Index | Invasion | `0`-`49` |
| Name | Name used in the server logs | text in quotes |
| RespawnMessage | Message number shown to everybody when it starts | number from `Data/Lang/*_Message.txt`, `-1` = none |
| DespawnMessage | Message number shown when it ends | number, `-1` = none |
| BossIndex | Monster class of the boss | class, `-1` = no boss |
| BossMessage | Message shown when a player kills the boss (`%s` = player name) | number, `-1` = none |
| InvasionTime | How long the invasion lasts | **seconds**, shipped `900` (15 min) |

### Section 2 - Where it spawns

| Column | Meaning | Values |
|---|---|---|
| Index | Invasion | `0`-`49` |
| Group | Respawn group `0`-`19`. For each group the server picks **one** of its rows at random | `0`-`19` |
| Map | Map number | map |
| Value | Spot set to use: matches the `Value` column of the type-3 spots in `Data/MonsterSetBase/Invasion/*.txt` | number |

Several rows with the same Group make the invasion appear on a random map among them. Rows in different groups all spawn.

### Section 3 - Which monsters spawn

| Column | Meaning | Values |
|---|---|---|
| Index | Invasion | `0`-`49` |
| Group | Respawn group this monster belongs to (same numbers as section 2) | `0`-`19` |
| MonsterClass | Monster class | class |
| RegenType | `0` = killed monsters do not come back during the invasion. Other value = they respawn after RegenTime | `0` / `1` |
| RegenTime | Respawn delay when RegenType is not 0 | milliseconds |

The positions and quantities come from `Data/MonsterSetBase/Invasion/<map>.txt`, section `3` (columns `Monster MapNumber Range BeginPosX BeginPosY EndPosX EndPosY Direction Quantity Value`). A monster spawns only where a type-3 spot has the same class, map and Value.

## Example

A new invasion 16, "Golden Goblin", on Lorencia or Devias, twice a day:

```
0
//Index   Year   Month   Day   DoW   Hour   Minute   Second
16        *      *       *     *     12     15       0
16        *      *       *     *     22     15       0
end

1
//Index   Name              RespawnMessage  DespawnMessage  BossIndex  BossMessage  InvasionTime
16        "Golden Goblin"   194             -1              78         -1           600      // 10 min
end

2
//Index   Group   Map   Value
16        0       0     0                 // group 0: Lorencia...
16        0       2     0                 // ...or Devias (random)
end

3
//Index   Group   MonsterClass   RegenType   RegenTime
16        0       78             0           0
end
```

Then add type-3 spots for class 78 with Value `0` in `Data/MonsterSetBase/Invasion/000 - Lorencia.txt` and `002 - Devias.txt`, and add `16 "Golden Goblin"` to section 1 of `Data/Event/EventName.dat`.

## Schedule

Section 0 uses the shared format with a leading Index, see [Schedule syntax](schedule-syntax.md). Shipped: invasions 0-8 six times a day, Mini Bosses 9-13 four times a day, Selupan and Kundun three times a day.

```
0
//Index   Year   Month   Day   DoW   Hour   Minute   Second
0         *      *       *     *     0      40       0     // Skeleton King
0         *      *       *     *     4      40       0
1         *      *       *     *     0      50       0     // Red Dragon
end
```

## Rewards

There is no reward table. Invasion monsters drop from the item bags matched by their monster class in `Data/EventItemBagManager.txt`, for example:

| Monster | Bag file in `Data/EventItemBag/` |
|---|---|
| Skeleton King (55) / Death Bone (56) | `001 - Skeleton King.txt` / `137 - Death Bone.txt` |
| Red Dragon (42) | `002 - Red Dragon.txt` |
| White Wizard (135) and its ogres (136, 137) | `027 - White Wizard.txt`, `140 - ...`, `141 - ...` |
| Golden monsters (43, 53, 54, 78-83, 493-502) | `108 - Golden Budge Dragon.txt` ... `126 - Golden Great Dragon.txt` |
| Medusa (561) | `106 - Medusa.txt` |
| Balrog (38), Ice Queen (25), Gorgon (18), Zaikan (59), Hydra (49) | `135 - Boss Balrog.txt`, `133 - Boss Ice Queen.txt`, `134 - Boss Gorgon.txt`, `131 - Boss Zaikan.txt`, `132 - Boss hydra.txt` |
| Selupan (459), Kundun (275) | `067 - Selupan.txt`, `032 - Kundun.txt` |

## Commands related

| Command | Who | What |
|---|---|---|
| `/startinvasion <index>` | Admin (GameMaster 2) | Adds a one-off start of that invasion 2 minutes from now |
| `/reload event` | Admin | Re-reads the file and resets all schedules |

## Pitfalls

* `/startinvasion 0` is refused: the command only accepts indexes from `1`. Give Skeleton King another index if you need to start it by hand.
* `/startinvasion` works even with `InvasionManagerSwitch = 0`, and that invasion then keeps following its schedule until the next `/reload event`.
* The Index must be the same in all four sections **and** in section 1 of [`EventName.dat`](event-name.md). Otherwise the invasion runs but shows no (or the wrong) name in the event window and in the start notices.
* The client's `EventNameDisplay.txt` must also list the same indexes, see [Client custom files](../client/custom-files.md). The shipped client file lists only 0 to 9, and its index 9 is "Medusa2", while the server's 9 is "Mini Boss Balrog".
* Messages 5035 to 5070 in the shipped `Data/Lang/000 - Eng_Message.txt` are in Portuguese. Translate them in your language file.
* Shipped message numbers look mixed up for the last bosses: Zaikan uses 5032 (Nightmare's text), Hydra 5034 (Kundun's), Selupan's boss message 5036 (Balrog's), Kundun 5037/5038 (Ice Queen's). The intended texts are 5042, 5044, 5046, 5047 and 5048.
* InvasionTime is in seconds, RegenTime in milliseconds.
* A monster with no matching type-3 spot (same class, map and Value) in `Data/MonsterSetBase/Invasion` never appears.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat`, `InvasionManager.dat` and `EventName.dat`, removes the invasion monsters and recalculates every schedule. Spot changes in `Data/MonsterSetBase` need `/reload monster`. Bags need `/reload eventitembag`.

{% hint style="warning" %}
A running invasion is cut short by `/reload event`. Reload between invasions.
{% endhint %}
