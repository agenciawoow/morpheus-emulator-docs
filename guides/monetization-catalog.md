# Monetization catalog blueprint

A ready-to-adapt blueprint for your server's store: what to sell, how to price it relative to everything else, how to run bundles and limited-time offers, what to keep out of the store, and where each product is set up in the control panel.

It assumes you have read [Phase 5: Monetization design](profitable-server-methodology.md#phase-5-monetization-design) of the methodology.

{% hint style="warning" %}
Every price on this page is **illustrative**. The numbers only show proportions between products. Your prices depend on your rates, your audience, your region and how players can earn each coin. Nothing here is a revenue forecast.
{% endhint %}

## Where products live

Morpheus gives you five places to sell. Each has its own strengths:

| Store | Opened with | Best for | Setup | Reference |
|---|---|---|---|---|
| Morpheus Shop | Shop window in the HUD | Items the player builds within your limits, buffs for 1, 7 or 30 days | [Morpheus Shop editor](../dashboard/editors/morpheus-shop.md) | [Morpheus Shop](../muserver/customs/morpheus-shop.md) |
| Cash Shop | **X** key | Packages with several purchase options, pets, time-limited items, gifts | [Cash Shop editor](../dashboard/editors/cash-shop.md) | [Cash Shop](../muserver/data/cash-shop.md) |
| Buy VIP | **End** key | VIP tiers by days | Server and client `CustomBuyVip.txt` | [Buy VIP](../muserver/customs/buy-vip.md) |
| Battle Pass | Battle Pass button | Premium and Premium+ plans per season | [Battle Pass editor](../dashboard/editors/battle-pass.md) | [Battle Pass](../muserver/customs/battle-pass.md) |
| Inventory extension | Inventory window | Extra inventory rows | `GameServerInfo - Custom.dat` | [Server features](../muserver/customs/server-features.md#buy-inventory-extension) |

## Product categories

| Category | Purpose | Examples | Fairness risk |
|---|---|---|---|
| **Convenience** | Saves time or effort, no power | Inventory extension, extra warehouse access through VIP, longer offline helper time | Low |
| **Cosmetic** | Looks, identity, status | Pets for looks, wings with modest stats, costume-like items, rank titles earned with VIP | Low |
| **Progression boosts** | Speeds up progress for a limited time | Experience seals and scrolls (1, 7, 30 days), VIP experience bonus | Medium: keep time-limited |
| **Season pass** | Rewards for playing during a season | Battle Pass Premium and Premium+ | Low to medium: depends on the Premium row |
| **VIP** | A bundle of convenience and boosts by days | VIP 1, VIP 2, VIP 3 | Medium: keep benefits mostly convenience |
| **Built items** | Gear the player builds within your limits | Morpheus Shop items with capped options | High unless limits are strict |

## Setting your price anchor

Pick **one reference product** and price everything else relative to it. A common anchor is the 30-day VIP 1, because it is the product most players compare everything against.

Illustrative ladder in WCoinC, with 30-day VIP 1 = 100%:

| Product | Relative price | Illustrative WCoinC |
|---|---|---|
| Inventory extension, one row | 10% | 100 |
| Experience seal, 7 days | 20% | 200 |
| VIP 1, 7 days | 35% | 350 |
| **VIP 1, 30 days (anchor)** | **100%** | **1,000** |
| Battle Pass Premium (one season) | 100% to 150% | 1,000 to 1,500 |
| VIP 2, 30 days | 150% | 1,500 |
| Battle Pass Premium+ | 200% to 250% | 2,000 to 2,500 |
| VIP 3, 30 days | 220% | 2,200 |
| Permanent cosmetic wing | 250% to 400% | 2,500 to 4,000 |

{% hint style="info" %}
The shop editors' **bulk price adjustment** keeps proportions intact when you rescale: select every package, choose **Increase/decrease %**, preview **Before** and **After**, then save. See [Bulk price adjustment](../dashboard/editors/README.md#bulk-price-adjustment).
{% endhint %}

## Price ladders per coin

Give each coin a role (see [The three in-game coins](profitable-server-methodology.md#the-three-in-game-coins)) and keep each ladder consistent with that role. Illustrative example:

### WCoinC (premium coin)

| Product | 1 day | 7 days | 30 days | Permanent |
|---|---|---|---|---|
| Experience seal (buff) | 50 | 200 | 600 | Not offered |
| Cosmetic pet | Not offered | 300 | 800 | 2,000 |
| Cosmetic wing | Not offered | 500 | 1,200 | 3,000 |
| Built weapon, capped options | Not offered | 400 | 1,000 | Not offered |

The Morpheus Shop lets you set a separate price for each duration and each coin. A price of `0` means "not sold in that coin or duration", which is how you hide a permanent version or keep an item out of a cheaper coin.

### WCoinP (event coin)

| Product | Illustrative price | Why |
|---|---|---|
| Tournament entry item | 50 | Gives the slot weight and creates a coin sink |
| Experience seal, 7 days | 250 | Slightly more expensive than in WCoinC, so the premium coin stays attractive |
| VIP 1, 7 days | 400 | A reachable goal for active non-paying players |

### GoblinPoint (earned coin)

| Product | Illustrative price | Why |
|---|---|---|
| Consumables and potions packs | 50 to 200 | Daily sink for online rewards |
| Experience seal, 1 day | 300 | Lets free players taste the boost |
| Jewel bundle (small) | 500 | Earnable, but slower than buying |

{% hint style="warning" %}
For Buy VIP, prefer WCoinC (`Coin1`) or WCoinP (`Coin2`). The VIP window checks a GoblinPoint price against the WCoinP balance, so GoblinPoint VIP prices can be refused or accepted wrongly. See [Buy VIP pitfalls](../muserver/customs/buy-vip.md#pitfalls).
{% endhint %}

## VIP tiers

Buy VIP sells account levels 1, 2 and 3 for a number of days. The **benefits** of each level come from the `_AL1` to `_AL3` settings across the server, which is what makes VIP valuable without selling items:

| Benefit | Setting | Docs |
|---|---|---|
| Experience and drop | `AddExperienceRate_AL*`, `ItemDropRate_AL*` | [GameServerInfo - Common](../muserver/gameserver/common.md) |
| Offline MU Helper time, speed and offline experience | `HelperOffline*_AL*` | [Server features](../muserver/customs/server-features.md#mu-helper-offline) |
| Offline store time | `CustomStoreTime_AL*` | [Server features](../muserver/customs/server-features.md#custom-store-offline-store) |
| Offline attack time and auto-buff | `CustomAttack*_AL*` | [Server features](../muserver/customs/server-features.md#custom-attack-offline-attack) |
| Online coin rewards | `OnlineRewardCoin*Value_AL*` | [Server features](../muserver/customs/server-features.md#reward-online-users) |
| `/pick` access | Requirements per account level | [Pick](../muserver/customs/pick.md) |
| VIP-only `/move` destinations | VIP requirement per destination | [Custom Move](../muserver/customs/custom-move.md) |
| VIP-only buff bots | Optional VIP restriction | [Bot Buffer](../muserver/customs/bot-buffer.md) |
| Extra warehouses per account type | Client and server command settings | [Client settings](../dashboard/client-hub/client-settings.md) |

Illustrative tier design:

| | VIP 1 | VIP 2 | VIP 3 |
|---|---|---|---|
| Experience bonus over free | Small | Medium | Medium-high |
| Offline helper time | Longer than free | Longer | Unlimited |
| Online reward | 2x free | 3x free | 4x free |
| Exclusive | `/pick` | VIP `/move` maps | VIP buff bots |
| Price (30 days, illustrative) | 1,000 WCoinC | 1,500 WCoinC | 2,200 WCoinC |

{% hint style="info" %}
A player cannot buy a plan at or below their current VIP level, so VIP cannot be extended from the window. Sell durations that suit your audience (7 and 30 days), and use the admin `/setvip` command for compensation. The `Exp+` and `Drop+` columns of the VIP window are display only: keep them equal to your real `_AL` settings.
{% endhint %}

## Season pass

| Element | Recommendation (illustrative) | Where |
|---|---|---|
| Plans | Premium at about the price of a 30-day VIP 1; Premium+ at about double, with more instant levels | Battle Pass editor, **Plans** |
| Payment | In-game coin (`PaymentType 1`, for example WCoinC) or your website wallet (`PaymentType 2`) | [Battle Pass](../muserver/customs/battle-pass.md#enable-it) |
| Premium row | Cosmetics, time-limited boosts, jewels; strong items only near the end of the track | Battle Pass editor, **Track** |
| Free row | Enough value that a free player finishes the season feeling rewarded | Battle Pass editor, **Track** |
| Premium missions | A few extra weeklies so Premium holders always have something to do | Battle Pass editor, **Missions** |

{% hint style="warning" %}
Do not rely on the prize **Multipliers** per plan for now: the Battle Pass editor notes they are currently read but not applied by the server. Put the extra value for Premium+ in instant levels, XP bonus and extra weekly missions instead.
{% endhint %}

## Bundles

| Bundle type | How to build it | Where |
|---|---|---|
| **Quantity bundle** (for example 10 jewels) | One Cash Shop package variant with a **Quantity** above 1 | [Cash Shop editor](../dashboard/editors/cash-shop.md#variants-purchase-options) |
| **Duration ladder** (1, 7, 30 days, permanent) | Variants of one package, each with its own price; or the four duration columns of the Morpheus Shop | [Cash Shop editor](../dashboard/editors/cash-shop.md), [Morpheus Shop editor](../dashboard/editors/morpheus-shop.md) |
| **Mixed bundle** (several different items) | Sell a box item that opens an EventItemBag bag. In the advanced model, a rule with **Whole list** drops every item of the list, and **Straight to inventory** delivers it directly | [EventItemBag editor](../dashboard/editors/event-item-bag.md) |
| **Bonus coin bundle** | A Cash Shop package that gives a GoblinPoint bonus on purchase | [Cash Shop editor](../dashboard/editors/cash-shop.md#package-details) |
| **Season bundle** | Battle Pass Premium+ with instant levels | [Battle Pass editor](../dashboard/editors/battle-pass.md#plans) |

{% hint style="info" %}
For a mixed bundle, check the bag's **Trigger** tab for shadowed triggers (another bag with a lower number and the same trigger always wins), and use the **Chances** tab to confirm the bag gives exactly what you advertise.
{% endhint %}

## Limited-time offers tied to events

Offers work best when they are tied to something happening in the game. Illustrative calendar:

| Moment | Offer | How |
|---|---|---|
| New Battle Pass season | Premium at launch with extra instant levels | Raise **Levels granted** in the plan for the first week, then lower it |
| Experience weekend | Experience seal discount | [Bonus Manager](../muserver/events/bonus-manager.md) window plus a temporary price cut |
| Tournament week | Tournament entry item bundle | Cash Shop package added for the week |
| Castle Siege | Guild-wide cosmetic | Cash Shop package with **Can be gifted** on |
| Server anniversary | A one-time cosmetic | Time-limited package, removed afterwards |

How to run a flash sale safely:

{% stepper %}
{% step %}
### Back up

Create a folder [backup](../dashboard/backups-and-restore.md) before changing many prices.
{% endstep %}

{% step %}
### Adjust prices in bulk

Select the products, choose **Increase/decrease %** (for example −25%), check the preview and save. See [Bulk price adjustment](../dashboard/editors/README.md#bulk-price-adjustment).
{% endstep %}

{% step %}
### Apply

**Morpheus Shop:** price changes only need **Reload custom**; players get the new catalog the next time the shop requests it. **Cash Shop:** reload `cashshop` **and** publish to the client, because the shop scripts are client files.
{% endstep %}

{% step %}
### Revert on time

At the end, apply the opposite adjustment (or restore the backup files) and reload again. Announce the end time in advance.
{% endstep %}
{% endstepper %}

{% hint style="success" %}
Because Morpheus Shop prices change with a reload and no client update, it is the best place for short flash sales. Use the Cash Shop for offers planned a few days ahead, published together with your weekly client update.
{% endhint %}

## What not to sell

Keeping these out of the store protects the free population that makes the server worth paying for:

| Do not sell | Why | Offer instead |
|---|---|---|
| The best item of the season at maximum options | Ends the game for everyone else | Capped versions through Morpheus Shop limits |
| Items that decide PvP on their own | Turns tournaments and sieges into a payment contest | Cosmetics; keep the Balance Class `MaxItemBonus` cap |
| Tournament wins, ranking positions or titles | Destroys trust in every ranking | Tournament **entry** items |
| Unlimited jewel packs at low prices | Inflates the economy and crashes player trading | Small, time-boxed bundles |
| Permanent experience boosts | Permanent power creep | 1, 7 or 30-day seals |
| Anything only obtainable by paying that gives power | Free players feel locked out | An earnable path in WCoinP or GoblinPoint |

{% hint style="warning" %}
The Morpheus Shop discount table by account level is currently **not applied**: the price charged is always the base price plus option costs. Do not advertise VIP discounts in the Morpheus Shop. See [Morpheus Shop](../muserver/customs/morpheus-shop.md).
{% endhint %}

## Setting it up in the panel

| Product | Steps | Apply |
|---|---|---|
| Morpheus Shop item | **Editors → Morpheus Shop**: **Add item**, set the tab, create a price range, set option costs and **Limits** | **Reload custom**; if the item list changed, also publish to the client from the editor's bar |
| Cash Shop package | **Editors → Cash Shop (X)**: pick the category, **Add package**, add variants with duration, quantity and price | Reload `cashshop`, then **Publish to client** |
| VIP plan | Edit `Data/Custom/CustomBuyVip.txt` in the [File editor](../dashboard/file-editor.md) and the matching client file in [Client settings](../dashboard/client-hub/client-settings.md) (**Custom loaders**) | `/reload custom`, then publish the client update |
| VIP benefits | `_AL1` to `_AL3` keys in [Configuration forms](../dashboard/configuration-forms.md) or the File editor | `/reload common` or `/reload custom` |
| Battle Pass plans | **Editors → Battle Pass → Plans** | **Reload custom** (and **Reload lang** if texts changed) |
| Inventory extension | `PriceInventoryExt1..4` in `GameServerInfo - Custom.dat` | `/reload custom` |
| Bundle box | **Editors → EventItemBag**: **+ New**, advanced model, **Whole list** rule, item trigger | **Reload EventItemBag** |
| Tournament entry and prizes | `Data/Event/MorpheusTournament.dat`, sections 5 and 7 to 9 | `/reload event`, between editions |

## Catalog checklist

- [ ] One anchor product chosen; every price set relative to it.
- [ ] Each coin has a ladder that fits its role.
- [ ] Every product classified (convenience, cosmetic, boost, season pass, VIP, built item).
- [ ] Morpheus Shop **Limits** reviewed for every range you sell.
- [ ] The "do not sell" list published to players.
- [ ] Server and client sides match: the editors' status panels show no sync problems.
- [ ] One limited-time offer planned per event cycle, with a clear end date.
- [ ] Sales reviewed monthly and prices adjusted in bulk, never one by one in a hurry.
