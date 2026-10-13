# Items

This page covers the files that **define items** and how they can be moved, stacked, priced and used. Options (excellent, ancient, socket, harmony, 380...) are on [Item options](item-options.md); drop rules are on [Item drop](item-drop.md).

| File | What it controls |
|---|---|
| `Item/Item.txt` | Every item: size, requirements, damage/defense, which classes can use it |
| `Item/ItemValue.txt` | Buy/sell price overrides (Zen and coins) |
| `Item/ItemValueTrade.txt` | Extra fee charged when an item is put in a trade |
| `Item/ItemMove.txt` | Which items can be dropped, sold, traded or stored |
| `Item/ItemStack.txt` | Which items stack, how many, and what a full stack turns into |
| `Item/ItemTicketVip.txt` | Items that grant VIP days when used |
| `Item/LuckyItem.txt` | Lucky items created by Lucky Tickets |
| `Item/JewelOfStore.txt` | Jewel bank: which jewels can be deposited and how they are bundled |
| `Item/ItemMakeColor.txt` | Legacy color table (not read by this GameServer) |

{% hint style="info" %}
**Item index.** Many files write an item as one number: `Index = Section × 512 + Type`. Example: Jewel of Bless is section 14, type 13 → `14 × 512 + 13 = 7181`. Files that use two columns (`Section` `Type`) say so in their header.
{% endhint %}

---

## Item.txt

### What it does

The master list of items. Anything not in this file does not exist for the server: it cannot drop, be sold, be created by commands or by bags.

### Where it lives

`MuServer/Data/Item/Item.txt`

{% hint style="danger" %}
The client has its own item table (`Item.bmd`). Names, sizes, requirements and class flags must match on both sides. Adding a brand-new item needs client work too. Changing numbers (damage, requirements) only on the server makes the client show wrong values.
{% endhint %}

### Syntax

The file has one block per item section (`0` to `15`). Each block starts with the section number on its own line, a `//` header, the item lines and `end`.

| Section | Items |
|---|---|
| 0 | Swords and gloves (Rage Fighter) |
| 1 | Axes |
| 2 | Maces and scepters |
| 3 | Spears |
| 4 | Bows, crossbows, arrows, bolts |
| 5 | Staffs, sticks, books |
| 6 | Shields |
| 7 | Helms |
| 8 | Armors |
| 9 | Pants |
| 10 | Gloves |
| 11 | Boots |
| 12 | Wings, capes, orbs, misc. jewels and bundles |
| 13 | Pets, rings, pendants, tickets, misc. |
| 14 | Potions, jewels, boxes, event items |
| 15 | Scrolls (skill books) |

**Base columns (all sections)**

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0–511 | Item type inside the section. |
| `Kind1` | code | Main category (1 weapon, 2 pendant, 3 armor, 4 ring, 6 wings, 9 jewel, 10 skill scroll...). Copy from a similar item. |
| `Kind2` | code | Sub-category (sword, staff, shield, helm...). Copy from a similar item. |
| `Kind3` | code | Tier/special group. Copy from a similar item. |
| `Slot` | -1, 0–11 | Equipment slot: 0 right hand, 1 left hand, 2 helm, 3 armor, 4 pants, 5 gloves, 6 boots, 7 wings, 8 pet, 9 pendant, 10/11 rings. `-1` = cannot be equipped. |
| `Width` / `Height` | 1–4 | Size in inventory slots. |
| `Drop` | 0 / 1 | `1` = can be a **normal random monster drop** (see [Item drop](item-drop.md#how-a-monster-drop-is-decided)). `0` = only from bags, shops, mixes, commands. |
| `SkillIndex` | skill id | Skill the item gives (weapons with skill, skill scrolls). `0` = none. |
| `Value` | number | Base value used by the price formula when the item has no `ItemValue.txt` row. |
| `iZen` | Zen | Fixed buy price used by some items (scrolls, special items). |
| `Name` | `"text"` | Item name (quoted). |

**Extra columns per section**

{% tabs %}
{% tab title="Weapons (0–5)" %}
| Column | Meaning |
|---|---|
| `TwoHand` | `1` = two-handed. |
| `Level` | Drop level: monsters around this level drop it (see Item drop). |
| `PowerATTK` | Reserved; keep as shipped. |
| `DamageMin` / `DamageMax` | Base damage. |
| `WeaponSpeed` | Attack speed bonus. |
| `Durability` | Max durability. |
| `MagicDurability` | Durability for staffs, sticks, books (replaces `Durability` for them). |
| `Magicpower` | Wizardry/curse damage rate. |
| `ReqLevel` `ReqStrength` `ReqDexterity` `ReqEnergy` `ReqVitality` `ReqCharisma` | Requirements to equip (`ReqCharisma` = Command). |
{% endtab %}
{% tab title="Shields, armor (6–11)" %}
| Column | Meaning |
|---|---|
| `Level` | Drop level. |
| `PowerATTK` | Reserved; keep as shipped. |
| `Defense` | Base defense. |
| 4th column | Depends on the section: **6** shields = `SuccessfulBlocking` (defense rate); **7–9** helms/armors/pants = `MagicDefense`; **10** gloves = `AttackSpeed`; **11** boots = `WalkSpeed`. |
| `Durability` | Max durability. |
| `ReqLevel` … `ReqCharisma` | Requirements. |
{% endtab %}
{% tab title="Wings (12)" %}
| Column | Meaning |
|---|---|
| `Level` | Drop level. |
| `PowerATTK` | Reserved. |
| `Defense` | Base defense. |
| `Durability` | Max durability. |
| `DamageMin` / `DamageMax` | Damage values used by some items of this section. |
| `ReqLevel` … `ReqCharisma` | Requirements. |
{% endtab %}
{% tab title="Pets, rings (13)" %}
| Column | Meaning |
|---|---|
| `ReqLevel` | Level required. |
| `Durability` | Max durability (life for pets). |
| `IceRes` `PoisonRes` `LightRes` `FireRes` `EarthRes` `WindRes` `WaterRes` | Elemental resistances. |
{% endtab %}
{% tab title="Consumables (14)" %}
| Column | Meaning |
|---|---|
| `DropLevel` | Drop level. |

Section 14 has **no** class columns.
{% endtab %}
{% tab title="Scrolls (15)" %}
| Column | Meaning |
|---|---|
| `Level` | Drop level. |
| `ReqLevel` | Level required to learn. |
| `ReqEnergy` | Energy required to learn. |
{% endtab %}
{% endtabs %}

**Class columns (every section except 14)** — 15 columns, in this order:

| Column | Class |
|---|---|
| `DW` | Dark Wizard |
| `DK` | Dark Knight |
| `FE` | Fairy Elf |
| `MG` | Magic Gladiator |
| `DL` | Dark Lord |
| `SM` | Summoner |
| `RG` | Rage Fighter |
| `GL` | Grow Lancer |
| `RW` | Rune Wizard |
| `SL` | Slayer |
| `GC` | Gun Crusher |
| `KN` | Light Wizard |
| `LM` | Lemuria Mage |
| `IK` | Illusion Knight |
| `AQ` | Alchemist |

The last eight columns (GL to AQ) are for later-season classes. On a Season 6 server only the first seven matter, but every line must still have all 15 values.

| Value | Meaning |
|---|---|
| `0` | This class cannot use the item. |
| `1` | Usable from the base class. |
| `2` | Requires the 2nd class (e.g. Blade Knight). |
| `3` | Requires the 3rd class (e.g. Blade Master). |

### Example (real lines, section 0)

```
//Index  Kind1  Kind2  Kind3  Slot  Width  Height  Drop  SkillIndex  Value  iZen  Name           TwoHand  Level  PowerATTK  DamageMin  DamageMax  WeaponSpeed  Durability  MagicDurability  Magicpower  ReqLevel  ReqStrength  ReqDexterity  ReqEnergy  ReqVitality  ReqCharisma  DW DK FE MG DL SM RG GL RW SL GC KN LM IK AQ
0        1      1      1      0     1      2       1     0           0      0     "Kris"         0        6      0          6          11         50           20          0                0           0         40           40            0          0            0            1  1  1  1  1  1  1  1  1  1  0  0  1  1  0
17       1      1      1      0     2      4       0     23          0      0     "Dark Breaker" 1        104    0          128        153        40           89          0                0           0         180          50            0          0            0            0  2  0  0  0  0  0  0  0  0  0  0  0  0  0
```

"Dark Breaker" is two-handed, never drops at random (`Drop 0`), gives skill 23 and can only be used by a Dark Knight **after the 2nd class change** (`DK = 2`).

### Rules & pitfalls

- Keep the exact column count of the section. A missing value shifts every item after it.
- Names must stay quoted.
- Changing `Width`/`Height` without changing the client corrupts inventories visually.
- Removing an item that players own makes those items disappear from the game.

---

## ItemValue.txt

### What it does

Fixes the price of specific items instead of the automatic formula. Also lets NPC shops sell an item for **coins** (WCoinC, WCoinP, Goblin Points) instead of Zen.

### Where it lives

`MuServer/Data/Item/ItemValue.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | item index | Item (`Section × 512 + Type`). |
| `Level` | 0–15 or `*` | Exact item level, or `*` for any. |
| `OpExe` (Grade) | 0–63 or `*` | Excellent options mask. `*` = any; `0` = **no** excellent; `N` = that **exact** mask. |
| `Zen Value` | Zen | Buy price in Zen. |
| `Coin1` | number (optional) | Price in coin 1 (WCoinC). |
| `Coin2` | number (optional) | Price in coin 2 (WCoinP). |
| `Coin3` | number (optional) | Price in coin 3 (Goblin Points). |
| `SellValue` | Zen (optional) | Price the NPC pays when the player sells. `0` = Value / 3. |

When several lines match, the **most specific** wins (exact level beats `*`, exact grade beats `*`). On a tie, the first line in the file wins.

### Example (real lines)

```
//Index   Level   OpExe   Zen Value   Coin1   Coin2   Coin3   SellValue   Comment
6687      *       0       10000000    0       0       0       500000      //Spirit DL (Dark Horse)
6687      1       0       10000000    0       0       0       500000      //Spirit of Dark Raven
7179      8       *       1500        0       0       0       500         // Uniria +8
```

### Rules & pitfalls

- `OpExe 1` does **not** mean "excellent yes". It only matches an item whose excellent mask is exactly `1`. Use `*` for "any".
- Lines with an invalid index, level above 15, grade above 63 or a negative value are skipped with a red log line.
- The Coin columns are optional: a short line does not shift the next lines.
- After `/reload shop` the prices of items already in players' inventories are recalculated.

---

## ItemValueTrade.txt

### What it does

Charges a **fee** to the player who puts a listed item in a trade window. When the trade is confirmed, each side pays Zen/coins for every listed item they are giving. If they can't pay, the trade is cancelled with a message.

### Where it lives

`MuServer/Data/Item/ItemValueTrade.txt` (shipped empty)

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | item index | Item (`Section × 512 + Type`). |
| `Zen` | Zen | Fee in Zen. |
| `Coin1` | number | Fee in coin 1 (WCoinC). |
| `Coin2` | number | Fee in coin 2 (WCoinP). |
| `Coin3` | number | Fee in coin 3 (Goblin Points). |

For **stackable** items the fee is multiplied by the stack size.

### Example

```
//Index   Zen      Coin1   Coin2   Coin3   Comment
7181      100000   0       0       0       //Jewel of Bless: 100k Zen per jewel traded
end
```

---

## ItemMove.txt

### What it does

Blocks actions for specific items. Items **not** listed are allowed everywhere.

### Where it lives

`MuServer/Data/Item/ItemMove.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | item index | Item (`Section × 512 + Type`). |
| `AllowDrop` | 0 / 1 | `0` = cannot be dropped on the ground. |
| `AllowSell` | 0 / 1 | `0` = cannot be sold to NPCs. |
| `AllowTrade` | 0 / 1 | `0` = cannot be traded or put in a personal store. |
| `AllowVault` | 0 / 1 | `0` = cannot be stored in the warehouse. |

### Example (real lines)

```
//Index   AllowDrop   AllowSell   AllowTrade   AllowVault   Comment
6144      0           1           1            1            //Wings of Elf
6159      0           1           1            1            //Jewel of Chaos
```

Wings of Elf and Jewel of Chaos cannot be thrown on the ground, but can be sold, traded and stored.

---

## ItemStack.txt

### What it does

Defines which items stack in one slot, the stack limit, and optional "auto-craft": when a stack reaches the limit it turns into another item (e.g. 5 Symbols of Kundun → Lost Map).

### Where it lives

`MuServer/Data/Item/ItemStack.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | item index | Item (`Section × 512 + Type`). |
| `Level` | 0–15 or `-1` | Item level the rule applies to. `-1` = any level. |
| `MaxStack` | 1–255 | Maximum quantity in one slot. |
| `CreateItemIndex` | item index or `-1` | Item created when the stack reaches `MaxStack`. `-1` = nothing. |
| `BreakUp` | 0 / 1 | `1` = players can split one unit off the stack. |

### Example (real lines)

```
//Index  Level  MaxStack  CreateItemIndex  BreakUp  //Comment
2063     -1     255       -1               0        //Arrow
7197     -1     5         7196             0        //Symbol of Kundun
7268     -1     255       -1               0        //Lucky Coin
```

Five Symbols of Kundun of the same level become item 7196 (Lost Map) automatically.

### Rules & pitfalls

- The client must also accept the stack (quantity display). Very large stacks of items the client does not consider stackable show wrong numbers.
- Jewels that use the jewel bank (JewelOfStore.txt, below) or bundles should not get a `CreateItemIndex` that conflicts with the bundle mix.

---

## ItemTicketVip.txt

### What it does

Turns an item into a **VIP ticket**: when the player uses it, the account receives VIP days.

### Where it lives

`MuServer/Data/Item/ItemTicketVip.txt` (shipped with only a commented example)

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Type` | 0–15 | Item section. |
| `Index` | 0–511 | Item type. |
| `Level` | 0–15 or `-1` | Required item level; `-1` = any. |
| `VipLevel` | 0–3 | VIP level granted: 0 Free, 1 Bronze, 2 Silver, 3 Gold. |
| `Days` | number | Days added. |
| `description` | `"text"` | Text shown to the player. |

### Example (shipped, commented)

```
//Type  Index   Level   VipLevel Days   description
//14      15      -1      1        5      "5 dias de vip bronce"
end
```

Remove the `//` to activate: item 14/15 at any level gives 5 days of Bronze VIP.

---

## LuckyItem.txt

### What it does

Lists the **Lucky items** (armor pieces created by the Lucky Ticket mix in the Chaos Machine), their durability wear and the option rolls they get.

### Where it lives

`MuServer/Data/Item/LuckyItem.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | item index | The lucky item. |
| `Group` | 0 / 1 | Ticket group. Each Lucky Ticket creates items of its group in the same section (helm ticket → helm) that the player's class can use. |
| `Decay` | number | Durability wear factor of the lucky item (higher = lasts longer). |
| `Option0`–`Option6` | ItemOptionRate row | Rolls for level, skill, luck, JoL, excellent, ancient, socket — row index in [ItemOptionRate.txt](item-options.md). Row `10` = "(Lucky)". |

### Example (real lines)

```
//Index   Group   Decay   Option0   Option1   Option2   Option3   Option4   Option5   Option6   Comment
3646      0       2400    10        10        10        10        10        10        10        //Scale Helm
3650      1       2400    10        10        10        10        10        10        10        //Dark Soul Helm
```

---

## JewelOfStore.txt

### What it does

Configures the **jewel bank**: which jewels players can deposit into their account balance, and how they withdraw them (loose or as bundles of 10/20/30).

### Where it lives

`MuServer/Data/Item/JewelOfStore.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Jewel` | item index | The jewel that can be deposited. |
| `Level` | 0–15 or `*` | Required level of the jewel; `*` = any. |
| `Bundled` | item index or `-1` | Bundle item used when withdrawing in packs (e.g. `6174` = Bundled Jewel of Bless). |
| `Unit` | number | How many bank units one loose jewel is worth. |
| `Pack1` / `Pack2` / `Pack3` | number | Jewels in a bundle of level +0 / +1 / +2. |
| `Name` | `"text"` | Display name. |

### Example (real line)

```
//Jewel  Level  Bundled  Unit  Pack1  Pack2  Pack3 Name
7181     *      6174     1     10     20     30    "Jewel of Bless"
```

### Rules & pitfalls

- `Pack1`–`Pack3` must match how many jewels the client/bundle mix uses (10/20/30 by default), or players can withdraw more than they deposited.

---

## ItemMakeColor.txt

Legacy table (item list in section 0 and a 14-color RGB palette in section 1). It is **not read** by this GameServer version. You can leave it as is.

---

## Edit in the panel

There is no dedicated visual editor for these files. Use the [File editor](../../dashboard/file-editor.md), which checks the syntax before saving.

## Reload module

| Files | Reload |
|---|---|
| `Item.txt`, `ItemMove.txt`, `ItemStack.txt`, `ItemTicketVip.txt`, `LuckyItem.txt`, `JewelOfStore.txt` | `/reload item` |
| `ItemValue.txt` | `/reload shop` (also `/reload item`) |
| `ItemValueTrade.txt` | `/reload custom` |

{% hint style="warning" %}
`/reload all` deliberately **skips** the item tables. After adding or removing items in `Item.txt`, restart the GameServer instead of relying on a reload.
{% endhint %}
