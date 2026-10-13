# Reset, Master Reset and experience tables

| File | What it controls |
|---|---|
| `Util/ResetTable.txt` | Reset requirements and rewards **by reset range** |
| `Util/MasterResetTable.txt` | Master Reset requirements and rewards **by master-reset range** |
| `Util/ExperienceTable.txt` | Experience multiplier by level / reset / master reset range |

The commands themselves (`/reset`, `/mreset`) and their global settings — reset type, default level, Zen, points per reset — are in `GameServerInfo - Command.dat`. These tables **override** those defaults for specific ranges. If a table is empty, or no row matches, the global settings are used.

**Account levels:** every `_AL0`…`_AL3` column is per VIP level — AL0 = Free, AL1 = Bronze, AL2 = Silver, AL3 = Gold.

---

## ResetTable.txt

### What it does

Lets the reset cost grow with the number of resets: higher level requirement, more Zen, jewels to deliver, and different rewards per range.

### Where it lives

`MuServer/Data/Util/ResetTable.txt` (shipped empty: global settings are used)

### Syntax

**Section 0 – requirements**

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Row id. Links to section 1. |
| `MinReset` / `MaxReset` | number | Range of the reset being done (the player's next reset number). |
| `Level_AL0`…`Level_AL3` | level or `-1` | Level required. `-1` = global setting. |
| `Money_AL0`…`Money_AL3` | Zen or `-1` | Zen cost. `-1` = Zen of the `/reset` line in `Command.txt`, else the global setting. |
| `Bless` `Soul` `Life` `Creation` `Guardian` `Gemstone` `Harmony` `Chaos` `LowerStone` `HigherStone` | number or `*` | Jewels the player must carry; they are consumed. `*`/`0` = not required. |

The jewel columns are, in order: Jewel of Bless, Soul, Life, Creation, Guardian, Gemstone, Jewel of Harmony, Jewel of Chaos, Lower Refining Stone, Higher Refining Stone.

**Section 1 – rewards**

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Same `Index` as the section-0 row. |
| `Point_AL0`…`Point_AL3` | number or `-1` | Stat points **per reset**. With cumulative resets (reset type 1) the player gets `points × total resets`. `-1` = global setting. |
| `ItemBag_AL0`…`ItemBag_AL3` | bag code or `*` | Reward bag: the **SpecialValue** of a bag in [EventItemBagManager.txt](event-item-bag.md) (shipped reset reward = `86`). `*` = none. |

### Example

```
0
//Index MinReset   MaxReset   Level_AL0   Level_AL1   Level_AL2   Level_AL3   Money_AL0   Money_AL1   Money_AL2   Money_AL3   Bless  Soul  Life  Creation  Guardian  Gemstone  Harmony  Chaos  LowerStone  HigherStone
1       1          50         400         390         380         370         1000000     900000      800000      700000      *      *     *     *         *         *         *        *      *           *
2       51         100        400         395         390         385         5000000     4000000     3000000     2000000     5      5     *     *         *         *         *        *      *           *
end

1
//Index   Point_AL0   Point_AL1   Point_AL2   Point_AL3   ItemBag_AL0   ItemBag_AL1   ItemBag_AL2   ItemBag_AL3
1         500         550         600         650         *             *             *             *
2         450         500         550         600         86            86            86            86
end
```

- Resets 1–50: level 400 (Free) to 370 (Gold), 1M to 700k Zen, 500–650 points per reset.
- Resets 51–100: 5 Bless + 5 Soul extra and the reset reward bag (code 86).

---

## MasterResetTable.txt

### What it does

Same idea for the Master Reset (`/mreset`): requirements and rewards per range of master resets.

### Where it lives

`MuServer/Data/Util/MasterResetTable.txt` (shipped with an example row commented out)

### Syntax

**Section 0 – requirements**

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Row id. Links to section 1. |
| `MinMReset` / `MaxMReset` | number | Range of the master reset being done. |
| `Reset_AL0`…`Reset_AL3` | number or `-1` | Resets the character must have. |
| `Level_AL0`…`Level_AL3` | level or `-1` | Level required (level + master level for master characters). |
| `Money_AL0`…`Money_AL3` | Zen or `-1` | Zen cost. |
| `Bless` … `HigherStone` | number or `*` | Jewels consumed (same order as ResetTable). |

**Section 1 – rewards**

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Same `Index` as section 0. |
| `Point_AL0`…`Point_AL3` | number or `-1` | Points per master reset. |
| `ItemBag_AL0`…`ItemBag_AL3` | bag code or `*` | Reward bag SpecialValue (shipped master reset reward = `87`). |

### Example (shipped, commented)

```
0
//Index MinMReset MaxMReset Reset_AL0 Reset_AL1 Reset_AL2 Reset_AL3 Level_AL0 Level_AL1 Level_AL2 Level_AL3 Money_AL0 Money_AL1 Money_AL2 Money_AL3 Bless Soul Life Creation Guardian Gemstone Harmony Chaos LowerStone HigherStone
//1		0         4000      400       400       400       400       400       400       400       400       2030630     2030630     2030630     2030630    10     10    //10    10         *         *         *        *      *           *
end

1
//Index   Point_AL0   Point_AL1   Point_AL2   Point_AL3   ItemBag_AL0   ItemBag_AL1   ItemBag_AL2   ItemBag_AL3
//1         5000        5000        5000        5000        *             *             *             *
end
```

{% hint style="warning" %}
The commented example has a `//` in the middle of the jewel columns. If you uncomment it, remove that inner `//` too — otherwise the rest of the line is ignored and the row is short of columns.
{% endhint %}

---

## ExperienceTable.txt

### What it does

Multiplies the experience a character earns according to its level, master level, resets and master resets. Typical use: high rate for new players, lower rate as resets grow.

### Where it lives

`MuServer/Data/Util/ExperienceTable.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `MinLevel` / `MaxLevel` | level or `*` | Character level range. |
| `MinMasterLevel` / `MaxMasterLevel` | level or `*` | Master level range (used for master experience). |
| `MinReset` / `MaxReset` | number or `*` | Reset range. |
| `MinMasterReset` / `MaxMasterReset` | number or `*` | Master reset range. |
| `ExperienceRate` | % | Multiplier. `100` = no change, `700` = ×7. |

The **first** matching line wins. No match = 100%.

The final experience is: monster experience × global rate (`GameServerInfo - Common.dat`, per VIP level) × map rate ([MapManager](maps.md)) × event bonuses × this table.

### Example (real lines)

```
//MinLevel MaxLevel MinMasterLevel MaxMasterLevel MinReset MaxReset MinMasterReset MaxMasterReset ExperienceRate
*          *        *              *              0        4        *              *              700
*          *        *              *              5        9        *              *              695
```

Characters with 0–4 resets get ×7 experience; 5–9 resets ×6.95.

### Rules & pitfalls

- Order matters: put specific lines (narrow ranges) **before** general ones.
- Leave no gaps in the ranges if you want every player covered; uncovered players get 100%.

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload util
```

Reloads all `Util/` files. `/reload all` includes it.
