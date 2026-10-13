# Mix Expansion

An extra mixing window opened from an NPC (by default the Chaos Goblin in Noria), with its own list of named recipes, success rates per VIP level and talismans.

## What it is

The player talks to the configured NPC. Instead of the normal Chaos Machine, a mix window opens with a list of recipes by name (for example "Wings Level 4"). The player picks a recipe, sees its success rate, Zen cost and the talismans it accepts, places the items and mixes. On success the window returns the result item; on failure the items are lost.

{% hint style="danger" %}
**Treat this system as experimental.** In the current version:

* The shipped `MixExpansion.txt` is written in an **older layout** that the current server does not understand. Loaded as is, no recipe is created and the NPC setting becomes garbage, so the window never opens. The file must be rewritten in the layout below before anything works.
* The items placed in the expansion window are **not saved to the database**. Items left in the window when the player logs out or the server restarts are lost.
* The mix does not verify that all listed ingredients are present before rolling the rate.

Do not enable it on a live server without testing it end to end on a test server, and warn players not to leave items in the window.
{% endhint %}

## Enable it

There is no on/off key. The NPC and the recipes come from `Data/Custom/MixExpansion.txt`.

| File | Key | Shipped | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `GoblinMixNpc`, `GoblinMixNpcX`, `GoblinMixNpcY`, `GoblinMixNpcMap` | `238`, `185`, `105`, `3` | **Not read by the server.** They document the default NPC (Chaos Goblin at Noria 185,105). The real NPC position is section 0 of `MixExpansion.txt`; changing these keys has no effect. |

The client must support the expansion window. The shipped client files have no setting for it.

## Files & syntax

File: `Data/Custom/MixExpansion.txt`. Four sections. The layout below is the one the server reads.

### Section 0: NPC

| Column | Meaning | Values |
|---|---|---|
| `Npc` | NPC class that opens the window | Monster index (238 = Chaos Goblin) |
| `Map` | Map of that NPC | Map number, `*` = any |
| `PosX` / `PosY` | Position of that NPC | Coordinates, `*` = any |

If section 0 is missing, the defaults are NPC 238 on map 3 at 185,105.

### Section 1: recipes

| Column | Meaning | Values |
|---|---|---|
| `Key` | Recipe number | 1 or higher (0 is ignored) |
| `Rate_AL0` .. `Rate_AL3` | Success rate per account level | 0..100 % |
| `RequiredType` | Recipe type shown to the client | Number |
| `Money` | Zen cost | Zen |
| `MainItem` | Which ingredient row (section 2 `MainIndex`) is the "main" item. On success that item is kept and upgraded instead of creating a new one | Number, `0` = none |
| `TalismanOfChaos` | Item code of the chaos talisman accepted | `Type * 512 + Index`, `*` = none |
| `TalismanOfLuck` | Item code of the luck talisman | `Type * 512 + Index`, `*` = none |
| `TalismanRate` | Success rate added per luck talisman | % |
| `MaxTalisman` | Maximum luck talismans that count | Number |
| `Name` | Recipe name in the list (last column) | Text in quotes, up to 31 characters |

### Section 2: ingredients

| Column | Meaning | Values |
|---|---|---|
| `Key` | Recipe from section 1 | Number |
| `MainIndex` | Row id, matched against `MainItem` | Number |
| `MinIndex` / `MaxIndex` | Item code range accepted | `Type * 512 + Index` |
| `MinLevel` / `MaxLevel` | Item level range | 0..15 |
| `MinDur` / `MaxDur` | Durability range | 0..255 |
| `Skill` | Not checked for ingredients | Any |
| `Luck` | `1` = must have luck | 0 / 1 |
| `MinOpt` / `MaxOpt` | Additional option range | 0..7 |
| `Exc` | `1` = must be excellent | 0 / 1 |
| `Anc` | `1` = must be ancient | 0 / 1 |
| `MinCount` / `MaxCount` | How many items of this row | Number |

### Section 3: results

| Column | Meaning | Values |
|---|---|---|
| `Key` | Recipe from section 1 | Number |
| `MinIndex` / `MaxIndex` | Result item code. Equal values = that item; a range = a random existing item below `MaxIndex` | `Type * 512 + Index` |
| `Level` | Result level | 0..15, `255` = random |
| `Skill` | Skill option | 0 / 1, `255` = random |
| `Luck` | Luck option | 0 / 1, `255` = random |
| `Option` | Additional option | 0..7, `255` = random |
| `Exc` | Excellent bitmask | 0..63, `255` = random |
| `Anc` | Ancient option | `255` = random |

With several result rows, one is drawn at random. With a `MainItem`, only the first result row is used, to change the main item's options.

## Example

```
0
//Npc  Map  PosX  PosY
  238  3    185   105
end

1
//Key Rate0 Rate1 Rate2 Rate3 Type Money     MainItem TalismanOfChaos TalismanOfLuck TalismanRate MaxTalisman Name
  1   60    65    70    75    0    10000000  0        7264            7221           5            2           "Wings Level 4"
end

2
//Key MainIndex MinIndex MaxIndex MinLvl MaxLvl MinDur MaxDur Skill Luck MinOpt MaxOpt Exc Anc MinCount MaxCount
  1   0         6180     6180     0      15     0      255    0     0    0      7      0   0   1        1      // one item 12,36
end

3
//Key MinIndex MaxIndex Level Skill Luck Option Exc Anc
  1   6339     6339     0     255   255   255    0   0      // result item code (Type*512+Index)
end
```

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload custom` | Admin | Re-reads `MixExpansion.txt`. |

## Pitfalls

* **Rewrite the shipped file first.** It uses an old layout (recipe in section 0, name in the second column) and loads as nonsense.
* The `GoblinMix*` keys in `GameServerInfo - Custom.dat` do nothing. Set the NPC in section 0.
* A recipe with ingredients but no section 3 row can crash the GameServer on a successful mix. Always give every recipe at least one result row.
* Items in the expansion window are not saved. Never leave items there across logouts or restarts.
* Any item can go into the window; the mix does not refuse missing ingredients.
* The Zen is taken before the roll and is not refunded on failure.

## Reload module

`/reload custom` re-reads `Data/Custom/MixExpansion.txt`, including the NPC in section 0. Players with the window open should close and reopen it.
