# Effects (buffs) and Mining

| File | What it controls |
|---|---|
| `Effect.txt` | Every buff/debuff: icon group, the item that grants it, duration, values, whether it survives logout |
| `Mining.txt` | The mining system (ore deposits mined with a Pickax) |

---

## Effect.txt

### What it does

Lists every effect a character or monster can have: skill buffs (Greater Damage, Mana Shield...), debuffs (Poison, Ice, Stun...), item buffs (Seals, Scrolls, Elixirs, Talismans, event potions), GM and event states. Items that give a buff when used are linked here by `ItemIndex`, and their duration and strength come from this file.

### Where it lives

`MuServer/Data/Effect.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | effect id | Effect number. Skills (`Effect` column of [Skill.txt](skills.md)) and the server refer to it. |
| `Group` | number | Buff group/icon. Effects of the **same group** replace each other (you can't stack two Seals of the same group). |
| `ItemIndex` | item index or `*` | Item that applies this effect when used (`Section × 512 + Type`). `*` = not from an item. |
| `Name` | `"text"` | Name. |
| `Save` | 0 / 1 | `1` = the effect is saved and restored after logout/login. |
| `Type` | 0 / 1 / 2 | `0` = normal buff (from skills; ends on death/clear). `1` = item buff counted while online. `2` = item buff with **real-time** expiry: the time keeps running while the player is offline. |
| `Flag` | 0 / 1 | `1` = debuff (removed by cleansing skills/items). |
| `Count` | seconds or `*` | Duration. `*` = set by the skill or by whoever applies it. |
| `Value1`–`Value4` | number or `*` | Strength of the effect (meaning depends on the effect: % experience, + damage, + HP...). `*` = set by the code. |

### Example (real lines)

```
//Index   Group   ItemIndex   Name                   Save   Type   Flag   Count   Value1   Value2   Value3   Value4
1         1       *           "Greater Damage"       1      0      0      *       *        *        *        *
29        60      6699        "Seal of Ascension"    1      2      0      1800    50       *        *        *
55        27      *           "Poison"               0      0      1      *       *        *        *        *
```

- Seal of Ascension (item 6699): saved, real-time expiry, 1800 seconds (30 minutes), value 50 (+50% experience).
- Poison: a debuff, duration and damage set by the skill.

### Example: make the Seal of Wealth last 2 hours

```
30        61      6700        "Seal of Wealth"       1      2      0      7200    *        25       *        *
```

### Rules & pitfalls

- Keep `Index` and `Group` as shipped; the client draws the buff icon by group.
- `Type 2` items keep counting while offline: players who buy a 30-minute seal and log out lose it.
- Several lines can share one `ItemIndex` group in the shipped file (two "Seal of Ascension" rows); the server uses the first match for the item.
- Cash Shop buff items use the product's own duration (see [Cash Shop](cash-shop.md)).

---

## Mining.txt

### What it does

Configures **mining**: players equip a Pickax in the right hand and hit an ore deposit. Each successful hit advances the deposit's stage (1 to 5) and gives a reward; the last stage can give a bonus.

### Where it lives

`MuServer/Data/Mining.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | monster index | The deposit (a monster class in `Monster.txt`; shipped: 605, 606, 607). |
| `Stage` | 1–5 | Mining stage this line is for. |
| `Value` | number | Pickax durability spent per attempt. The Pickax must have at least this durability. |
| `ItemRate` | per 10,000 | Success chance of the attempt. |
| `ItemIndex` | item index | Reward item. |
| `ItemCount` | number | Reward quantity. |
| `BonusItemRate` | per 10,000 or `*` | Chance of an extra bonus at this stage. `*` = none. |
| `BonusItemIndex` | item index or `*` | Bonus item. |
| `BonusItemCount` | number or `*` | Bonus quantity. |

### Example (real lines)

```
//Index   Stage   Value   ItemRate   ItemIndex   ItemCount   BonusItemRate   BonusItemIndex   BonusItemCount
605       1       2       4750       7181        1           *               *                *
605       5       2       4750       7181        33          250             7181             100
```

On deposit 605, each attempt costs 2 Pickax durability and succeeds 47.5% of the time. Stage 1 gives 1 Jewel of Bless; stage 5 gives 33, plus a 2.5% chance of 100 more.

### Rules & pitfalls

- The Pickax is item 0/41 and must be in the **right hand**.
- Deposits need spawns: place monsters 605–607 with the [Spot editor](../../dashboard/editors/spot.md) or in `MonsterSetBase`.
- High `ItemCount` on jewels inflates your economy quickly.

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload common
```

Reloads `GameServerInfo - Common.dat`, `Effect.txt`, `MapManager.txt`, `MapServerInfo.dat`, languages and `Mining.txt`. `/reload all` includes it.
