# Custom Jewel

New jewels that change an item when dropped on it: add levels, skill, luck, additional option, excellent options, ancient options or socket slots.

## What it is

The player drags a custom jewel onto an equipped-type item (weapon, armor, wing) in the inventory, just like a Jewel of Bless. If the item qualifies, the jewel is consumed and, depending on its success rate, the item gains what the jewel gives, or loses what you set for a failure.

The shipped server file defines nine jewels:

| Jewel | Item | Effect on success |
|---|---|---|
| Jewel of Level | 14,244 | +1 level (items +9..+12) |
| Jewel of Skill | 14,245 | Adds the skill option |
| Jewel of Luck | 14,246 | Adds luck |
| Jewel of Additional | 14,247 | Additional option to the maximum |
| Jewel of Excellent | 14,248 | +1 random excellent option |
| Jewel of Socket | 14,249 | +1 empty socket slot |
| Jewel of Ancient 1 / 2 | 14,250 / 14,251 | Turns the item into ancient set A or B, +5 stat |
| Jewel of Full | 14,252 | +15 levels, skill, luck, full additional, 6 excellent options |

## Enable it

There is no on/off key: every jewel listed in the file works. Related keys:

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Common.dat` | `MaxItemOption` | `4` | Cap of the additional option a jewel can reach. |
| `GameServerInfo - Common.dat` | `JewelSocketPentagram` | `0` (not in the shipped file) | `1` = the socket jewel also works on pentagrams with fewer than 5 slots. |

## Files & syntax

| Side | File |
|---|---|
| Server | `Data/Custom/CustomJewel.txt` |
| Client | Client custom file `CustomJewel.txt` in the Client Hub (see [Client custom files](../client/custom-files.md)) |

The server file has three sections. Up to 15 jewels (`Index` 0..14). Sections 1 and 2 are linked to section 0 by `Index`.

### Section 0: jewel and target rules

| Column | Meaning | Values |
|---|---|---|
| `Index` | Jewel number | 0..14 |
| `ItemIndex` | The jewel item | `Type * 512 + Index` (14,244 = 7412) |
| `MinItemLevel` / `MaxItemLevel` | Target item level range | 0..15, `*` = any |
| `MaxItemOption1` | Target must have skill at most this. Also requires the item to have a skill | `0` = only items without skill, `*` = any |
| `MaxItemOption2` | Target luck at most this | `0` = only items without luck, `*` = any |
| `MinItemOption3` / `MaxItemOption3` | Target additional option range | 0..7, `*` = any |
| `MinItemNewOption` | Target must have at least this many excellent options | Number, `*` = any |
| `MaxItemNewOption` | Below 10: target may have at most this many excellent options. 10 or more: the target must **not** have the option `value - 10` (bitmask) | Number, `*` = any |
| `MaxItemSetOption` | Any number blocks items that are already ancient. Bit 1 (`1`) = item must have ancient set A available, bit 2 (`2`) = set B available | `*` = any |
| `MinItemSocketOption` / `MaxItemSocketOption` | Socket rules: `0` = must be a socket item; N = at least / at most N sockets on the item. The maximum also requires the item to be below its socket limit | Number, `*` = any |
| `EnableSlotWeapon` | Works on weapons (types 0..5) | `0` = no, `1` / `*` = yes |
| `EnableSlotArmor` | Works on armor and shields (types 6..11) | `0` = no, `1` / `*` = yes |
| `EnableSlotWing` | Works on wings, capes and custom wings | `0` = no, `1` / `*` = yes |
| `SuccessRate_AL0` .. `SuccessRate_AL3` | Success chance per account level (free, VIP 1..3) | 0..100 % |
| `SalePrice` | Zen value of the jewel (shop price / sell value) | Zen |
| `ModelName` | Model name of the jewel | Text in quotes. Keep it equal to the client file |

### Section 1: what success adds

| Column | Meaning | Values |
|---|---|---|
| `Index` | Jewel number from section 0 | 0..14 |
| `Level` | Levels added (capped at +15) | Number |
| `Option1` | Adds skill | 0 / 1 |
| `Option2` | Adds luck | 0 / 1 |
| `Option3` | Additional option levels added (capped by `MaxItemOption`) | Number |
| `NewOption` | Below 10: number of random excellent options added. 10 or more: adds the exact bitmask `value - 10` | Number |
| `SetOption` | Ancient bitmask: `1` = set A, `2` = set B, `4` = +5 stat, `8` = +10 stat | Sum of bits (`5` = set A +5) |
| `SocketOption` | Empty socket slots added | Number |

### Section 2: what failure removes

Same columns as section 1. On failure, `Level`, `Option1`, `Option2` and `Option3` are **subtracted** (never below 0). The other columns are ignored on failure. An empty section 2, as shipped, means a failed jewel only wastes the jewel.

## Example

```
0
//Index ItemIndex MinLvl MaxLvl MaxOpt1 MaxOpt2 MinOpt3 MaxOpt3 MinExc MaxExc MaxSet MinSock MaxSock Weapon Armor Wing Rate0 Rate1 Rate2 Rate3 SalePrice ModelName
  0     7412      9      12     *       *       *       *       *      *      *      *       *       1      1     *    60    70    80    90    20        "Jewel180"   // Jewel of Level
  4     7416      9      15     *       *       *       *       0      5      0      *       *       1      1     *    50    60    70    80    20        "Jewel184"   // Jewel of Excellent
end

1
//Index Level Option1 Option2 Option3 NewOption SetOption SocketOption
  0     1     0       0       0       0         0         0     // +1 level
  4     0     0       0       0       1         0         0     // +1 random excellent option
end

2
//Index Level Option1 Option2 Option3 NewOption SetOption SocketOption
  0     1     0       0       0       0         0         0     // failure: -1 level
end
```

With this, the Jewel of Level works on +9..+12 weapons and armor, succeeds 60% for free accounts and 90% for VIP 3, and costs one level on failure.

## Pitfalls

* **The client file must describe the same jewels.** The client decides whether the jewel can be dragged onto an item and how it looks; the server decides the result. The client file uses separate `Type` and `Index` columns and has no `Index` (row number) column. If the rules differ, players see "can apply" and the server refuses, or the opposite. The shipped files already differ (for example the client lets the Jewel of Level start at +0, the server at +9). Align them before going live, then publish the client update from the Client Hub.
* The jewel item itself must exist in `Data/Item/Item.txt` and in the client.
* `MaxItemOption1` set to `0` or higher also rejects items that have no skill at all, which is what you want for a Jewel of Skill.
* Time-limited items cannot receive a custom jewel.
* Lucky items only accept custom jewels if the lucky-item lock is released in the server settings.
* `SuccessRate` above 100 behaves as 100.

## Reload module

* `/reload custom` re-reads `Data/Custom/CustomJewel.txt`. The new rules apply to the next jewel used.
* The client file never hot-reloads: after changing the client custom file `CustomJewel.txt`, it must be [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).
