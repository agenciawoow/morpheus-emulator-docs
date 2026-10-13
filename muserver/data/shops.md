# NPC shops

## What it does

NPC shops are the stores that open when players talk to merchants (Hanzo, Pasi, Amy, Lumen...). Two parts:

| File | Role |
|---|---|
| `ShopManager.txt` | Links a shop number to an NPC (and optionally to one exact NPC position). |
| `Shop/NNN - Name.txt` | The items of shop `NNN`, in the order they appear. |

Prices come from [ItemValue.txt](items.md) (or the automatic formula when the item has no row there). An item with a `Coin1`/`Coin2`/`Coin3` price in ItemValue is sold for that coin instead of Zen.

## Where it lives

```
MuServer/Data/
├── ShopManager.txt
└── Shop/
    ├── 000 - Hanzo the Blacksmith.txt
    ├── 001 - Liaman the Barmaid.txt
    └── ...
```

---

## ShopManager.txt

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0–999 | Shop number. Loads the file `Shop/NNN - ....txt` with the same number. |
| `Npc` | monster index | NPC class (from `Monster.txt`) that opens this shop. |
| `Map` | map or `*` | Only the NPC standing on this map. `*` = any. |
| `X` / `Y` | coordinate or `*` | Only the NPC at this exact position. `*` = any. |
| `Comment` | `// text` | Free text. |

With `Map`, `X` and `Y` you can give **two NPCs of the same class different shops** (for example a Hanzo in Lorencia and another Hanzo elsewhere).

### Example (real lines)

```
//Index   Npc	Map	X	Y	Comment
0         251	*	*	*	//Hanzo the Blacksmith
5         253	*	*	*	//Amy the Potion Girl
end
```

---

## Shop files (16 columns)

### Syntax

One item per line; the file ends with `end`. Blank lines and `//` comments are allowed.

| # | Column | Values | Meaning |
|---|---|---|---|
| 1 | `Group` | 0–15 | Item section. |
| 2 | `Index` | 0–511 | Item type. |
| 3 | `Level` | 0–15 | Item level. |
| 4 | `Dur` | 0–255 | Durability (or quantity for potions/arrows). `0` = the item's default durability. |
| 5 | `Skill` | 0 / 1 | With skill. |
| 6 | `Luck` | 0 / 1 | With luck. |
| 7 | `Option` | 0–7 | Additional option level (+4 per point). |
| 8 | `ExcOpt` | 0–63 | Excellent options bitmask (1, 2, 4, 8, 16, 32; add them up). |
| 9 | `Anc` | number | Ancient value as stored on the item. `0` = not ancient. `5` = set A +5 stat, `9` = set A +10, `6` = set B +5, `10` = set B +10. |
| 10 | `JoH` | number | Jewel of Harmony option as stored on the item (`option × 16 + level`). `0` = none. |
| 11 | `Oex` | 0 / 1 | 380 option. `1` = the item has its 380 options (only for items in `380ItemType.txt`). |
| 12–16 | `Socket1`–`Socket5` | 0–255 | Socket contents. `255` = no socket, `254` = empty socket, other = a seed-sphere option. Only used by socket items. |

### Example (real lines, `000 - Hanzo the Blacksmith.txt`)

```
// Group Index	Level	Dur	Skill	Luck	Option	ExcOpt	Anc	JoH	Oex	Socket1	Socket2	Socket3	Socket4	Socket5	// Item Name
7	0	7	0	0	1	0	0	0	0	0	255	255	255	255	255	// Bronze Helm
0	14	0	7	1	1	0	0	0	0	0	255	255	255	255	255	// Lightning Sword
end
```

- Bronze Helm +7 with luck, default durability.
- Lightning Sword +0 with durability 7, skill and luck.

### How items are placed

The shop window is **8 columns × 15 rows** (120 slots). Items are placed in file order, each in the first free space where it fits. When the window is full, the remaining lines are ignored. Group lines of the same size together to avoid gaps.

### Rules & pitfalls

- The file must follow the `NNN - Name.txt` pattern and its number must exist in `ShopManager.txt`, otherwise it is never loaded.
- Items not in `Item.txt` are skipped.
- Lines with fewer than 16 values shift every following line. Always write all 16 values (use `255` for unused sockets).
- Selling excellent or ancient items in a shop changes your economy: set prices in [ItemValue.txt](items.md).

## Edit in the panel

The **NPC Shop editor** (**Editors → NPC Shop editor**) shows the 8×15 grid, lets you drag items, edit options with hints, and auto-arrange to close gaps. See [NPC Shop editor](../../dashboard/editors/npc-shop.md).

## Reload module

```
/reload shop
```

Reloads `ItemValue.txt`, `ShopManager.txt` and every shop file, and recalculates item prices in players' inventories. `/reload all` includes it.
