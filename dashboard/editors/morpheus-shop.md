# Morpheus Shop editor

**Edit the Morpheus shop item by item:** which items appear, price per duration and coin, options, limits, discounts and texts. Changed a price? Reload the GameServer. Changed the item list? Also publish to the client.

For the meaning of every column, see the MuServer reference: [Morpheus Shop](../../muserver/customs/morpheus-shop.md).

## How the Morpheus Shop works

The shop has two halves, and the editor shows both together for each item:

| Half | File | Controls | How to apply |
|---|---|---|---|
| **Client registry** | Client custom file `CustomMorpheusShop.txt` (published to players from the Client Hub) | Which items appear, in which tab, and how the 3D model is shown. | **Encode + Publish to client** |
| **Server catalog** | `Data/Custom/CustomMorpheusShop.txt` | Price per duration and coin, option costs, limits, discounts and texts — defined per **item range**. | **Reload** the `custom` module |

The client receives the server catalog when the shop opens, so price changes only need a GameServer reload. A new or removed item needs a new client configuration.

{% hint style="warning" %}
The shop only opens when **CustomMorpheusShopSwitch** is on in **both** places: in the client settings ([Client Hub → Client settings](../client-hub/client-settings.md) → Custom tab) and in the server's `GameServerInfo - Custom.dat`. The status chips **Client** and **Server (GameServer)** show **on** / **OFF** for each side.
{% endhint %}

## Screen layout

The top bar contains the Save → Reload → Publish bar, the mode tabs (**By item** / **Advanced mode**), a search (**Search by name or group,index…**), **Add item** and **Adjust prices (N)**.

| Pane | Content |
|---|---|
| **Groups** | **All items**, **With problems**, then the shop tabs: Swords, Axes, Scepters, Spears, Bows, Staffs, Shields, Helms, Armors, Pants, Gloves, Boots, Pendants/Rings, Wings/Pets, Others. |
| **Items** | Items of the group, with badges **Priced**, **no price** or **all prices are 0**. |
| **Details** | The selected item. |

## Adding or removing an item

1. Click **Add item** and pick one or more items.
2. Set **Tab (group)** and the 3D adjustments, then create a price range (below).
3. To remove, select the item and click **Remove from shop** (*Leaves the client registry and, if it has a price range of its own, that range is removed too.*).

## Item details

The header shows `group G, index I · code C`.

### How it appears (client)

*Shop tab and 3D model adjustment on the card. Changing this requires Encode + Publish.*

| Field | Description |
|---|---|
| **Tab (group)** | Which shop tab the item appears in. |
| **Offset X** / **Offset Y** | Position of the 3D model on the card. |
| **Scale** | 3D model size (e.g. wings `0.6`). |

### Prices (server)

Price per duration (**1 day**, **7 days**, **30 days**, **Permanent**) × coin (**WCoin (C)**, **WCoin (P)**, **Goblin Point**). **0 = unavailable for that duration and coin.**

If the item has no price yet: *This item has no price: it shows in the shop but cannot be bought.* Click **Create a price range for this item**.

### Shared ranges and splitting

The server catalog defines prices per **range of items**. If the current range covers several items you'll see: *This range covers N items (min–max). Editing here changes all of them. To change only this one, split it.* Click **Split this item** to give the item its own range (**Range for this item only (#item).**).

### Option costs, limits and discounts

| Section | Description |
|---|---|
| **Option costs** | How much each increment (level, luck, skill, excellent…) costs, per coin. |
| **Limits** | How far each option can go on this item. **0 disables.** |
| **Discounts by account level** | % discount per level: 0 = normal, 1 = bronze, 2 = silver, 3 = gold. |

When an item has no line in a section, the server uses the default; click **Create a line for this item** to customise it.

### Shop name and description

Per language. Empty uses the item name. Without a text in the player's language, the server tries Portuguese, then English, then the item list name.

{% hint style="info" %}
Item code 0 cannot have its own name and description — in the file, `0 0` is reserved for global texts. The shop uses the item list name for it.
{% endhint %}

## Status checks

| Message | Meaning |
|---|---|
| **Shop ready: switches on and every item priced** | Everything is fine. |
| **&lt;item&gt; is in the shop but has no price range: it shows up and cannot be bought.** | Create a price range. |
| **&lt;item&gt; has a server price but is not in the client registry: nobody sees it.** | Add the item to the shop or remove the price. |
| **&lt;item&gt; appears twice in the client registry.** | Remove the duplicate. |
| **Section S, line L: inverted range (min greater than max).** | Fix the range in Advanced mode. Blocks saving. |
| **The item list was saved but not yet published to the client (Encode + Publish).** | Publish to the client. |

## Advanced mode

**Advanced mode** shows the raw tables exactly as they are in the files: **Server · Data/Custom/CustomMorpheusShop.txt** and **Client · CustomMorpheusShop.txt**. Use it for bulk edits of ranges. Switching modes with unsaved changes reloads the files and loses them.

## Saving and applying

1. Click **Save** (*Saved. Changed a price? Reload the GameServer. Changed the item list? Also publish to the client.*).
2. **Price changes:** click **Reload &lt;GameServer&gt;** (module **custom**).
3. **Item list changes** (added/removed items, tab, offset, scale): click **Encode + Publish to client**. This rebuilds the client configuration with the item list and uploads it; players get it on the next launcher start.

## Bulk price changes

Tick items (**Select all (N)**) and click **Adjust prices (N)** — see [Bulk price adjustment](README.md#bulk-price-adjustment). If several ticked items share one price range, remember that the range is a single set of prices: split items first if they need different prices.
