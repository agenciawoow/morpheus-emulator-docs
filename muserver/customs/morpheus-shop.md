# Morpheus Shop

The premium in-game store where players build the item (level, luck, skill, options, excellent, socket, harmony) before buying it with WCoinC, WCoinP or GoblinPoint.

## What it is

The Morpheus Shop replaces the legacy Cash Shop window with a modern full-screen store. The player browses the catalog by category, picks an item and, before paying, customizes it: item level, luck, skill, additional option, +380 option, excellent options, sockets and harmony, always inside the limits you define. They buy exactly what they want, and you control exactly what they are allowed to build. Each option adds an extra cost on top of the base price.

Effect items (seals, scrolls, elixirs, auras: entries of `Data/Effect.txt` with Type 2) work like the original cash shop: buying one **activates the buff** for 1, 7 or 30 days instead of creating an item in the inventory. The button shows "Activate" instead of "Buy".

The catalog shown to the player is the intersection of two files:

| Side | File | Decides |
|---|---|---|
| Client | Client custom file `CustomMorpheusShop.txt` (Client Hub) | **Which** items appear, in which tab, and how the 3D preview is framed |
| Server | `Data/Custom/CustomMorpheusShop.txt` | **Prices**, option prices, option limits, discounts and texts |

An item listed on the client without a price on the server does not show a price and cannot be bought. A priced item that is not listed on the client never appears. See [client custom files](../client/custom-files.md) for the client file.

{% hint style="info" %}
The Morpheus control panel has a visual editor for this catalog.
{% endhint %}

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomMorpheusShopSwitch` | `1` | `1` = purchases go through the Morpheus Shop. `0` = every purchase is refused (result 4) and the legacy Cash Shop is used. |
| Client settings (Client Hub) | `CustomMorpheusShopSwitch` | `1` | `1` = the client opens the new shop window instead of the legacy one. |
| Client settings (Client Hub) | `EnableMorpheusShopCustomOptions` | `1` | `1` = clicking Buy opens the customization popup (sliders for Level/Luck/Skill/Add/380/Exc/Socket/Harmony). `0` = buys the base item with no adjustable options. |

{% hint style="warning" %}
Both switches (server `Custom.dat` and the client settings) must be `1`. The server file is always loaded, even with the switch at `0`, so errors in it are logged either way.
{% endhint %}

`CustomShopMessageBox` (also in `Custom.dat`) is a different feature: a confirmation box for **NPC shop** purchases. See [Shop MessageBox](server-features.md#shop-messagebox).

## Files & syntax

Server file: `Data/Custom/CustomMorpheusShop.txt` (about 250 KB, ~960 priced items as shipped). It has 5 numbered sections, each ending with `end`. Every row starts with an **item range** `ItemMinIndex ItemMaxIndex`, where the index is the main item index `Group * 512 + Index` (for example `6144` = group 12, index 0). The first row whose range contains the item wins.

### Section 0 - Item value (base price)

| Column | Meaning | Values |
|---|---|---|
| ItemMinIndex | First item of the range | `Group*512+Index` |
| ItemMaxIndex | Last item of the range | same |
| 1DayCoin1 / 1DayCoin2 / 1DayCoin3 | Price for 1 day in WCoinC / WCoinP / GoblinPoint | `0` = not sold in that coin |
| 7DayCoin1..3 | Price for 7 days | same |
| 30DayCoin1..3 | Price for 30 days | same |
| UnlimitedCoin1..3 | Price for a permanent item | same |
| MorpheusCreditUnlimited | Read but not used by the GameServer | keep `0` |

The player chooses the coin. The server debits **only** the chosen coin and refuses the purchase if the price in that coin is `0`. The shop shows the cheapest non-zero price of each item.

For effect items only the 1/7/30-day columns are valid: "Unlimited" is refused, and an effect item with only an Unlimited price disappears from the catalog (the GameServer log warns you at load time).

### Section 1 - Item option value (extra cost per option)

| Column | Meaning |
|---|---|
| ItemMinIndex, ItemMaxIndex | Item range |
| LevelCoin1..3 | Extra cost per item level (multiplied by the chosen level) |
| LuckyCoin1..3 | Extra cost for luck |
| SkillCoin1..3 | Extra cost for skill |
| AddCoin1..3 | Extra cost per additional-option step |
| 380Coin1..3 | Extra cost for the +380 option |
| ExcCoin1..3 | Extra cost for the excellent options value sent by the shop |
| SetCoin1..3 | Extra cost for the socket value sent by the shop |
| SocketCoin1..3 | Extra cost for the harmony value sent by the shop |

Each group is `Coin1 Coin2 Coin3` (WCoinC, WCoinP, GoblinPoint). The extra cost is added in the same coin the player chose for the base price. A row with all zeros means options are free (or, combined with Section 2, not allowed).

### Section 2 - Item max options (what the player may build)

| Column | Meaning | Values |
|---|---|---|
| ItemMinIndex, ItemMaxIndex | Item range | |
| MaxLevel | Highest item level | `0..15` |
| MaxLucky | Luck allowed | `0` no, `1` yes |
| MaxSkill | Skill allowed | `0` no, `1` yes |
| MaxAdd | Highest additional option step | `0..7` |
| Max380 | +380 allowed | `0` no, `1` yes |
| MaxExcellent | How many excellent options at most | `0..6` |
| MaxSetItem | **Used as the maximum number of filled sockets** | `0..5` |
| MaxSocket | **Used as the harmony switch**: `0` = no harmony, any other value = harmony allowed | |

{% hint style="warning" %}
The last two column names in the file header are historical. The server uses `MaxSetItem` as the socket count limit and `MaxSocket` as the harmony on/off. Follow this table, not the header names.
{% endhint %}

### Section 3 - Item discount rate

| Column | Meaning |
|---|---|
| ItemMinIndex, ItemMaxIndex | Item range |
| Discount_AL0 .. Discount_AL3 | Discount per account level (free, VIP1, VIP2, VIP3) |

The discount is read and sent to the shop window with the catalog, but in the current version neither the shop window nor the GameServer applies it: the price charged is always Sections 0 + 1. The shipped file uses `0` everywhere; keep it that way.

### Section 4 - Shop texts (multi-language)

| Column | Meaning | Values |
|---|---|---|
| ItemMin | `0` together with ItemMax `0` = global text; otherwise the item it applies to | |
| ItemMax | same | |
| Lang | Language code | `en`, `pt`, ... |
| Key | Text key | global: `currency_wcoinC`, `currency_wcoinP`, `currency_goblin`, `ui_buy_button`, `ui_close_button`, `ui_storage_tab`, `ui_gift_tab`, `ui_use_button`, `ui_recharge`, `ui_activate_button`, `ui_effect_ok_title`, `ui_dur_1`... ; per item: `name`, `desc`, `category`, `banner` |
| "Value" | Text, in quotes | `{name}` and `{dur}` placeholders are allowed in the effect messages |

If a key is missing in the player's language, the client falls back to `en`.

### Client file - `CustomMorpheusShop.txt`

A single list (no sections) ending with `end`:

| Column | Meaning |
|---|---|
| ItemIndex | Main item index `Group*512+Index` (same numbering as the server ranges) |
| ItemGroup | Group / tab (0 Swords ... 14 Others3) |
| ItemPosX, ItemPosY | 3D preview offset |
| ItemScale | 3D preview scale (`<= 0` becomes `1.0`) |

Comment a line (`//`) to hide an item from the shop.

## Example

```
0
//ItemMinIndex ItemMaxIndex 1DayC1 1DayC2 1DayC3 7DayC1 7DayC2 7DayC3 30DayC1 30DayC2 30DayC3 UnlC1 UnlC2 UnlC3 Credit
0000           0000         0      0      0      0      0      0      0       0       0       50    0     0     0   //Kris: 50 WCoinC, permanent
6144           6144         0      0      0      0      0      0      0       0       0       300   0     500   0   //Wings of Elf: 300 WCoinC or 500 GP
end

1
//Range        Level(C1 C2 C3)  Luck  Skill  Add   380   Exc   Set   Socket
0000  2596     6 0 0            5 0 0 4 0 0  3 0 0 0 0 0 2 0 0 0 0 0 0 0 0   //weapons: +6 WC per level, +5 luck...
end

2
//Range        MaxLevel MaxLucky MaxSkill MaxAdd Max380 MaxExc MaxSetItem(=sockets) MaxSocket(=harmony)
0000  2596     15       1        1        7      0      6      0                    0   //weapons
end

3
0000  8191     0 0 0 0      //no discount
end

4
0  0  en  ui_buy_button  "Buy"
0  0  pt  ui_buy_button  "Comprar"
end
```

(Section 1 row above is shown compressed; in the file each option has three coin columns.)

## Rewards

The purchase result:

- Normal items are created directly in the inventory with the options the player chose, then saved to the database **before** the coins are debited (no "paid but did not receive").
- Effect items activate the buff for the chosen time. Buying the same effect again adds the time; buying a different effect of the same family (for example Seal of Ascension while a Master Seal is active) is refused.
- Coins are debited from the account's WCoinC, WCoinP or GoblinPoint balance.

Purchase results the player can get: insufficient coins, inventory full, invalid item/options/price, shop disabled, effect of the same family already active. Every attempt is written to the Cash Shop log with the reason.

## Commands related

None. The shop is opened from the client interface.

## Pitfalls

- Item indexes are `Group*512+Index`, not the group/index pair you see in `Item.txt`. Wings group 12 starts at `6144`.
- An item must be listed in the client file **and** priced in the server file. Editing only one side makes items disappear or show without price.
- A price of `0` in a coin column means "not sold in that coin". An item with all zero prices does not appear.
- Effect items (Type 2 in `Effect.txt`) need 1/7/30-day prices; Unlimited is refused.
- `MaxSetItem` = socket count and `MaxSocket` = harmony (see Section 2). Leaving `MaxSocket` at `0` blocks harmony on that range.
- Section 3 discounts are currently not applied (neither displayed nor charged).
- Ranges are checked in file order; put specific ranges before broad ones.
- The client list is part of the client data: changing it requires publishing a client update from the Client Hub.

## Reload module

- Server file and `CustomMorpheusShopSwitch`: `/reload custom` (admin). The catalog is rebuilt; players with the shop open receive the new catalog on their next request.
- Client custom file `CustomMorpheusShop.txt` and the client settings keys: never hot-reload. They must be [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).
