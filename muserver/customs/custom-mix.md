# Custom Mix

Your own Chaos Machine recipes: required items, Zen cost, success rate per VIP level, and a prize drawn from an item bag.

## What it is

The player puts the ingredients in the Chaos Machine and presses Mix, like any MU combination. If the ingredients match your recipe and the player has the Zen, the server rolls the success rate. On success the ingredients are replaced by a prize drawn from an event item bag. On failure the ingredients are lost.

## Enable it

There is no on/off key. A recipe is active as soon as it is in `CustomMix.txt`.

## Files & syntax

File: `Data/Custom/CustomMix.txt`. Three sections, linked by `Index`.

`Index` is the **mix type number** that the client sends when the player presses Mix. A custom recipe with the same number as a built-in Chaos Machine mix replaces it.

### Section 0: recipe

| Column | Meaning | Values |
|---|---|---|
| `Index` | Mix type number | 1 or higher |
| `MixMoney` | Zen cost. The Castle Siege chaos tax is added on top | Zen |
| `MixRate_AL0` .. `MixRate_AL3` | Success rate per account level (free, VIP 1..3) | 0..100 % |

### Section 1: ingredients

One row per required item. All rows with the same `Index` must be satisfied.

| Column | Meaning | Values |
|---|---|---|
| `Index` | Mix type number from section 0 | 1 or higher |
| `Group` | Read but not used by this version | Any number (keep `0`) |
| `Count` | How many items matching this row must be in the Chaos Machine | Number |
| `ItemIndex` | Item required | `Type * 512 + Index` (13,64 = 6720) |
| `ItemLevel` | Exact level required | 0..15, `-1` = any |
| `ItemOption1` | Exact skill option | 0 / 1, `-1` = any |
| `ItemOption2` | Exact luck option | 0 / 1, `-1` = any |
| `ItemOption3` | Exact additional option | 0..7, `-1` = any |
| `ItemNewOption` | At least this many excellent options | Number, `-1` = any |
| `ItemSetOption` | Any value 0 or higher requires an ancient item | `-1` = any |

### Section 2: prize

| Column | Meaning | Values |
|---|---|---|
| `Index` | Mix type number from section 0 | 1 or higher |
| `Group` | Read but not used by this version | Any number (keep `0`) |
| `BagSpecial` | Event item bag that gives the prize | The `SpecialValue` of a row in `Data/EventItemBagManager.txt` |

## Example

```
0
//Index   MixMoney   MixRate_AL0   MixRate_AL1   MixRate_AL2   MixRate_AL3
12	  1000000    60            70            80            90
end

1
//Index   Group   Count   ItemIndex   ItemLevel   ItemOption1   ItemOption2   ItemOption3   ItemNewOption   ItemSetOption
12	  0	  2	  6720	      -1	  -1		-1	      -1	    -1		    -1     // 2x item 13,64
12	  0	  2	  6721	      -1	  -1		-1	      -1	    -1		    -1     // 2x item 13,65
end

2
//Index   Group   BagSpecial
12	  0	  200      // prize from the bag whose SpecialValue is 200
end
```

The bag with `SpecialValue = 200` must exist in `Data/EventItemBagManager.txt`, pointing to an item bag file in `Data/EventItemBag/`.

## Rewards

One item drawn from the event item bag named by `BagSpecial`, created in the Chaos Machine. Level, options and chance come from that bag. The Morpheus control panel has a visual editor for event item bags.

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload custom` | Admin | Re-reads `CustomMix.txt`. |
| `/reload eventitembag` | Admin | Re-reads the item bags used as prizes. |

## Pitfalls

* **The shipped recipe has no prize.** It points to `BagSpecial = 200`, and the shipped `EventItemBagManager.txt` has no bag with `SpecialValue = 200`. A "successful" mix then ends as a failure message, with the Zen already taken. Create the bag or change the number before using it.
* The mix type number is decided by the client's recipe list. Use a number the client actually sends for your ingredients; a number no client recipe produces is never triggered. A number already used by a standard mix (for example 1 = Chaos Weapon, 7 = Wings level 1) replaces that standard mix for everyone.
* Section 1 rows with `Index` 0 or below are ignored, so a recipe with index 0 has no ingredient check at all.
* Extra items in the Chaos Machine are not rejected. They are consumed with the mix.
* Only the first section 2 row of an `Index` is used.
* Success rates above 100 count as 100.

## Reload module

* `/reload custom` re-reads `Data/Custom/CustomMix.txt`.
* `/reload eventitembag` re-reads the prize bags.
* No client change is needed unless you need a new recipe in the client's recipe list.
