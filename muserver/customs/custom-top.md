# Custom Top

The `/top <type> [class]` chat command: shows a top-players list (resets, master resets...) as notices, read live from the database.

## What it is

The player types, for example, `/top rr` or `/top rr dk`. The server prints the title of that top ("TOP RR (DK)") followed by the list of characters and their values, as notices in the chat area. With a class filter only that class is listed. If the type does not exist, the player sees the usage help (`/top type class`).

## Enable it

| File | Key / column | Default | Meaning |
|---|---|---|---|
| `Data/Command.txt` | row `51 "/top"` - Enable AL0..AL3 | `0 0 0 0` | **Disabled as shipped.** Set to `1` for the account levels that may use it. |
| `Data/Command.txt` | row `51 "/top"` - GameMaster | `2` | Shipped as admin-only. Set to `0` to let players use it. |

There is no key in `GameServerInfo`.

## Files & syntax

File: `Data/Custom/CustomTop.txt`. A single list (no section number), closed by `end` or end of file.

| Column | Meaning | Values |
|---|---|---|
| Index | Top type; passed to the database procedure | `1, 2...` |
| Command | Word typed after `/top`, in quotes | `"rr"` (case-insensitive) |
| Title | Title printed above the list, in quotes | `"TOP RR"` |

This file only defines names. The list itself comes from the SQL stored procedure `WZ_CustomTop`, called as `EXEC WZ_CustomTop <Index>, <class>` where class is `-1` for all classes or the class code (`0` DW, `16` DK, `32` FE, `48` MG, `64` DL, `80` SU, `96` RF). The procedure must return `VALUE1` (name) and `VALUE2` (value).

## Example

```
//Index Command Title
  1     "rr"    "TOP RESETS"
  2     "mr"    "TOP MASTER RESETS"
end
```

The shipped file has these two rows commented out, so `/top` has no types until you uncomment them.

## Commands related

| Command | Who | What |
|---|---|---|
| `/top <type> [class]` | As set in Command.txt (shipped: admin, disabled) | Prints the top list for `<type>`, optionally filtered by class: `dw`, `dk`, `fe`, `mg`, `dl`, `su` or `rf`. |

## Pitfalls

- Three things must line up: the `/top` row enabled in `Command.txt`, a row in `CustomTop.txt`, and a branch for that Index in `WZ_CustomTop`.
- Type words are limited to 9 characters; class must be lowercase (`dk`, not `DK`).
- An unknown class word is treated as "all classes".
- "No results!" means the procedure returned nothing for that Index/class.
- For a graphical ranking use the [Ranking window](ranking.md) instead.

## Reload module

`/reload custom` re-reads `CustomTop.txt`. `Command.txt` changes need `/reload command`. Procedure changes apply immediately.
