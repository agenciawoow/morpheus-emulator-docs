# Start Item

Items and buffs every new character receives on their first login, defined per class.

## What it is

The first time a newly created character enters the game, it receives the starting kit of its class directly in the inventory (for example a +11 set, a weapon and a pet) and the starting buffs. This happens only once per character: the database marks the character as already equipped.

## Enable it

There is no on/off key: if the file has rows for a class, new characters of that class receive them. To disable, leave both sections empty.

## Files & syntax

File: `Data/Custom/CustomStartItem.txt`. Sections `0` and `1`, each closed by `end`.

### Section 0 - Starting items

| Column | Meaning | Values |
|---|---|---|
| Class | Character class | `0` DW, `1` DK, `2` FE, `3` MG, `4` DL, `5` SU, `6` RF |
| Section | Item group | e.g. `7` helm, `8` armor, `12` wings/others |
| Type | Item index in the group | |
| Level | Item level | `0..15` |
| Dur | Durability | `0` = item default |
| Skill | `0`/`1` | |
| Luck | `0`/`1` | |
| Option | Additional option step | `0..7` |
| ExcOp | Excellent option bitmask | `0..63` |
| SetOp | Ancient (set) option | `0` = none |
| JoH | Harmony option byte | `0` = none |
| OEx | +380 option byte | `0` = none |
| Socket1..Socket5 | Socket options (only on socket items) | `255` = no socket |
| Time | Duration in **seconds** | `0` = permanent; `259200` = 3 days |

### Section 1 - Starting buffs

| Column | Meaning | Values |
|---|---|---|
| Class | Character class | `0..6` as above |
| Effect | Effect index from `Data/Effect.txt` | |
| Power1, Power2 | Effect values | |
| Time | Duration in **minutes** | `4320` = 3 days |

## Example

```
0
//Class Section Type Level Dur Skill Luck Option ExcOp SetOp JoH OEx S1  S2  S3  S4  S5  Time
  1     7       1    11    0   0     1    0      0     0     0   0   255 255 255 255 255 259200   // DK: Dragon Helm +11, 3 days
  1     8       1    11    0   0     1    0      0     0     0   0   255 255 255 255 255 259200   // DK: Dragon Armor +11
  1     0       11   11    0   0     1    0      0     0     0   0   255 255 255 255 255 259200   // DK: weapon
  1     12      134  0     0   0     0    0      0     0     0   0   255 255 255 255 255 259200   // DK: wings
end

1
//Class Effect Power1 Power2 Time
  1     40     30     0      4320     // DK: 3-day buff
end
```

As shipped: every class gets a temporary (3-day) +11 set, a weapon or shield and class wings, plus four 3-day buffs (effects 40, 78, 46, 35).

## Rewards

Items are created in the inventory (one database insert per row); buffs are applied on the spot. The character is then flagged in the database so it never receives the kit again.

## Commands related

None.

## Pitfalls

- Only the **first login** of a character triggers the kit. Characters created before you edited the file will not get the new items.
- The inventory must have room for all items; rows are delivered one by one and an item that does not fit is simply not delivered. A new character has an empty inventory, so this only matters for very large kits.
- Use one row per item and per class; there is no "all classes" wildcard.
- The loader also accepts a Section `2` (Class, WC, WP, GP coin bonus), but the current GameServer does **not** pay it. Do not rely on starting coins from this file.
- Item `Time` is in seconds, buff `Time` is in minutes.

## Reload module

`/reload custom` (admin) re-reads `CustomStartItem.txt`. The new kit applies to characters logging in for the first time after the reload.
