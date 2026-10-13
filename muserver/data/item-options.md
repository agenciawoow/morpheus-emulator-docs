# Item options

This page covers every file that decides **which options an item gets** and **what those options do**.

| File | What it controls |
|---|---|
| `Item/ItemOptionRate.txt` | The odds tables used to roll level, skill, luck, additional option, excellent count, ancient and sockets |
| `Item/ItemOption.txt` | What each option slot (skill, luck, additional, excellent bits) actually gives, per item range |
| `Item/ExcellentOptionRate.txt` | Legacy reference table (not read by this GameServer) |
| `Item/380ItemOption.txt` / `380ItemType.txt` | The "380" (PvP) options and which items have them |
| `Item/SetItemOption.txt` / `SetItemType.txt` | Ancient sets: bonuses and which items belong to which set |
| `Item/SocketItemOption.txt` / `SocketItemType.txt` | Seed-sphere options, socket bonuses and which items have sockets |
| `Item/JewelOfHarmonyOption.txt` / `JewelOfHarmonyType.txt` | Jewel of Harmony options and which items can be smelted |
| `Item/PetItemOption.txt`, `EarringItemOption.txt`, `FlagItemOption.txt` | Extra stats for custom pets, earrings and flags |

---

## ItemOptionRate.txt

### What it does

Holds **weighted odds tables**. Other systems point to a row of this file to roll an item's options:

- normal monster drops (row `0` "Common" and row `1` "Exc"),
- mixes (Chaos item mix, wing mixes, socket weapon mix),
- Moss Merchant, Lucky items,
- [ItemDrop.txt](item-drop.md) and the [Advanced EventItemBag](event-item-bag.md) (`Option0`–`Option6` columns).

### Where it lives

`MuServer/Data/Item/ItemOptionRate.txt`

### Syntax

Seven sections, one per kind of option. Each line is a row: `Index` followed by one weight per possible result, then a comment.

| Section | Rolls | Columns | Result of column N |
|---|---|---|---|
| `0` | Item **level** | `Rate0`–`Rate15` | +N (0 to +15) |
| `1` | **Skill** | `Rate0`–`Rate1` | 0 = no skill, 1 = skill |
| `2` | **Luck** | `Rate0`–`Rate1` | 0 = no luck, 1 = luck |
| `3` | **Additional option** (Jewel of Life level) | `Rate0`–`Rate7` | Option level N (+4 × N damage/defense, or +N% for some items) |
| `4` | **How many excellent options** | `Rate0`–`Rate6` | N excellent options, chosen at random among the item's 6 (wings, Dinorant, Fenrir have fewer) |
| `5` | **Ancient** | `Rate0`–`Rate6` | 0 = not ancient. 1, 3, 4 = the item's 1st set; 2, 5, 6 = its 2nd set. Odd values give +10 stat, even values +5 stat. |
| `6` | **How many sockets** | `Rate0`–`Rate5` | N empty sockets (only for socket items; capped by `SocketItemType.txt`) |

**Weights are relative.** The server adds up the weights of the row and picks one column in proportion. A row `7000 1500 1000 500` gives 70% / 15% / 10% / 5%. Weights of `0` (or less) are never picked. Rows don't need to add up to 10,000.

**Row meaning (shipped):**

| Row | Label | Used by |
|---|---|---|
| `0` | (Common) | Normal monster drops, and any `0` in an Option column |
| `1` | (Exc) | Excellent monster drops |
| `2` | (Set) | Ancient drops |
| `3` | (ChaosItemMix) | Chaos weapon mix |
| `4`–`8` | (Wing/Socket mixes) | Wing 1/2/3, monster wing, socket weapon mixes |
| `9` | (MossMerchant) | Moss Merchant gambling |
| `10` | (Lucky) | Lucky items |
| `11` and up | (EventItemBag) (...) | Ready-made rows for ItemDrop and bags. The label tells you the result, e.g. `(EventItemBag) (1)` = always 1, `(0~2)` = 0, 1 or 2 with equal chance. |

Socket section extra rows: `3` (OneHand) and `4` (TwoHand) are used by normal drops of one- and two-handed socket weapons.

### Example (real lines)

```
0
//Index   Rate0   Rate1   Rate2   Rate3   Rate4 ... Rate15   Comment
0         7000    1500    1000    500     0     ... 0        //Level Option (Common)
27        3334    3333    3333    0       0     ... 0        //Level Option (EventItemBag) (0~2)
end

1
//Index   Rate0   Rate1   Comment
0         9400    600     //Skill Option (Common)
12        0       10000   //Skill Option (EventItemBag) (1)
end
```

- Row `0` of section 0: monster drops come +0 (70%), +1 (15%), +2 (10%) or +3 (5%).
- Row `27`: +0, +1 or +2, one third each.
- Row `12` of section 1: always with skill.

### Rules & pitfalls

{% hint style="warning" %}
A row index that does not exist in the section (or `*`) means **"do not roll"**: the value stays as it is (level from the `Level` column, other options off). Row `0` exists and **does** roll — that is the classic EventItemBag trap.
{% endhint %}

- Changing row `0` changes **every** normal monster drop on the server.
- Add new rows with new indexes instead of editing the shipped `(EventItemBag)` rows other files already use.

---

## ItemOption.txt

### What it does

Defines **what an option gives** once an item has it: the skill flag, the luck bonus, the additional option, each excellent bit, and fixed bonuses of special items (Angel, rings, Fenrir...).

### Where it lives

`MuServer/Data/Item/ItemOption.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0–13+ | Option slot. `0` = skill, `1` = luck, `2` = additional option (JoL), `3`–`8` = excellent bits 1, 2, 4, 8, 16, 32, `9`+ = special fixed bonuses. |
| `OptionIndex` | option code | The effect (80–127, see the [option code list](#option-codes)). |
| `OptionValue` | number | Strength of the effect. For the additional option it is multiplied by the option level. |
| `ItemMinIndex` / `ItemMaxIndex` | item index | Item range this line applies to. |
| `ItemOption1` | 1 or `*` | Line applies only if the item has skill (`*` = don't care). |
| `ItemOption2` | 1 or `*` | Line applies only if the item has luck. |
| `ItemOption3` | 1+ or `*` | Line applies only if the item has at least this additional option level. |
| `ItemNewOption` | bit or `*` | Line applies only if the item has this excellent bit. |

### Example (real lines)

```
//Index   OptionIndex   OptionValue   ItemMinIndex   ItemMaxIndex   ItemOption1   ItemOption2   ItemOption3   ItemNewOption   Comment
1         84            5             0000           2559           *             1             *             *               //General Weapon
3         99            12            0000           2559           *             *             *             1               //General Weapon
```

- Luck on any weapon (indexes 0–2559): +5% critical damage rate (code 84).
- Excellent bit 1 on a weapon: code 99 ("Add Hunt MP") with value 12.

### Rules & pitfalls

- The client shows option texts from its own tables. Changing a value here changes the real effect but not the tooltip.

---

## ExcellentOptionRate.txt

A table of per-option weights by item range (`Index ItemMinIndex ItemMaxIndex Name Rate`). It is **not read** by this GameServer version: excellent options are chosen uniformly at random once ItemOptionRate section 4 (above) has decided how many. Leave the file as is.

---

## 380ItemOption.txt and 380ItemType.txt

### What they do

"380" items carry extra PvP options (attack success rate, additional damage, SD...). `380ItemOption.txt` lists the 8 options and their default values; `380ItemType.txt` says which items have which two options.

### Where they live

`MuServer/Data/Item/380ItemOption.txt`, `MuServer/Data/Item/380ItemType.txt`

### Syntax

**380ItemOption.txt**

| Column | Values | Meaning |
|---|---|---|
| `Index` | 1–8 | Option id. |
| `Name` | `"text"` | Name. |
| `Value` | number | Default value of the option. |

**380ItemType.txt**

| Column | Values | Meaning |
|---|---|---|
| `Section` | 0–15 | Item section. |
| `Type` | 0–511 | Item type. |
| `OptionIndex1` | 1–8 | First option (380ItemOption index). |
| `OptionValue1` | number or `*` | Value; `*` = use the default from 380ItemOption. |
| `OptionIndex2` | 1–8 | Second option. |
| `OptionValue2` | number or `*` | Value; `*` = default. |

### Example (real lines)

```
//Index Name                            Value
2       "Additional Damage"             200
...
//Section   Type   OptionIndex1   OptionValue1   OptionIndex2   OptionValue2
0           22     2              *              1              *
```

Item 0/22 has option 2 (Additional Damage, 200) and option 1 (attack success rate, 10).

---

## SetItemOption.txt and SetItemType.txt (ancient sets)

### What they do

`SetItemType.txt` says which items can be ancient and which set(s) they belong to. `SetItemOption.txt` defines the bonus of each set by number of pieces worn, and the full-set bonus.

### Syntax

**SetItemType.txt**

| Column | Values | Meaning |
|---|---|---|
| `Section` | 0–15 | Item section. |
| `Type` | 0–511 | Item type. |
| `StatType` | 0–4 | Stat that receives the ancient +5/+10: 0 Strength, 1 Agility, 2 Energy, 3 Vitality, 4 Command. |
| `OptionIndex1` | set id or 0 | 1st set (A) this item belongs to (index in SetItemOption). `0` = none. |
| `OptionIndex2` | set id or 0 | 2nd set (B). `0` = none. |

**SetItemOption.txt**

| Column | Values | Meaning |
|---|---|---|
| `Index` | set id | Set number (referenced by SetItemType). |
| `Name` | `"text"` | Set name. |
| `Option1`/`Value1` … `Option12`/`Value12` | code / value | Six tiers × two bonuses. Tier 1 (`Option1`, `Option2`) is active with 2 pieces, tier 2 (`Option3`, `Option4`) with 3 pieces, and so on. `*` = no bonus. |
| `FullOption1`/`FullValue1` … `FullOption5`/`FullValue5` | code / value | Extra bonuses when the **whole** set is worn. |

Set bonus codes: 0 Strength, 1 Agility, 2 Energy, 3 Vitality, 4 Command, 5 min. damage, 6 max. damage, 7 wizardry %, 8 damage, 9 attack rate %, 10 defense, 11 max. HP, 12 max. MP, 13 max. AG, 14 AG recovery, 15 critical rate, 16 critical damage, 17 excellent rate, 18 excellent damage, 19 skill damage, 20 double damage rate, 21 ignore defense rate, 22 shield defense %, 23 two-handed damage %, 24–28 damage/defense by stat, 29–35 elemental damage, 36 all stats.

### Example (real lines)

```
//Section   Type   StatType   OptionIndex1   OptionIndex2
0           32     0          37             38
```

Item 0/32 (Sacred Glove) can be ancient of set 37 or 38; the ancient stat bonus goes to Strength.

---

## SocketItemOption.txt and SocketItemType.txt

### What they do

`SocketItemType.txt` lists the socket items and their maximum sockets. `SocketItemOption.txt` defines seed-sphere options, bonus options (a matching element pattern on weapons/armor) and package options (counts of each element).

### Syntax

**SocketItemType.txt**

| Column | Values | Meaning |
|---|---|---|
| `Section` | 0–15 | Item section. |
| `Type` | 0–511 | Item type. |
| `MaxSocket` | 1–5 | Maximum sockets. |

**SocketItemOption.txt — section 0 (seed options)**

| Column | Values | Meaning |
|---|---|---|
| `Index` | number | Seed option id (this is the value stored in a socket). |
| `Type` | 1–6 | Element: 1 Fire, 2 Water, 3 Ice, 4 Wind, 5 Lightning, 6 Earth. |
| `SubIndex` | number | Option number inside the element. |
| `Name` | `"text"` | Name. |
| `Rate` | weight | Chance weight when a seed with this element is created. |
| `Value1`–`Value5` | number | Value by sphere level (Mono, Di, Tri, Tetra, Penta). |

**Section 1 (bonus options)**

| Column | Meaning |
|---|---|
| `Index`, `Name`, `Value` | Bonus id, name and value. |
| `MinItemSection` / `MaxItemSection` | Item sections that can get it (0–4 weapons, 5 staffs, 6–12 armor). |
| `ReqSocket1`–`ReqSocket5` | Element required in each socket, in order (0 = any). |

**Section 2 (package options)**

| Column | Meaning |
|---|---|
| `Index`, `Name`, `Value` | Package id, name and value. |
| `FireReq` `WaterReq` `IceReq` `WindReq` `LightReq` `EarthReq` | How many seeds of each element are needed. |

### Example (real lines)

```
0
//Index   Type   SubIndex   Name                                    Rate   Value1   Value2   Value3   Value4   Value5
1         1      1          "Attack speed increase"                 20     7        8        9        10       11
end
//Section   Type   MaxSocket
0           26     3
```

---

## JewelOfHarmonyOption.txt and JewelOfHarmonyType.txt

### What they do

`JewelOfHarmonyOption.txt` lists the options a Jewel of Harmony can add, by item group, with the value and restore cost at each refine level. `JewelOfHarmonyType.txt` lists items that can be **smelted** into Refining Stones and the minimum level for it.

### Syntax

**JewelOfHarmonyOption.txt** — section `1` = weapons, `2` = staffs, `3` = armor.

| Column | Values | Meaning |
|---|---|---|
| `Index` | 1–15 | Option id inside the group. |
| `Name` | `"text"` | Name. |
| `Rate` | weight | Chance weight when the jewel picks an option. |
| `Level` | 0–15 | Minimum item level for this option. |
| `Value0`/`Money0` … `Value15`/`Money15` | number / Zen | For refine level N: option value, and Zen cost to **restore** (remove) the option at that level. |

**JewelOfHarmonyType.txt**

| Column | Values | Meaning |
|---|---|---|
| `Section` | 0–15 | Item section. |
| `Type` | 0–511 | Item type. |
| `Level` | 0–15 | Minimum item level to smelt it into a Refining Stone. |

### Example (real lines)

```
1
//Index   Name                     Rate   Level   Value0   Money0   Value1   Money1 ...
1         "Min. attack increase"   50     0       2        100000   3        110000 ...
end
//Section   Type   Level
0           0      4
```

---

## PetItemOption.txt, EarringItemOption.txt, FlagItemOption.txt

### What they do

Give extra stats to custom pets/muuns, earrings and flags (nation flags). The three files share the same format.

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Type` | 0–15 | Item section. |
| `Index` | 0–511 | Item type. |
| `IncDmgRate` | % | Damage increase while equipped. |
| `DecDmgRate` | % | Damage reduction while equipped. |
| `Opt1`/`Val1` … `Opt4`/`Val4` | option code / value | Up to four extra options. `0` = none. |

### Example (real lines, PetItemOption.txt)

```
//Type	Index	IncDmgRate	DecDmgRate	Opt1	Val1	Opt2	Val2	Opt3	Val3	Opt4	Val4
12	83	0	0	119	50000	97	100	0	0	0	0
```

Item 12/83 gives +50000 HP (code 119) and +100 attack speed (code 97).

### Option codes

Used by `ItemOption.txt` (`OptionIndex`) and these three files:

| Code | Effect | Code | Effect |
|---|---|---|---|
| 80 | Add physical damage | 104 | Mul BP |
| 81 | Add magic damage | 105 | Add wing command |
| 82 | Add defense success rate | 106–108 | Fenrir options 1–3 |
| 83 | Add defense | 109 | Full damage reflect rate |
| 84 | Add critical damage rate | 110 | Full HP restore rate |
| 85 | Add HP recovery rate | 111 | Full MP restore rate |
| 86 | Add Zen drop rate | 112 | Dinorant option |
| 87 | Mul defense success rate | 113 | Add curse damage |
| 88 | Add damage reflect | 114 | Add curse damage by level |
| 89 | Add damage reduction | 115 | Mul curse damage |
| 90 | Mul MP | 116 | Add double damage rate |
| 91 | Mul HP | 117 | Add experience rate |
| 92 | Add excellent damage rate | 118 | Add combo experience rate |
| 93 | Add physical damage by level | 119 | Add HP |
| 94 | Mul physical damage | 120 | Add MP |
| 95 | Add magic damage by level | 121 | Add HP by level |
| 96 | Mul magic damage | 122 | Add MP by level |
| 97 | Add speed | 123 | Add damage |
| 98 | Add HP on kill (hunt HP) | 124 | Add damage by level |
| 99 | Add MP on kill (hunt MP) | 125 | Mul damage |
| 100 | Add wing HP | 126 | Add defense by level |
| 101 | Add wing MP | 127 | Mul defense |
| 102 | Add ignore defense rate | | |
| 103 | Add BP | | |

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md). The [EventItemBag editor](../../dashboard/editors/event-item-bag.md) shows the ItemOptionRate rows by name when you pick them for a bag.

## Reload module

```
/reload item
```

{% hint style="warning" %}
`/reload all` does not reload item files. Use `/reload item`, or restart the GameServer for structural changes.
{% endhint %}
