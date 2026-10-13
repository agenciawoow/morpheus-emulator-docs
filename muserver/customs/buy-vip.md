# Buy VIP

An in-game window where players buy a VIP plan (account level) with WCoinC, WCoinP or GoblinPoint.

## What it is

The player presses **End** in game to open the VIP window. It lists the plans you configured, with name, duration and price. When the player confirms, the server checks the price against their coin balance, takes the coins, raises the account to the plan's VIP level for the number of days set, and shows `[BuyVip] Congratulations, you subscribed to <plan>!`.

A player can only buy a plan whose VIP level is **higher** than the one they already have.

## Enable it

There is no on/off key. The system is active when `CustomBuyVip.txt` has plans and the client has the matching file. With no rows, nothing can be bought.

The messages come from `Data/Lang`, translated per player language:

| Lang index | English text |
|---|---|
| 757 | `[BuyVip] You do not have enough funds to purchase!` |
| 758 | `[BuyVip] Congratulations, you subscribed to %s!` |
| 759 | `[BuyVip] The selected plan is lower than your current plan!` |

The benefits of each VIP level (experience, drop, command access and so on) are set elsewhere, through the `_AL1`..`_AL3` keys of the GameServerInfo files (for example `AddExperienceRate_AL1`, `ItemDropRate_AL1` in `GameServerInfo - Common.dat`) and the AL columns of files such as `Data/Command.txt`.

## Files & syntax

| Side | File |
|---|---|
| Server | `Data/Custom/CustomBuyVip.txt` |
| Client | Client custom file `CustomBuyVip.txt` in the Client Hub (see [Client custom files](../client/custom-files.md)) |

One row per plan, then `end`. No sections.

| Column | Meaning | Values |
|---|---|---|
| `Index` | Plan number. The client sends this number when the player buys | 1 or higher. Rows with `0` are ignored |
| `Exp+` | Experience bonus shown in the window | Number (display only, see Pitfalls) |
| `Drop+` | Drop bonus shown in the window | Number (display only) |
| `Days` | Plan duration | Days |
| `AccLevel` | VIP level given | `1`, `2` or `3` |
| `Coin1` | Price in WCoinC | Number, `0` = not charged in this coin |
| `Coin2` | Price in WCoinP | Number, `0` = not charged in this coin |
| `Coin3` | Price in GoblinPoint | Number, `0` = not charged in this coin |
| `VipName` | Plan name shown to the player | Text in quotes |

## Example

```
//#Index  #Exp+  #Drop+  Days  #AccLevel #Coin1  #Coin2  #Coin3   #VipName
  1       10     10      7     1         10      0       0        "VIP Bronze"    // 7 days of VIP 1 for 10 WCoinC
  2       20     20      30    1         20      0       0        "VIP Bronze"
  4       10     10      7     2         15      0       0        "VIP Silver"
  7       10     10      7     3         20      0       0        "VIP Gold"
end
```

{% hint style="info" %}
In the shipped server file every plan is commented out (`//`), while the client file has nine plans. Players see plans in the window but every purchase is ignored. Remove the `//` from the server rows (or copy the client rows) to sell VIP.
{% endhint %}

## Rewards

The plan sets the account level (`AccLevel`) for `Days` days. The price columns are debited at the same time. Coins: `Coin1` = WCoinC, `Coin2` = WCoinP, `Coin3` = GoblinPoint.

## Commands related

| Command | Who | What |
|---|---|---|
| `/setvip <account> <type> <days>` | Admin (`GameMaster = 2`, index 29 in `Data/Command.txt`) | Gives VIP by hand, for example to compensate a failed purchase. |
| `/reload custom` | Admin | Re-reads `CustomBuyVip.txt`. |

## Pitfalls

* **Server and client files must match.** The window lists the plans of the client file; the server charges and grants from its own file using the `Index`. A plan that exists only on the client is ignored; a different price on each side means the player pays what the server says, not what the window shows. Change both files together and publish the client update from the Client Hub.
* `Exp+` and `Drop+` are only shown in the window. The real bonus of each VIP level comes from the `_AL1`..`_AL3` settings. Keep the numbers in the window honest.
* A player cannot extend or repeat their current level: buying a plan with `AccLevel` equal to or lower than their current VIP is refused (Lang 759).
* Number the plans in increasing order (1, 2, 3, ...). The server also refuses a purchase when the plan's `Index` is not greater than the player's current VIP level, so very low indexes for high plans can be blocked by mistake.
* Prefer `Coin1` (WCoinC) and `Coin2` (WCoinP). The balance check for `Coin3` (GoblinPoint) compares against the WCoinP balance, so a GoblinPoint price can be refused or accepted wrongly.
* The name shown in the success message comes from the server file. The shipped client says "VIP Bronze / Silver / Gold"; the commented server rows say "Vip Bronze / Prata / Ouro" (Portuguese). Translate them when you enable them.

## Reload module

* `/reload custom` re-reads `Data/Custom/CustomBuyVip.txt`. New prices and plans apply to the next purchase.
* The client file never hot-reloads: after changing the client custom file `CustomBuyVip.txt`, it must be [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).
