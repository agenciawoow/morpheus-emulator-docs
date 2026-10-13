# Moss Merchant

Scheduled gamble merchant: Moss appears in Elbeland for a short time and sells mystery weapons whose options are rolled when bought.

## What it is

* At the scheduled time a global notice says "The Moss Merchant has appeared!" and Moss (monster class 492) spawns in **Elbeland (map 51) at 22, 225**.
* Players buy one of his five gamble items (items 13,71 to 13,75: Sword/Mace/Spear, Staff, Bow/Crossbow, Scepter, Stick). The server immediately rolls a real weapon of that type, with random level, luck, skill, options and excellent options.
* When the time is over, "The Moss Merchant has left!" is shown and he disappears.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `MossMerchantEvent` | `0` | `1` = Moss appears on his schedule |
| `GameServerInfo - Event.dat` | `MossMerchantEventTime` | `120` | How long Moss stays, in **seconds** (`120` = 2 minutes) |

## Files & syntax

`Data/Event/MossMerchant.dat`, 2 sections. **Both sections are commented out in the shipped file.**

### Section 0 - Schedule

`Year Month Day DoW Hour Minute Second`, see [Schedule syntax](schedule-syntax.md).

### Section 1 - Gamble items

| Column | Meaning | Values |
|---|---|---|
| Index | Item that can be rolled, as `section x 512 + index` (e.g. `5` = Blade, `517` = Battle Axe, `1024` = Mace) | item code |
| Group | Which gamble item rolls it | `0` = Sword/Mace/Spear (13,71), `1` = Staff (13,72), `2` = Bow/Crossbow (13,73), `3` = Scepter (13,74), `4` = Stick (13,75) |
| Option0 | Row of section 0 (level) in `Data/Item/ItemOptionRate.txt` | row index, shipped `9` |
| Option1 | Row of section 1 (skill) | row index |
| Option2 | Row of section 2 (luck) | row index |
| Option3 | Row of section 3 (additional option) | row index |
| Option4 | Row of section 4 (excellent options) | row index |
| Option5 | Row of section 5 (ancient / set) | row index |
| Option6 | Row of section 6 (sockets) | row index |
| GambleRate | Weight of this item inside its group. Chance = GambleRate / sum of the group's rates | number |
| Comment | Free text after `//` | |

`ItemOptionRate.txt` row `9` of each section is reserved for Moss ("(MossMerchant)" comments): for example level row 9 is `2500 2000 1500 1500 1000 500 500 500` (chance per +0 ... +7), luck row 9 is `9500 500` (5% luck).

## Example

```
0
//Year   Month   Day   DoW   Hour   Minute   Second
*        *       *     *     14     0        0          // every day 14:00
*        *       *     *     20     0        0          // every day 20:00
end

1
//Index   Group   Option0   Option1   Option2   Option3   Option4   Option5   Option6   GambleRate   Comment
5         0       9         9         9         9         9         9         9         1633         //Blade
517       0       9         9         9         9         9         9         9         1633         //Battle Axe
1024      0       9         9         9         9         9         9         9         1633         //Mace
end
```

With `MossMerchantEventTime = 1800` in `GameServerInfo - Event.dat`, Moss stays 30 minutes.

## Schedule

Section 0 uses the shared format, see [Schedule syntax](schedule-syntax.md). The shipped rows (02:00, 08:00, 14:00, 20:00) are commented out.

```
0
//Year   Month   Day   DoW   Hour   Minute   Second
//*        *       *     *     2      0        0
//*        *       *     *     8      0        0
end
```

## Rewards

The reward is the rolled weapon itself. The player needs a free 2 x 4 space in the inventory. The gamble items and their prices are in Moss's shop: shop 19 in `Data/ShopManager.txt` (NPC 492), file `Data/Shop/019 - Moss Merchant.txt`.

## Commands related

There is no command to call Moss.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | Re-reads this file and the keys, recalculates the next appearance |

## Pitfalls

* `MossMerchantEventTime` is in **seconds**, not minutes. The shipped `120` keeps Moss for only 2 minutes.
* Turning on `MossMerchantEvent` is not enough: uncomment the schedule and the item list.
* A group with no items makes that gamble item fail: the purchase is refused and no Zen is taken. Make sure each gamble item sold in the shop has at least one row in its group.
* Changing row 9 of `ItemOptionRate.txt` also changes any other system that points at row 9.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `MossMerchant.dat` and recalculates the next appearance. `ItemOptionRate.txt` reloads with `/reload item`, Moss's shop with `/reload shop`.

{% hint style="warning" %}
`/reload event` resets Moss to idle: if he is in Elbeland, he leaves. Reload while he is not there.
{% endhint %}
