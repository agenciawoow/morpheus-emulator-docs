# Cash Shop (X shop)

## What it does

The in-game Cash Shop (opened with **X**) sells packages for WCoinC, WCoinP or Goblin Points. The server side has two files:

| File | Role |
|---|---|
| `CashShop/CashShopPackage.txt` | The **packages** shown in the shop: category, price, currency, bonus points, and which products each package contains. |
| `CashShop/CashShopProduct.txt` | The **products**: the actual item (with options, quantity and duration) a buyer receives. |

{% hint style="danger" %}
The client shows the shop from its own **IBS script files** (categories, package numbers, prices, icons). Package numbers, products and currencies on the server **must match** the client scripts. If they don't, purchases fail with "CoinIndex mismatch" or similar messages in the Cash Shop log. Whenever you change these files, publish matching client scripts and raise `CashShopScriptVersion*` in `GameServerInfo - Common.dat`.
{% endhint %}

The shop is switched on with `CashShopSwitch = 1` in `GameServerInfo - Common.dat`.

## Where it lives

```
MuServer/Data/CashShop/
├── CashShopPackage.txt
└── CashShopProduct.txt
```

---

## CashShopPackage.txt

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Cat` | number | Category id (tab of the shop). Must exist in the client script. |
| `ID` | number | Package **base** index (the package number used by the client). |
| `Main` | number | Package **main** index (usually the same as `ID`). |
| `Item` | item index | Item shown for the package (`Section × 512 + Type`). |
| `Coin` | 508 / 509 / 0 | Currency: `508` = WCoinC, `509` = WCoinP, `0` = Goblin Points. |
| `Price` | number | Price in that currency. |
| `Bonus` | number | Goblin Points given as a bonus on purchase. |
| `PBase1`–`PBase10` | product id or 0 | Up to 10 products (base index in CashShopProduct). `0` = empty. |
| `PMain1`–`PMain10` | product id or 0 | The matching product main indexes. |

### Example (real line)

```
//Cat  ID    Main  Item  Coin  Price  Bonus  PBase1  PBase2 ... PBase10  PMain1  PMain2 ... PMain10  Comment
112    1524  1524  6694  508   430    0      15240   0      ... 0        15240   0      ... 0        // Moonstone Pendant
```

Package 1524 in category 112 costs 430 WCoinC and contains product 15240.

---

## CashShopProduct.txt

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `ID` | number | Product **base** index (referenced by `PBase*`). |
| `Number` | number | Product **main** index (referenced by `PMain*`). |
| `Value` | number | Price of this product option (shown by the client). |
| `Item` | item index | Item delivered (`Section × 512 + Type`). |
| `Level` | 0–15 | Item level. For the VIP ticket item (13/124) this is the **VIP level** granted. |
| `Skill` | 0 / 1 | With skill. |
| `Luck` | 0 / 1 | With luck. |
| `Option` | 0–7 | Additional option level. |
| `ExOpt` | 0–63 | Excellent options bitmask. |
| `AncOp` | number | Ancient value as stored on the item (0 = none; see [Shops](shops.md)). |
| `JOH` | number | Jewel of Harmony value (0 = none). |
| `Oexe` | 0 / 1 | 380 option. |
| `Socket1`–`Socket5` | 0–255 | `255` = no socket, `254` = empty socket. |
| `Qtd` | number | Quantity / durability (stack size for stackable items). |
| `Duration` | seconds | Lifetime of the item. `0` = permanent. For the VIP ticket, the VIP duration. |

### Example (real line)

```
//ID    Number  Value  Item  Level  Skill  Luck  Option  ExOpt  AncOp  JOH  Oexe  Socket1  Socket2  Socket3  Socket4  Socket5  Qtd  Duration  Comment
15240   15240   430    6694  0      0      0     0       0      0      0    0     255      255      255      255      255      1    0         // Moonstone Pendant
```

One permanent Moonstone Pendant.

### Example: a 7-day item

```
15241   15241   300    6694  0      0      0     0       0      0      0    0     255      255      255      255      255      1    604800    // Moonstone Pendant (7 days)
```

## Rules & pitfalls

- The `Coin` of a package must be the same currency the client script uses for it. A mismatch makes every purchase of that package fail.
- Buff items (seals, scrolls) that are linked to an effect in [Effect.txt](effects-and-mining.md) are applied as a buff when used from the Cash Shop storage, lasting the product's `Duration` in seconds. A player who already has that buff active cannot use another one until it ends.
- Keep the `Comment` after `//` with a space or tab before it.

## Edit in the panel

The **Cash Shop (X)** editor (**Editors → Cash Shop (X)**) edits packages, products and the client scripts together, so both sides stay in sync. See [Cash Shop editor](../../dashboard/editors/cash-shop.md).

## Reload module

```
/reload cashshop
```

Reloads the Cash Shop settings of `GameServerInfo - Common.dat` and both files. Players must reopen the shop (and download new scripts if the version changed).
