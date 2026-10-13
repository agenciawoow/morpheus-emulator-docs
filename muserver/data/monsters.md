# Monsters and spawns

| File / folder | What it controls |
|---|---|
| `Monster/Monster.txt` | Every monster and NPC: stats, speed, respawn, drop rates, resistances |
| `MonsterSetBase/NNN - Map.txt` | Where NPCs and monsters spawn on each map |
| `MonsterSetBase/Event/` | Spawns used by events (Blood Castle, Devil Square, Chaos Castle, Illusion Temple, Kalima, Double Goer, Imperial Guardian...) |
| `MonsterSetBase/Invasion/` | Spawn areas used by invasions |
| `Monster/KanturuMonsterSetBase.txt` | Spawns of the Kanturu boss battle stages |
| `Monster/MovePath/MovePath_35.dat` | Waypoints for Crywolf monster AI |

Monster AI and monster skills are on [Monster AI and skills](monster-ai.md).

---

## Monster.txt

### What it does

Defines every monster and NPC class: level, life, damage, defense, speed, how often it respawns, how likely it is to drop items and Zen, and elemental resistances. Spawn files and events refer to a monster by its `Index`.

### Where it lives

`MuServer/Data/Monster/Monster.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0–1023 | Monster class id. Used everywhere (spawns, bags, ItemDrop, commands). |
| `Rate` | number | Not used by the server; keep `1`. |
| `Name` | `"text"` | Name (quoted). |
| `Level` | number | Monster level. Drives the normal drop table and experience. |
| `MaxLife` | number | Life (HP). |
| `MaxMana` | number | Mana. |
| `DamageMin` / `DamageMax` | number | Attack damage. |
| `Defense` | number | Physical defense. |
| `MagicDefense` | number | Magic defense. |
| `AttackRate` | number | Attack success rate. |
| `DefenseRate` | number | Defense success rate (block). |
| `MoveRange` | tiles | How far it wanders when idle. |
| `AttackType` | code | Attack type (melee/ranged/magic animation). Copy from a similar monster. |
| `AttackRange` | tiles | Distance from which it attacks. |
| `ViewRange` | tiles | Distance at which it notices players. |
| `MoveSpeed` | ms | Time per step (lower = faster). |
| `AttackSpeed` | ms | Time between attacks (lower = faster). |
| `RegenTime` | seconds | Respawn time after death. |
| `Attribute` | code | Internal behaviour flag used by some events. Keep as shipped. |
| `ItemRate` | number | Item drop divisor. The normal item chance is roughly `ItemDropRate / ItemRate` (see [Item drop](item-drop.md#how-a-monster-drop-is-decided)). Lower = drops more. `0` is treated as 1. |
| `MoneyRate` | number | Zen drop divisor: Zen drops with a chance of `10 / MoneyRate` when no item drops. |
| `MaxItemLevel` | number | Kept for compatibility; not used by the drop logic in this version. |
| `MonsterSkill` | number | Legacy field. Special skills come from `MonsterSkill.txt`. |
| `Resistance1`–`Resistance4` | 0–255 | Ice, Poison, Lightning and Fire resistance. |

### Example (real lines)

```
//Index  Rate  Name             Level  MaxLife  MaxMana  DamageMin  DamageMax  Defense  MagicDefense  AttackRate  DefenseRate  MoveRange  AttackType  AttackRange  ViewRange  MoveSpeed  AttackSpeed  RegenTime  Attribute  ItemRate  MoneyRate  MaxItemLevel  MonsterSkill  Resistance1  Resistance2  Resistance3  Resistance4
0        1     "Bull Fighter"   6      100      0        16         20         6        0             28          6            3          0           1            5          400        1600         03         2          130       20         6             0             0            0            0            0
```

Bull Fighter: level 6, 100 HP, respawns in 3 seconds, item divisor 130, Zen divisor 20.

### Rules & pitfalls

- The client has its own monster list (names, models). A new `Index` needs client support to show correctly.
- Very low `RegenTime` on bosses floods the map.
- Changing `Level` changes which items the monster drops (normal drop table) and the experience it gives.

---

## MonsterSetBase (spawns)

### What it does

Places NPCs and monsters on the maps. Each map has its own file.

### Where it lives

```
MuServer/Data/MonsterSetBase/
├── 000 - Lorencia.txt
├── 002 - Devias.txt
├── ...
├── Event/      009 - Devil Square 1.txt, 011 - Blood Castle 1.txt, ...
└── Invasion/   000 - Lorencia.txt, 002 - Devias.txt, ...
```

The server reads **every** file named `NNN - Name.txt` in this folder **and its subfolders**. `NNN` is the map number.

{% hint style="warning" %}
The **map number in the file name wins**. The server overwrites the `MapNumber` column of every line with the number at the start of the file name. A line with `MapNumber 0` inside `002 - Devias.txt` spawns in Devias.
{% endhint %}

Files that don't follow the `NNN - Name.txt` pattern (three digits, space, dash, space) are ignored, for example `teste000 - Lorencia - Copy.txt`.

Spawns are loaded only for maps that **this** GameServer hosts (see `MapServerInfo.dat` in [Maps](maps.md)).

### Syntax

A file has up to five sections, `0` to `4`, each closed by `end`. The section number is the **spawn type**.

| Section | Type | Columns | Behaviour |
|---|---|---|---|
| `0` | Fixed point | `Monster MapNumber Range PositionX PositionY Direction` | Spawns exactly at X,Y. Used for NPCs and shopkeepers. |
| `1` | Area | `Monster MapNumber Range BeginPosX BeginPosY EndPosX EndPosY Direction Quantity` | `Quantity` monsters at random positions inside the rectangle. |
| `2` | Scattered point | `Monster MapNumber Range PositionX PositionY Direction` | Like a fixed point, but each spawn lands within ±3 tiles. |
| `3` | Invasion | `Monster MapNumber Range BeginPosX BeginPosY EndPosX EndPosY Direction Quantity Value` | **Not** spawned at startup. The invasion system spawns them when the invasion with the same `Value` (group) starts. |
| `4` | Event | `Monster MapNumber Range PositionX PositionY Direction` | **Not** spawned at startup. Spawned by events (Blood Castle, Devil Square, Chaos Castle, Imperial Guardian...). |

| Column | Values | Meaning |
|---|---|---|
| `Monster` | monster index | Class from `Monster.txt`. |
| `MapNumber` | map | Overwritten by the file name (see above). |
| `Range` | tiles | Leash: how far from its spawn point the monster may chase or walk. `0` = it does not move (NPCs). |
| `PositionX` / `PositionY` | 0–255 | Spawn coordinate. |
| `BeginPosX`…`EndPosY` | 0–255 | Spawn rectangle (corner to corner). |
| `Direction` | 0–7 or `-1`/`*` | Facing: 0 NW, 1 N, 2 NE, 3 E, 4 SE, 5 S, 6 SW, 7 W. `-1` or `*` = random. |
| `Quantity` | number | How many monsters spawn in the rectangle. |
| `Value` | group | Invasion group (matches section 2 of `Event/InvasionManager.dat`). |

### Examples (real lines)

{% tabs %}
{% tab title="NPC (type 0)" %}
```
0
//Monster  MapNumber  Range  PositionX  PositionY  Direction  Comment
251        00         00     116        141        03         //Hanzo the Blacksmith
end
```
{% endtab %}
{% tab title="Area (type 1)" %}
```
1
//Monster  MapNumber  Range  BeginPosX  BeginPosY  EndPosX  EndPosY  Direction  Quantity  Comment
3          0          6      185        126        185      126      -1         12        // Spider
end
```
12 Spiders spawn at (185,126), random facing, each may move up to 6 tiles from where it spawned.
{% endtab %}
{% tab title="Invasion (type 3)" %}
```
3
//Monster  MapNumber  Range  BeginPosX  BeginPosY  EndPosX  EndPosY  Direction  Quantity  Value  Comment
055        00         30     125        190        140      203      0          04        0      //Death King
end
```
From `Invasion/000 - Lorencia.txt`: 4 Death Kings appear in that rectangle when invasion group 0 is started. Schedules and groups are configured in `Event/InvasionManager.dat` — see [Invasions](../events/invasions.md).
{% endtab %}
{% tab title="Event (type 4)" %}
```
4
//Monster  MapNumber  Range  PositionX  PositionY  Direction  Comment
084        11         30     014        034        00         //Chief Skeleton Warrior
end
```
From `Event/011 - Blood Castle 1.txt`: spawned by Blood Castle 1 during the event.
{% endtab %}
{% endtabs %}

### Rules & pitfalls

- Spawning on a tile where monsters can't stand (safe zone, wall) fails: the server tries random tiles nearby; if none works, the monster doesn't appear.
- Each `Quantity` monster counts toward the GameServer's monster limit. Huge quantities can hit the limit and leave later spawns empty.
- Don't put normal monsters (type 1/2) inside event map files expecting the event to control them: they spawn permanently.
- Keep only one copy of a map's file. Files in subfolders are loaded too, so a copy in `Event/` with the same normal spawns doubles them.

### Edit in the panel

The **Spot editor** (**Editors → Spot editor**) shows each map with its terrain, lets you place NPCs, areas, invasion and event spots by clicking, and warns about wrong file names and blocked tiles. See [Spot editor](../../dashboard/editors/spot.md).

---

## KanturuMonsterSetBase.txt

### What it does

Spawns of the **Kanturu** boss battle (Maya, Nightmare, Tower of Refinement), by stage. These are spawned by the Kanturu event, not at startup.

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Stage` | number | Battle stage/group that spawns this line. |
| `Monster` | monster index | Monster class. |
| `Map` | map | Map (Kanturu 3 = 39). |
| `Range` | tiles | Leash range. |
| `X` / `Y` | 0–255 | Position. |
| `Dir` | 0–7 or `*` | Facing, `*` = random. |

### Example (real lines)

```
0
//Stage   Monster   Map   Range   X      Y      Dir
0         364       39    0       188    110    *
0         354       39    30      176    66     *
```

---

## MovePath_35.dat

Waypoints that Crywolf monsters (map 34) follow when they march toward the altars. Columns: `State Map X Y`. Edit only if you also change the Crywolf map layout.

---

## Reload module

```
/reload monster
```

Reloads `Monster.txt`, all `MonsterSetBase` files, Kanturu spawns and the monster AI/skill files. Monsters on the maps are removed and spawned again. `/reload all` includes it.
