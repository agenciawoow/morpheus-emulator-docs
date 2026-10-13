# Custom Move

Create your own chat teleport commands (for example `/bar` or `/church`) that send the player to a fixed map and position, with level, reset, VIP and PK requirements.

## What it is

The player types the command you created in the chat (for example `/igreja`) and is teleported to the configured spot. If they do not meet a requirement they get a notice explaining why (minimum level, maximum reset, VIP only, PK not allowed, and so on). The same rules as the normal `/move` apply: no teleport while trading, dead, with a personal shop open or during another teleport; Atlans refuses Uniria/Dinorant; Icarus and Kanturu 3 require wings, Dinorant or Fenrir; Gens battle maps require a Gens family.

## Enable it

There is no on/off key. A command exists as soon as it has a row in the file.

## Files & syntax

File: `Data/Custom/CustomMove.txt`. A single list (no section number) closed by `end`. Maximum 20 commands (per the file header).

| Column | Meaning | Values |
|---|---|---|
| Index | Unique row id | `0, 1, 2...` (duplicates overwrite each other) |
| Command | The chat command, in quotes, including the slash | `"/bar"` (case-insensitive) |
| Map | Destination map number | e.g. `0` Lorencia, `2` Devias, `3` Noria |
| X | Destination X | `0..255` |
| Y | Destination Y | `0..255` |
| MinLevel | Minimum level | `*` = no limit |
| MaxLevel | Maximum level | `*` = no limit |
| MinReset | Minimum resets | `*` = no limit |
| MaxReset | Maximum resets | `*` = no limit |
| MinMReset | Minimum master resets | `*` = no limit |
| MaxMReset | Maximum master resets | `*` = no limit |
| AccountLevel | Minimum account level | `0` free, `1..3` VIP level |
| PKMove | Can a PK (PK level 5+) use it? | `0` no, `1` yes |

## Example

```
//INDEX Command     Map  X    Y    MinLevel MaxLevel MinReset MaxReset MinMReset MaxMReset AccountLevel PKMove
  1     "/igreja"   2    211  27   50       *        *        *        *         *         1            1   // Devias church, VIP 1+, level 50+
  2     "/bar"      0    124  132  1        *        *        *        *         *         1            1   // Lorencia bar, VIP 1+
  4     "/cm"       3    180  100  1        *        *        *        *         *         1            0   // Noria, PKs not allowed
end
```

As shipped, every command requires `AccountLevel 1` (VIP). Set it to `0` to open a command to free accounts.

## Commands related

| Command | Who | What |
|---|---|---|
| Any name defined in the file | Player (per AccountLevel column) | Teleports to the row's map/X/Y |
| `/move <map>` | Player | Normal move list (`Data/Move/Move.txt`), not this file |

## Pitfalls

- Commands from `Data/Command.txt` are checked first. If you create a custom move with the same name as an existing command (for example `/move`), the custom move never runs.
- Only the first word typed is compared, so `/bar` and `/bar xyz` both work.
- `AccountLevel` is a **minimum**: `1` allows VIP 1, 2 and 3.
- The command name must include the `/` and be quoted.
- A [NPC Move](npc-customs.md#npc-move) row can teleport by clicking an NPC instead of typing.

## Reload module

`/reload move` (reloads Gate, Move, CustomMove and MoveSummon) or `/reload custom`. Both re-read this file; no restart needed.
