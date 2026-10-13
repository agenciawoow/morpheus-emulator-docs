# EventItemBag (boxes, boss drops and rewards)

## What it does

An **EventItemBag** ("bag") is a list of possible rewards plus the rules to pick from it. The server opens a bag when:

- a player **uses an item** (Box of Kundun, Box of Luck, Heart of Love, jewelry cases...),
- a player **kills a specific monster** (Kundun, Golden monsters, Medusa, invasion bosses...),
- an **event or system hands out a reward** (Blood Castle, Chaos Castle, Devil Square, Illusion Temple, Imperial Guardian, Double Goer, mixes such as Chaos Card / Lucky Coin / Cherry Blossom, Reset and Master Reset rewards, Online Lottery, Gift system...).

Two kinds of files work together:

| File | Role |
|---|---|
| `Data/EventItemBagManager.txt` | The **trigger table**: says which bag number opens for which item, monster or event code. |
| `Data/EventItemBag/NNN - Name.txt` | One file per bag: **what** can drop and **how** it is rolled. |

## Where it lives

```
MuServer/Data/
├── EventItemBagManager.txt
└── EventItemBag/
    ├── 000 - Box of Luck.txt
    ├── 007 - Box of Kundun 1.txt
    ├── ...
    ├── 145 - Reset Reward.txt
    └── _COMO-FUNCIONA.txt      (reference manual, never loaded)
```

{% hint style="info" %}
`_COMO-FUNCIONA.txt` is a Portuguese reference manual shipped inside the folder. The server ignores it because its name does not start with three digits. This page is the full English version of that manual, expanded.
{% endhint %}

---

## 1. EventItemBagManager.txt (the triggers)

### Syntax

One line per bag, terminated by `end`. Comments start with `//`.

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0–999 | Bag number. Must match the first three digits of a file in `Data/EventItemBag/` (`7` opens `007 - ....txt`). |
| `ItemIndex` | item index or `*` | Opens the bag when a player **uses** this item. Item index = `Section × 512 + Type` (for example `7179` = section 14, type 11, "Box of Kundun"). |
| `ItemLevel` | 0–15 or `*` | Required level of that item. `*` = any level. Lets one item (e.g. 7179) open different bags per level. |
| `MonsterClass` | monster index or `*` | Opens the bag when this monster dies (index from `Monster/Monster.txt`). The bag replaces that monster's normal drop. |
| `SpecialValue` | number or `*` | Event/system code. The server's events and systems call the bag by this code (Blood Castle level 1 = `0`, Chaos Castle 1 = `8`, Reset reward = `86`...). Keep the shipped codes; they are fixed in the server. |
| `Comment` | `// text` | Free text. |

Use only **one** trigger column per line and set the other two to `*`.

### Example (real lines)

```
//Index   ItemIndex   ItemLevel   MonsterClass   SpecialValue   Comment
0         7179        0           *              *              //Box of Luck
1         *           *           55             *              //Skeleton King
12        *           *           *              0              //Blood Castle 1
145       *           *           *              86             //Reset Reward
```

- Bag `0` opens when a player uses item 7179 at level 0.
- Bag `1` opens when monster 55 (Skeleton King) dies.
- Bag `12` is handed out by Blood Castle level 1.
- Bag `145` is given by the reset system (the `ItemBag_AL*` columns of [ResetTable](reset-and-experience.md) point to SpecialValue `86`).

### Event and system codes shipped (SpecialValue)

| Codes | Used by |
|---|---|
| 0–7 | Blood Castle 1–8 rewards |
| 8–14 | Chaos Castle 1–7 rewards |
| 15–16 | Starter Ring 40 / 80 |
| 17 | Senior Mix (Castle Siege) |
| 18 | Leo the Helper |
| 19–24 | Illusion Temple 1–6 |
| 25–27 | Cherry Blossom mixes 1–3 |
| 28 | Santa Claus |
| 29–31 | Lucky Coin exchanges |
| 32–36 | Chaos Card mixes 1–5 |
| 37–41 / 42–46 | Double Goer silver / golden chests |
| 47–51 / 52–56 | Imperial Guardian side boss / main boss |
| 57 | Crywolf reward |
| 60 / 61 / 62 | Online Lottery / Gift system / Find the Path event |
| 78–84 | Devil Square 1–7 |
| 86 / 87 | Reset reward / Master Reset reward |

---

## 2. Bag files: the two formats

Each bag file uses **one** of two formats. Pick one per file.

| Format | Sections used | When to use |
|---|---|---|
| **Normal** (simple) | `0`, `1`, `2` | Simple boxes: one list of items, fixed options, optional zen when the roll fails. |
| **Advanced** | `3`, `4`, `5` and up | Several independent draws, weighted item lists, per-class rewards, options from `ItemOptionRate.txt`, timed items, delivery straight to the inventory. |

{% hint style="danger" %}
**Do not mix the formats.** As soon as section `3` has a single valid line, the whole Normal part (sections 0/1/2) of that file is ignored at runtime. The Advanced format always wins.
{% endhint %}

### File rules (both formats)

- Comments are `//` up to the end of the line only. There is no `/* */` and no `#`. Always leave a space or a tab before a trailing `//`.
- Every section starts with its number alone on a line and ends with `end`.
- `*` means `-1` ("not set").
- The parser reads the file as a **stream of values**, not as lines. One column too many or too few shifts everything after it. The server detects this, writes a red `[ItemBag]` line in the log and ignores the extra values, but you must fix the file.
- If a section reaches the end of the file without `end`, the **whole bag is discarded** (with a red log line). The server prefers dropping nothing over dropping wrong items.
- Items that do not exist in `Data/Item/Item.txt` never drop. In the Normal format they are removed when the server checks the bags (red log line naming the bag, section and type); in the Advanced format they are refused at drop time (red log line with the item index).

---

## 3. Normal format

### Section 0 – settings (exactly one line)

| Column | Values | Meaning |
|---|---|---|
| `EventName` | `"text"` | Name shown in logs. Quoted. |
| `DropZen` | Zen | Zen dropped when the item roll fails. |
| `ItemDropRate` | 0–100 | Chance (%) that an **item** comes out. If the roll fails, `DropZen` drops instead. |
| `ItemDropCount` | 1+ | How many independent rolls happen when the bag opens. |
| `SetItemDropRate` | 0–10000 | Chance (per 10,000) of a random **ancient** item instead of a list item. Tested **before** the list, so it works even with an empty list. |
| `ItemDropType` | 0 / 1 | `0` = options exactly as written in sections 1/2. `1` = random options (see below). |
| `Fireworks` | 0 / 1 | `1` shows fireworks when the bag opens. |

### Section 1 – item list (8 columns)

| Column | Values | Meaning |
|---|---|---|
| `Section` | 0–15 | Item section (from `Item.txt`). |
| `Type` | 0–511 | Item type inside the section. |
| `MinLevel` | 0–15 | Lowest item level. |
| `MaxLevel` | 0–15 | Highest item level. The level is random between Min and Max. |
| `Skill` | 0 / 1 | Item comes with skill. |
| `Luck` | 0 / 1 | Item comes with luck. |
| `Option` | 0–7 | Additional option level (each point is +4 / +1% depending on the item). |
| `Excellent` | 0–6 | **How many** excellent options. The server picks which ones at random. |

### Section 2 – item list with ancient and sockets (10 columns)

Same 8 columns as section 1, plus:

| Column | Values | Meaning |
|---|---|---|
| `SetOption` | 0 / 1 | `1` makes the item ancient (random set of that item; 80% chance +5 stat, 20% +10 stat). The item must exist in `SetItemType.txt`. |
| `SocketOption` | 0–5 | Number of **empty** sockets for socket items. `0` = no sockets. |

### What happens when a Normal bag opens

For each of the `ItemDropCount` rolls:

1. Roll `ItemDropRate` (%). Failed → drop `DropZen` and go to the next roll.
2. Roll `SetItemDropRate` (per 10,000). Passed → drop a random ancient item and go to the next roll.
3. Otherwise pick **one line** of sections 1/2 at random (every line has the same chance) and create it.

With `ItemDropType = 1`:

- `Skill` and `Luck` set to 1 become a 50% coin flip each.
- `Option` is ignored and re-rolled at random (small chance, higher levels rarer).
- If the item got excellent options, it always gets skill.

{% hint style="warning" %}
**Trap: `ItemDropRate 100` kills the zen.** The test is "random 0–99 is **greater or equal** to ItemDropRate → zen". With `100` the item always comes out, so `DropZen` never drops. If you want zen sometimes, use a value below 100.
{% endhint %}

### Example (real file: `000 - Box of Luck.txt`)

```
0
//EventName    DropZen  ItemDropRate  ItemDropCount  SetItemDropRate  ItemDropType  Fireworks
"Box of Luck"  1000     100           1              0                0             1
end

1
//Section  Type  MinLevel  MaxLevel  Skill  Luck  Option  Excellent
0          6     7         8         0      0     0       0          //Gladius
0          7     7         8         0      0     0       0          //Falchion
7          0     7         8         0      0     0       0          //Bronze Helm
end
```

One roll; always an item (so the 1000 zen never drops); a random line, +7 or +8, no options; fireworks on.

**Chance per item:** with 88 lines in section 1, each item has 1/88 ≈ 1.14% per roll.

---

## 4. Advanced format

### Section 3 – the draws

2 or 3 columns. Each line is an **independent draw**.

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Draw id. Links to section 4. |
| `DropRate` | 0–10000 | Chance (per 10,000) that this draw happens. `10000` = always. |
| `DropInventory` | 0 / 1 (optional) | `0` = drops on the ground. `1` = goes straight into the inventory (falls to the ground if it does not fit). Without this column, `0`. |

The server looks at the **first data line** to decide if the file has 2 or 3 columns, and reads every other line the same way. You don't need to convert old files.

### Section 4 – who gets what

5 columns plus one flag per class.

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Links to the draw with the same `Index` in section 3. |
| `Section` | 5 and up | Which item list (section number in this file) to use. |
| `SectionRate` | weight | **Relative** weight between the lines that share the same `Index`. `9000` and `1000` on the same Index = 90% and 10%. They don't need to add up to 10000. |
| `MoneyAmount` | Zen | Zen dropped if the chosen item section does not exist or is empty. |
| `OptionValue` | bit sum | `1` = fireworks; `2` = check the whole **party** for the class requirement (not only the player); `4` = drop **every** item of the section; `8` = drop a random **ancient** item instead of the list. Add them up: `5` = fireworks + every item. |
| `DW DK FE MG DL SU RF [GL RW SL GC KUN LEM KIN]` | 0, 1, 2, 3 | Per-class flag. `0` = this class never gets this line. `1` = gets it from the base class. `2` = only after the 2nd class change. `3` = only after the 3rd class change. |

Files can have **7** class columns (DW–RF) or **14** (up to KIN). The server counts them on the first data line of section 4 and uses that number for all other lines.

### Sections 5 and up – item lists

11 columns per line. You can have as many item sections as you want (5, 6, 7...).

| Column | Values | Meaning |
|---|---|---|
| `Index` | item index | **Absolute** item index = `Section × 512 + Type`. Example: `6277` = section 12, type 133. `7181` = Jewel of Bless. |
| `Level` | 0–15 | Base item level (used when `Option0` is `*`). |
| `Grade` | bitmask 0–63 | Fixed excellent options, used only when `Option4` gives no excellent option. `1`, `2`, `4`, `8`, `16`, `32` = the six excellent options; add them up. |
| `Option0` | ItemOptionRate row or `*` | **Level** roll — row of section 0 of `ItemOptionRate.txt`. |
| `Option1` | row or `*` | **Skill** roll — section 1. |
| `Option2` | row or `*` | **Luck** roll — section 2. |
| `Option3` | row or `*` | **Additional option (JoL)** roll — section 3. |
| `Option4` | row or `*` | **How many excellent options** — section 4. |
| `Option5` | row or `*` | **Ancient** roll — section 5. |
| `Option6` | row or `*` | **How many sockets** — section 6. |
| `Duration` | seconds | Item lifetime. `0` = permanent. `86400` = 1 day. |

{% hint style="info" %}
Some shipped files label these columns with friendly names in the comment header: `Level Skill Luck Life Exe Ancient Socket` = `Option0 … Option6`. It is the same order.
{% endhint %}

### The option-column trap

{% hint style="danger" %}
**`0` does NOT mean "none".** In `Option0`–`Option6`, a number is a **row index** in `Data/Item/ItemOptionRate.txt`. Row `0` is the "(Common)" row used for normal monster drops — and it **rolls**. With `0` in Option1, Option2 and Option3 the item comes out with about 6% skill, 4% luck and 4% additional option, even though you never asked for it.

To **not** roll anything, write `*`. The server then leaves that value untouched (Level stays as the `Level` column, the rest stay off).

`ItemOptionRate.txt` also ships ready-made rows labelled `(EventItemBag)` starting at index **11** (for example skill `11` = never, `12` = always, `13` = 50/50). See [Item options](item-options.md).
{% endhint %}

### What happens when an Advanced bag drops

For **each** line of section 3, in order:

1. Roll `DropRate` / 10,000. Failed → skip to the next draw.
2. Collect the section-4 lines with the same `Index` whose class flags accept the player (or anyone in the party, if `OptionValue` has bit 2).
3. Pick one of them, weighted by `SectionRate`. No eligible line → nothing for this draw.
4. If the chosen item section is missing or empty → drop `MoneyAmount` zen.
5. Bit `8` set → drop a random ancient item (fireworks if bit 1).
6. Bit `4` set → create **every** item of the section. Otherwise create **one** item picked uniformly from the section.
7. Each item rolls `Option0`–`Option6` against `ItemOptionRate.txt`, then gets its `Duration`.

There is **no** `ItemDropCount` in this format: the number of items equals the number of section-3 draws that pass (times the section size, when bit 4 is used).

{% hint style="warning" %}
When a bag is opened by a **mix** (Chaos Card, Lucky Coin, Senior Mix, Cherry Blossom, reset rewards...) the server needs exactly one item: it uses the first draw that passes, gives one item, and bit `4` falls back to the first item of the section. `DropInventory` only matters for bags that drop on the ground.
{% endhint %}

### Chance math (Advanced)

```
P(item X) = DropRate/10000  ×  SectionRate(line) / Σ SectionRate(eligible lines)  ×  1 / (items in that section)
```

Example (real file `007 - Box of Kundun 1.txt`):

```
3
//Index  DropRate  DropInventory
0        10000     0
end

4
//Index  Section  Rate   Money  OptionValue  DW DK FE MG DL SU RF
0        5        9000   0      0            1  1  1  1  1  1  1
0        6        1000   0      0            1  1  1  1  1  1  1
end

5
//Index  Level  Grade  Option0  Option1  Option2  Option3  Option4  Option5  Option6  Duration
0004     0      0      *        1        0        9        20       0        0        0      // Sword of Assassin
0005     0      0      *        1        0        9        20       0        0        0      // Blade
end
```

- One draw, always happens (10000).
- 90% section 5, 10% section 6, every class accepted.
- If section 5 has 60 items, one specific item has `1 × 0.9 × 1/60 = 1.5%`.
- Options of the Sword of Assassin line: level `*` → stays +0; skill row `1` (always skill); luck row `0` (Common, 4%); JoL row `9` (Moss Merchant table); excellent row `20` (EventItemBag 1~2 → 1 or 2 excellent options, 50/50); ancient row `0` (none); sockets row `0` (none).

### Example: a reward that goes to the inventory, for 1 day

```
3
0    10000    1
end

4
0    5    10000    0    1    1 1 1 1 1 1 1
end

5
//Index  Level  Grade  Option0  Option1  Option2  Option3  Option4  Option5  Option6  Duration
7181     0      0      *        *        *        *        *        *        *        86400   // Jewel of Bless (1 day)
end
```

---

## 5. Common pitfalls

| Symptom | Cause | Fix |
|---|---|---|
| Bag never drops anything, no error | Its number is not in `EventItemBagManager.txt`. Bags without a manager line are never loaded. | Add the manager line. |
| Two files start with the same number (e.g. `131 - Boss Zaikan.txt` and `131 - Reset Reward.txt`) | Only the **first one in alphabetical order** is loaded; the other is silently ignored. | Give each bag a unique number. |
| File renamed to `_131 - ...` or `teste 131.txt` | Only names in the `NNN - Name.txt` pattern (three digits, space, dash, space) are read. | Rename to the pattern. |
| Items come out with skill/luck you didn't want | `0` in the Option columns (see the trap above). | Use `*` or an `(EventItemBag)` row. |
| Zen never drops (Normal) | `ItemDropRate 100`. | Lower it. |
| Whole bag stopped working after an edit | Missing `end` or a broken column count. Red `[ItemBag]` line in the log. | Fix the section; check the log. |
| Ancient with `Option5` doesn't appear | The item is not in `SetItemType.txt`, or the row picks set type B on an item that only has set A. | Use an ancient row for set A (values 1, 3, 4) or add the item to `SetItemType.txt`. |
| Item has no level variation | `Option0` is `*` (keeps `Level`). | Use a level row (e.g. `27` = +0~+2). |

## 6. Where to check that it worked

In the GameServer log, at startup or after a reload:

```
[ItemBag] <file>: secao 3 com N colunas
[ItemBag] <file>: secao 4 com N coluna(s) de classe
[ServerInfo] EventItemBag loaded successfully
```

Any **red** `[ItemBag]` line names the file and what is wrong (extra columns, missing `end`, item not in `Item.txt`).

## Edit in the panel

The **EventItemBag Editor** (**Editors → EventItemBag Editor**) edits the manager and every bag visually, in both formats, with the class flags, option rows and chance preview. See [EventItemBag editor](../../dashboard/editors/event-item-bag.md).

## Reload module

```
/reload eventitembag
```

Reloads `EventItemBagManager.txt` and every bag file. `/reload all` also includes it.
