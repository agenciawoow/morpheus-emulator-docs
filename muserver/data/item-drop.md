# Item drop

## What it does

`ItemDrop.txt` adds **extra, targeted drops**: a specific item, with specific options, from specific maps, monsters or monster levels, at a chance you choose. It is how you make Jewels of Bless drop everywhere, Fenrir materials only in Crywolf, skill scrolls only in Atlans, and so on.

The normal random drop (any item of the right level), the excellent and ancient chances per map, and Zen drops are explained in [How a monster drop is decided](#how-a-monster-drop-is-decided).

## Where it lives

`MuServer/Data/Item/ItemDrop.txt`

## Syntax

One line per rule. `*` means "any".

| Column | Values | Meaning |
|---|---|---|
| `Index` | item index | Item to drop (`Section × 512 + Type`). Must exist in `Item.txt`. |
| `Level` | 0–15 | Base item level (kept when `Option0` is `*`). |
| `Grade` | 0–63 | Fixed excellent mask, used only when `Option4` gives no excellent option (1, 2, 4, 8, 16, 32 = the six excellent options). |
| `Option0` | row or `*` | Level roll — row of [ItemOptionRate](item-options.md) section 0. |
| `Option1` | row or `*` | Skill roll (section 1). |
| `Option2` | row or `*` | Luck roll (section 2). |
| `Option3` | row or `*` | Additional option roll (section 3). |
| `Option4` | row or `*` | Excellent count roll (section 4). |
| `Option5` | row or `*` | Ancient roll (section 5). |
| `Option6` | row or `*` | Socket count roll (section 6). |
| `Duration` | seconds | Item lifetime. `0` = permanent. |
| `MapNumber` | map or `*` | Only on this map. |
| `MonsterClass` | monster or `*` | Only from this monster (index from `Monster.txt`). |
| `MonsterLevelMin` | level or `*` | Only from monsters of this level or higher. |
| `MonsterLevelMax` | level or `*` | Only from monsters of this level or lower. |
| `DropRate` | 0–1,000,000 or `*` | Chance **per 1,000,000** on each kill. `*` = always qualifies. |

{% hint style="warning" %}
`DropRate` is **per million**, not per 10,000. `300` = 0.03% per kill, `10000` = 1%, `1000000` = 100%.
{% endhint %}

As in the bags, `0` in an Option column is row 0 of ItemOptionRate ("Common") and **rolls**. Use `*` for "no option".

### How the rules are evaluated

On every monster kill the server checks every line:

1. The item must exist, and map / monster / level filters must match.
2. The line rolls its `DropRate` (per million). Event bonuses (BonusManager) can raise this chance.
3. If **several** lines pass on the same kill, only **one** item drops. The server picks among them with a weight of `1,000,000 / DropRate`, so the **rarer** line is more likely to be the one chosen.
4. If an `ItemDrop.txt` item dropped, the normal random drop of that kill does **not** happen.

## Example (real lines)

```
//Index  Level  Grade  Option0  Option1  Option2  Option3  Option4  Option5  Option6  Duration  MapNumber  MonsterClass  MonsterLevelMin  MonsterLevelMax  DropRate  //Comment
6688     0      0      *        *        *        *        *        *        *        0         34         *             25               150              300       // Fenrir item (Crywolf)
7181     0      0      *        *        *        *        *        *        *        0         *          *             1                150              30        //Jewel of Bless (all maps)
6158     0      0      *        *        *        *        *        *        *        0         7          *             1                150              250       //life - BK (Atlans)
```

- Item 6688 drops only in map 34 (Crywolf), from monsters level 25–150, at 300 per million (0.03%).
- Jewel of Bless drops on every map from level 1–150 monsters at 30 per million (0.003%).

### Example: an excellent item with random options

```
//Index  Level  Grade  Option0  Option1  Option2  Option3  Option4  Option5  Option6  Duration  MapNumber  MonsterClass  MonsterLevelMin  MonsterLevelMax  DropRate
3584     0      0      27       12       13       *        20       *        *        0         2          *             40               60               5000
```

In Devias (map 2), monsters level 40–60 have a 0.5% chance (5000 per million) to drop a Bronze Helm (3584), +0~+2, always with skill, 50% luck and 1–2 excellent options.

{% hint style="info" %}
A monster that has an EventItemBag in `EventItemBagManager.txt` (Golden monsters, bosses) drops **only** from its bag. `ItemDrop.txt` lines and the normal drop never apply to it, even when the bag rolls nothing.
{% endhint %}

## How a monster drop is decided

When a monster dies, the server tries, in order, and stops at the first that applies:

1. An [EventItemBag](event-item-bag.md) linked to that monster (`MonsterClass` column of `EventItemBagManager.txt`). If the monster has a bag, nothing below is tried.
2. Quest item drops (`Quest/QuestObjective.txt`).
3. `ItemDrop.txt` (this page).
4. The **normal drop**, built from these settings:

| Step | Source | Rule |
|---|---|---|
| Ancient | `MapManager.txt` `SetItemDropRate` | Monsters level 25+: chance per **1,000,000**. Drops a random ancient item. |
| Excellent | `MapManager.txt` `ExcItemDropRate` | Monsters level 25+: chance per **1,000,000**. The item is picked as if the monster were 25 levels lower and rolls ItemOptionRate row `1` (Exc). |
| Item | `GameServerInfo - Common.dat` `ItemDropRate` (per VIP level) × `MapManager.txt` `ItemDropRate` (%) × event bonuses, against the monster's `ItemRate` in `Monster.txt` | A random item with `Drop = 1` whose drop `Level` in `Item.txt` is between (monster level − 2) and (monster level + 4). Options roll ItemOptionRate row `0` (Common). Socket items only if the map has `SocketItemDrop = 1`. |
| Zen | Monster's `MoneyRate` | If no item dropped, Zen drops with a chance of 10 / `MoneyRate`. |

Blood Castle and Chaos Castle maps have their own drop rules and skip the normal drop.

See [Maps](maps.md) for the per-map columns and [Monsters](monsters.md) for `ItemRate` and `MoneyRate`.

## Rules & pitfalls

- A `DropRate` written as if it were per 10,000 makes the item **100× rarer** than intended.
- Many lines that match the same monster do not stack: at most one `ItemDrop.txt` item per kill.
- Lines whose item is not in `Item.txt` are skipped silently.
- Use `MonsterLevelMin`/`MonsterLevelMax` to keep high-value drops away from beginner maps when `MapNumber` is `*`.

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload item
```
