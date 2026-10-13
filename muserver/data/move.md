# Gates, warps and summon areas

| File | What it controls |
|---|---|
| `Move/Gate.txt` | Every portal, map entrance/exit and respawn point |
| `Move/Move.txt` | The warp list (M window and `/move <map>`): price, requirements, destination gate |
| `Move/MoveSummon.txt` | Where Dark Lord summon and Resurrection/Mobility talismans may teleport players |

Custom warp commands live in `Custom/CustomMove.txt` — see [Custom Move](../customs/custom-move.md).

---

## Gate.txt

### What it does

A **gate** is a rectangle on a map. There are three kinds:

- **Entrance** (`Flag 1`): stepping on it sends the player to another gate (stairs, dungeon doors).
- **Exit / destination** (`Flag 2`): where a player arrives through an entrance.
- **Respawn / warp target** (`Flag 0`): where players appear after dying (`DeathGate` in [MapManager](maps.md)), after `/move`, after a town portal, etc.

### Where it lives

`MuServer/Data/Move/Gate.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | gate number | Gate id. Referenced by `Move.txt`, `MapManager.txt` (`DeathGate`), events and other gates. |
| `Flag` | 0, 1, 2 | `0` = respawn/warp target, `1` = entrance, `2` = exit. |
| `Map` | map number | Map of this gate. |
| `X` `Y` `TX` `TY` | 0–255 | Rectangle (from X,Y to TX,TY). Arrival position is random inside it. |
| `TargetGate` | gate number | For entrances: the gate the player is sent to. `0` for others. |
| `TargetDir` | 0–7 | Facing on arrival. |
| `MinLevel` | level | Minimum level to use the gate. Magic Gladiator, Dark Lord and Rage Fighter need only 2/3 of it (except Swamp of Calmness). |
| `MaxLevel` | level | Maximum level. |
| `MinReset` / `MaxReset` | number or `*` | Reset range (`*` = no limit). |
| `AccountLevel` | 0–3 | Minimum VIP level. |

### Example (real lines)

```
//Index   Flag   Map   X      Y      TX      TY      TargetGate   TargetDir   MinLevel   MaxLevel   MinReset   MaxReset   AccountLevel
1         1      0     121    232    123     233     2            0           20         1400       *          *          0
2         2      1     107    247    110     247     0            1           20         1400       *          *          0
17        0      0     133    118    151     135     0            0           0          1400       *          *          0
```

- Gate 1: entrance in Lorencia (Dungeon stairs) → gate 2, level 20+.
- Gate 2: arrival point in Dungeon.
- Gate 17: Lorencia town area, used as respawn point.

### Rules & pitfalls

- The client has its own gate table (`Gate.bmd`). Entrance rectangles must match the client, or players step on stairs and nothing happens.
- A rectangle covering non-walkable tiles makes arrivals fail or land in walls.

---

## Move.txt

### What it does

The warp list players see in the **M** (Move) window and use with `/move <name>`.

### Where it lives

`MuServer/Data/Move/Move.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Warp id. Must match the client's move list (`Movereq.bmd`). |
| `MainName` | `"text"` | Name used by `/move` and shown in the window. |
| `SubName` | `"text"` | Secondary name. |
| `Money` | Zen | Cost of the warp. |
| `MinLvl` | level | Minimum level. |
| `MaxLvl` | level or `-1` | Maximum level; `-1` = no limit. |
| `MinReset` / `MaxReset` | number or `-1` | Reset range; `-1` = no limit. |
| `AccLvl` | 0–3 | Minimum VIP level. |
| `Gate` | gate number | Destination gate in `Gate.txt`. |

### Example (real lines)

```
//Index  MainName     SubName      Money   MinLvl  MaxLvl  MinReset  MaxReset  AccLvl  Gate
2        "Lorencia"   "Lorencia"   2000    10      -1      -1        -1        0       17
4        "Devias"     "Devias"     2000    20      -1      -1        -1        0       22
```

`/move Devias` costs 2000 Zen, needs level 20 and sends the player to gate 22.

### Rules & pitfalls

- Adding a line here does not add it to the client's M window; the client list must be updated too. `/move <MainName>` works without client changes.

---

## MoveSummon.txt

### What it does

Defines map areas where **summon-type teleports** are allowed: the Dark Lord's Summon skill, the Talisman of Resurrection and the Talisman of Mobility. If the destination is not inside a listed area, or the player doesn't meet the requirements, the teleport is refused.

### Where it lives

`MuServer/Data/Move/MoveSummon.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Map` | map number | Map. |
| `X` `Y` `TX` `TY` | 0–255 | Allowed area (`1 1 255 255` = whole map). |
| `MinLevel` / `MaxLevel` | level | Level range (MG/DL/RF use 2/3 of MinLevel). |
| `MinReset` / `MaxReset` | number or `*` | Reset range. |
| `AccountLevel` | 0–3 | Minimum VIP level. |
| `PkMove` | 0 / 1 | `0` = players with PK status (level 5+) can't be summoned here. |

### Example (real line)

```
//Map   X      Y      TX      TY      MinLevel   MaxLevel   MinReset   MaxReset   AccountLevel PkMove
0       1      1      255     255     1          400        *          *          0            0
```

Summons into Lorencia are allowed anywhere for levels 1–400, not for PKs.

### Rules & pitfalls

- Maps **not listed** don't allow summoning at all.
- `MaxLevel` is a hard limit. If your server's maximum level is above 400, raise the shipped `400` or high-level characters can't be summoned or use talismans.
- Extra hard-coded rules apply on top: no Uniria/Dinorant in Atlans, wings or Dinorant/Fenrir required for Icarus and Kanturu 3.

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload move
```

Reloads `Gate.txt`, `Move.txt`, `MoveSummon.txt` and `Custom/CustomMove.txt`.
