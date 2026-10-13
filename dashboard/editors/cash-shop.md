# Cash Shop (X) editor

**Edit the in-game cash shop while seeing each item by name and icon.** Whatever you save goes to the MuServer files **and** the client scripts at the same time, so both sides never drift apart. Then reload the GameServer and publish to the client.

For the file format of the cash shop, see the MuServer reference: [Cash Shop](../../muserver/data/cash-shop.md).

## Key concepts

| Concept | Meaning |
|---|---|
| **Zone** | A shop **tab**. |
| **Category** | A group inside a tab. |
| **Package** | The card the player sees: name, description, item, coin and category. |
| **Variant** (purchase option) | "7 days", "30 days", "permanent"… each with its own price. Up to 10 per package. With a single variant, its price is the package price. |

**One save, five files:** saving writes `CashShopPackage.txt` and `CashShopProduct.txt` on the server, and `ibscategory.txt`, `ibspackage.txt` and `ibsproduct.txt` on the client — always matching.

## Screen layout

At the top: the Save → Reload → Publish bar, a search box (**Search by name or package no.…**), **Add package** and **Adjust prices (N)**. Under it, a line shows the client script folder in use and a chip **shop on** or **shop OFF (CashShopSwitch = 0)**.

The editor has three panes:

| Pane | Content |
|---|---|
| **Categories** | **All packages**, **With problems**, then each zone (tab) with its categories. |
| **Packages** | The packages of the selected category, with a status icon and the number of options. |
| **Details** | All fields of the selected package and its variants. |

## Managing zones and categories

- **New zone (tab)** creates a tab; **New category** creates a group inside the selected tab.
- **Double-click** a name (or use **Rename**) to rename it.
- To remove a zone or category, it must be empty first — otherwise you see **Cannot remove** (*This zone still has N category(ies). Move or remove them first.*).

## Adding a package

1. Select the category where it should appear.
2. Click **Add package** and pick the item in the item picker.
3. Fill in the details (below) and add at least one variant.
4. Click **Save**.

You can also **Duplicate** an existing package and **Change item** on the copy.

## Package details

| Field | Description |
|---|---|
| **Shop name** | Shown on the card. No `@` and no quotes. |
| **Description** | Short text under the name. Use `#` for a line break. |
| **Category** | Where the package appears in the shop. |
| **Coin** | **WCoin (C)**, **WCoin (P)** or **Goblin Point**. Kept identical on server and client automatically. |
| **Goblin Point bonus** | Extra points the player earns on purchase (0 = none). |
| **Order** | Position inside the category (lower shows first). |
| **Can be gifted** | Enables the "Gift" button on this package. |

The header of the details pane shows the item (`name · group G, index I`) and where the package exists: **exists on server and client**, **server only — the client gets it on the next save**, or **client only — the server gets it on the next save**.

## Variants (purchase options)

**Each line is an option with its own price. Max 10.**

| Column | Description |
|---|---|
| **Option name** | E.g. `Pet Panda (7 days)`. |
| **Duration** | **Permanent**, a number of **days** or **hours**. |
| **Quantity** | How many units are delivered. |
| **Price (coin)** | Price in the package coin. |
| **Item options** | Expand to set **Level** (0–15), **Option (+4)** (0–7), **Excellent** (bit mask 0–63), **Ancient**, **Skill**, **Luck**, **Option 380**, **Harmony** and **Sockets** (five slots, 255 = empty). |

**The first option sets the price shown on the card.** Use **Add option** / **Remove option**. A package with no variant cannot be bought.

## Status and sync checks

The status panel compares server and client. When everything matches it shows **Server and client in sync**. Typical problems:

| Message | Meaning |
|---|---|
| **Package N exists on the client but not on the server. The GameServer rejects the purchase.** | Players see it but can't buy it. Saving from the editor fixes it. |
| **Package N, variant S: the client shows one price and the server charges another.** | Price mismatch. Saving re-aligns both sides. |
| **Package N: client and server use different coins. The purchase is rejected.** | Coin mismatch. |
| **Package N: more than 10 variants. The GameServer accepts at most 10.** | Remove variants. |
| **Package N: the item is not in the server Item.txt.** | Change the item. |
| **Package N: the category does not exist (the package is not shown in the shop).** | Move it to an existing category. |
| **shop OFF (CashShopSwitch = 0)** | The shop is disabled in the server settings; enable it in [Configuration](../configuration-forms.md). |

In the **Packages** list, icons show **Has errors: purchase fails**, **Has warnings**, or **Server and client match**. Use **With problems** in the Categories pane to list only packages that need attention.

## Saving, reloading and publishing

1. Click **Save**. Message: *Saved to all 5 files (server + client). Now reload the GameServer and publish to the client.*
2. Click **Reload &lt;GameServer&gt;** in the bar (module **cashshop**). The server starts charging and delivering what you saved. Nobody is kicked.
3. Click **Publish to client**. The shop scripts are uploaded with the rest of the client; players get them on the next launcher start. Until then they see the old showcase. The bar shows `published · version N`.

{% hint style="warning" %}
Don't skip step 3. If the server and the players' client differ, players may see packages they can't buy, or old prices.
{% endhint %}

## Bulk price changes

Tick packages in the list (**Select all (N)** ticks the whole category) and click **Adjust prices (N)**. See [Bulk price adjustment](README.md#bulk-price-adjustment). Remember to **Save** afterwards.
