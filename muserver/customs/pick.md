# Custom Pick

Auto-pickup: with `/pick <type>` the character automatically picks up chosen items (jewels, zen, excellent, socket or ancient items) that drop within view.

## What it is

The player types `/pick soul`, `/pick bless`, `/pick zen`... and from then on every matching item on the ground within their view range that they are allowed to loot is put straight into their inventory (stacking when possible). Typing the same command again turns that type off. Besides the types you define in the file there are three built-in types:

| Type | Picks up |
|---|---|
| `exc` | Any excellent item |
| `socket` | Any socket item |
| `setitem` | Any ancient (set) item |

Auto-pickup turns itself off (all types) when the player enters a safe zone, opens a window/trade/personal shop, dies, teleports, or moves to a map where pick is not allowed. Event items are never auto-picked.

## Enable it

Pick has no switch of its own in `GameServerInfo`. It is controlled by:

| File | Key / column | Default | Meaning |
|---|---|---|---|
| `Data/Command.txt` | row `54 "/pick"` - Enable AL0..AL3 | `1 1 1 1` | Enables the command per account level (also Money, Level, Reset, Delay, coin cost columns of that row). |
| `Data/MapManager.txt` | column `CustomPick` | `1` on most maps | `1` = auto-pickup allowed on that map, `0` = refused ("You cannot use this in this map!"). |

{% hint style="warning" %}
`GameServerInfo - Custom.dat` ships a "Custom Pick Command" block (`CustomPickSwitch`, `CustomPickEnable_AL0..3`, `CustomPickRequireLevel_AL0..3`, `CustomPickRequireReset_AL0..3`, `CustomPickRequireMoney_AL0..3`). **The current GameServer does not read these keys.** Use the `/pick` row of `Data/Command.txt` to enable it per account level and to set level, reset and Zen requirements.
{% endhint %}

## Files & syntax

File: `Data/Custom/CustomPick.txt`. A single list (no section number) closed by `end`. Maximum 20 types (ID `0..19`).

| Column | Meaning | Values |
|---|---|---|
| ID | Slot id, unique | `0..19` |
| String | Word typed after `/pick`, in quotes | `"soul"` (case-insensitive) |
| Cat | Item group | e.g. `14` |
| Item | Item index | e.g. `14` |

`Cat 14 / Item 15` is Zen: picking it adds the money directly.

## Example

```
//  ID   String       Cat   Item
    0    "soul"       14    14      // /pick soul     -> Jewel of Soul
    1    "bless"      14    13      // /pick bless    -> Jewel of Bless
    2    "chaos"      12    15      // /pick chaos    -> Jewel of Chaos
    3    "zen"        14    15      // /pick zen      -> Zen
    4    "life"       14    16      // /pick life     -> Jewel of Life
    5    "creation"   14    22      // /pick creation -> Jewel of Creation
    6    "gemstone"   14    41      // /pick gemstone -> Gemstone
    7    "guardian"   14    31      // /pick guardian -> Jewel of Guardian
end
```

## Commands related

| Command | Who | What |
|---|---|---|
| `/pick <type>` | Player (Command.txt GameMaster `0`) | Toggles auto-pickup for `<type>`: a String from the file, or `exc`, `socket`, `setitem`. |

## Pitfalls

- The `CustomPick*` keys in `Custom.dat` have no effect (see warning above).
- `exc`, `socket` and `setitem` are reserved words; do not reuse them as a String.
- IDs above `19` are not usable.
- Items still respect loot rights (party/owner protection): the player only picks what they could pick by hand.
- Pickup stops when the inventory is full for that item.
- Auto-pickup does not work in safe zones; it switches off as soon as the player steps into one.

## Reload module

`/reload custom` (admin) re-reads `CustomPick.txt`. Changes to `Data/Command.txt` need the command reload (`/reload command`), and `MapManager.txt` changes need `/reload map`.
