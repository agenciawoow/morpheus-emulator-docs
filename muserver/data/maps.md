# Maps

| File | What it controls |
|---|---|
| `MapManager.txt` | Per-map rules: PK, experience and drop multipliers, helper/custom systems allowed, respawn gate |
| `MapServerInfo.dat` | Which GameServer hosts which map (multi-server setups) |
| `Terrain/*.att` | Walkable tiles, safe zones and walls of every map |

---

## MapManager.txt

### What it does

One line per map with its multipliers and switches. Maps not listed use the defaults shown in the table (`*` gives the same default).

### Where it lives

`MuServer/Data/MapManager.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | map number | Map (0 Lorencia, 1 Dungeon, 2 Devias, 3 Noria, 4 Lost Tower, ...). |
| `NonPK` | `*`, 0, 1, 2 | `*` = use the global NonPK setting of `GameServerInfo - Common.dat`. `0` = PK allowed. `1` = **non-PK** map (players can't attack each other). `2` = **free PvP**: players can fight without getting self-defense or PK status. |
| `ViewRange` | tiles | How far players see other objects. Default 15. |
| `ExperienceRate` | % | Experience multiplier on this map (normal and master). `100` = no change, `200` = double. |
| `ItemDropRate` | % | Multiplier of the normal item drop chance. `100` = no change. |
| `ExcItemDropRate` | per 1,000,000 | Chance that a monster (level 25+) drops an **excellent** item. `0` = never. |
| `SetItemDropRate` | per 1,000,000 | Chance that a monster (level 25+) drops an **ancient** item. `0` = never. |
| `SocketItemDrop` | 0 / 1 | `1` = socket items can be normal drops here. |
| `HelperEnable` | 0 / 1 | `1` = MU Helper allowed. |
| `GensBattle` | 0 / 1 | `1` = Gens battle zone. |
| `CustomAttack` | 0 / 1 | `1` = the custom auto-attack (`/attack`, `/offattack`) can be used. |
| `CustomStore` | 0 / 1 | `1` = custom stores (`/store`, `/offstore`) can be opened. |
| `CustomPick` | 0 / 1 | `1` = auto-pick (`/pick`) works. |
| `PkDropItem` | 0 / 1 | `1` = outlaws (PK level 6+) can drop items when killed here. |
| `DeathGate` | gate number | Gate where players respawn after dying on this map (from `Move/Gate.txt`). Default 17 (Lorencia). |
| `Comment` | `"text"` | Map name. **Required** and quoted: it is a real column, not a `//` comment. |

### Example (real lines)

```
//Index  NonPK  ViewRange  ExperienceRate  ItemDropRate  ExcItemDropRate  SetItemDropRate  SocketItemDrop  HelperEnable  GensBattle  CustomAttack  CustomStore  CustomPick  PkDropItem  DeathGate  Comment
2        *      10         180             30            0                0                0               1             0           1             1            1           0           22         "Devias"
11       *      10         150             200           0                250              0               1             0           0             0            0           0           22         "Blood Castle 1"
```

- Devias: 180% experience, 30% of the normal item drop, no excellent/ancient drops from the map rate, helper, `/attack`, `/store` and `/pick` allowed, respawn at gate 22.
- Blood Castle 1: 150% experience, 200% item drop, ancient chance 250 per million (0.025%), custom systems off.

### Rules & pitfalls

- `ExcItemDropRate` and `SetItemDropRate` are **per million**. `250` = 0.025%.
- `ItemDropRate` multiplies the global `ItemDropRate` of `GameServerInfo - Common.dat`; `0` here turns normal drops off on that map.
- `DeathGate` must be a valid gate. A wrong number makes dead players respawn in Lorencia.
- Every line needs all 16 columns, including the quoted name at the end.

---

## MapServerInfo.dat

### What it does

When you run more than one GameServer (for example a main GameServer and a GameServerCS for Castle Siege), this file says which server hosts which map, and where to send players who walk into a map hosted elsewhere.

### Where it lives

`MuServer/Data/MapServerInfo.dat`

### Syntax

**Section 0 – servers**

| Column | Values | Meaning |
|---|---|---|
| `ServerCode` | number | GameServer code (the `ServerCode` of that GameServer). |
| `MapServerGroup` | number | Group of servers that share maps. |
| `InitSetVal` | 1 / 0 | `1` = this server hosts **every** map unless section 1 says otherwise. `0` = hosts **no** map unless section 1 says so. |
| `IpAddress` | `S` + address | Address players are sent to. **Must start with the letter `S`** (the server skips the first character). |
| `Port` | number | GameServer port. |

**Section 1 – map exceptions**

| Column | Values | Meaning |
|---|---|---|
| `ServerCode` | number | The server this rule is for. |
| `NotMoveOption` | 0 / 1 | `1` = this server **hosts** the map. `0` = the map is hosted by `NextServerCode`; players entering it are moved there. |
| `NextMap` | map number | The map. |
| `NextServerCode` | number or `-1` | Destination server when `NotMoveOption = 0`. `-1` when not used. |

### Example (structure of the shipped file)

```
0
//ServerCode   MapServerGroup   InitSetVal   IpAddress            Port
0              0                1            S<your-server-ip>    55901
19             0                0            S<your-server-ip>    55919
end

1
//ServerCode   NotMoveOption   NextMap   NextServerCode
0              0               30        19
19             1               30        -1
end
```

Server 0 hosts everything except map 30 (Castle Siege), which belongs to server 19. Server 19 hosts only map 30 (and the other maps listed for it).

### Rules & pitfalls

- Every `ServerCode` used in section 1 must be declared in section 0.
- Monster spawns are loaded only for maps the server hosts. If a map shows no monsters, check this file first.
- A broken file stops the GameServer at startup. With `/reload map` the server keeps the old tables and writes a red log line.

---

## Terrain (.att files)

### What they do

Each `.att` file is the tile map of one game map: which tiles are walkable, safe zones, walls and "no ground". The server uses it for movement, attacks, drops and spawn validation.

### Where they live

`MuServer/Data/Terrain/terrainN.att`, where `N` = map number + 1 (Lorencia = `terrain1.att`). Special states:

| File | Used for |
|---|---|
| `terrain35_PEACE.att`, `terrain35_OCCUPIED.att`, `terrain35_WAR.att` | Crywolf states |
| `terrain40_CLOSE.att`, `terrain40_OPEN.att` | Kanturu Tower of Refinement closed/open |

A map without its own file uses the previous map's terrain.

{% hint style="danger" %}
**Do not edit `.att` files by hand.** They are binary and must be identical to the client's terrain files. Copy them from the matching client version only.
{% endhint %}

The [Spot editor](../../dashboard/editors/spot.md) reads these files to show safe zones and blocked tiles on the map while you place spawns.

### Reload

```
/reload terrain
/reload terrain force
```

The reload is refused while Blood Castle, Chaos Castle, Double Goer, Imperial Guardian or the Morpheus Tournament is running, because those events open and close doors on the terrain. `force` reloads anyway (the doors go back to their file state).

---

## Edit in the panel

No dedicated visual editor for `MapManager.txt` and `MapServerInfo.dat`. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

| File | Reload |
|---|---|
| `MapManager.txt`, `MapServerInfo.dat` | `/reload map` (keeps the old tables if a file is broken) — also part of `/reload common` |
| `Terrain/*.att` | `/reload terrain` |
