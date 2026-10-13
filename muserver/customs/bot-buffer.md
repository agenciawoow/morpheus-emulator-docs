# Bot Buffer

Character-looking NPCs ("bots") standing in town that give buffs to players for Zen or PC points, with optional VIP and Gens restrictions.

## What it is

The bots look like real characters of a class (with the wings or gear you choose) and stand at fixed spots, for example "[BUFF] ELF" next to the Lorencia fountain. The player **opens a trade** with the bot (trade request, as with a player). If the player meets the requirements, the bot casts its buffs on them (with the skill animation), charges the Zen / PC points and shows "Buff applied!" plus the duration of each buff. Otherwise the bot explains why (level too high, VIP only, not enough Zen, wrong Gens family...).

## Enable it

There is no key in `GameServerInfo`. The bots are spawned when the GameServer starts, from `BotBuffer.txt`. A bot exists when its Section 1 row has `Enabled = 1`.

## Files & syntax

File: `Data/Custom/BotSystem/BotBuffer.txt`. Four sections (`1` to `4`), each closed by `end`. Up to 100 bots (`BotIndex 0..99`).

{% hint style="info" %}
Only `BotBuffer.txt` is read. `BotBuffer1.txt` and `BotBuffer_normal.txt` in the same folder are unused backups; editing them has no effect.
{% endhint %}

### Section 1 - Bots

| Column | Meaning | Values |
|---|---|---|
| BotIndex | Bot id | `0..99` |
| Class | Character class/evolution code (appearance) | `0,1,2` DW/SM/GM; `16,17,18` DK/BK/BM; `32,33,34` Elf/ME/HE; `48,49,50` MG/DM; `64,65,66` DL/LE; `80,81,82` SU/BS/DS; `96,98` RF/FM |
| Enabled | Spawn this bot | `0` no, `1` yes |
| ColorName | Name color (PK level) | `0..6` |
| Zen | Zen charged per use | `0` = free |
| PCPoint | PC points charged per use | `0` = free |
| MaxLvl | Highest player level that may use it | e.g. `250` |
| Name | Bot name, in quotes (max 10 characters) | `"[BUFF] ELF"` |
| Map | Map number | |
| X, Y | Position | |
| Dir | Facing direction | `0..7` |

### Section 2 - Restrictions

| Column | Meaning | Values |
|---|---|---|
| BotIndex | Bot id | |
| Only_AL | VIP only | `0` everyone, any other value = free accounts refused |
| ReqGens | Enable the Gens checks below | `0` no, `1` yes |
| GensContribution | Minimum Gens contribution | |
| GensFamily | Bot's Gens family | `0` none, `1` Vanert, `2` Duprian |

### Section 3 - Equipment (appearance)

| Column | Meaning | Values |
|---|---|---|
| BotIndex | Bot id | |
| Slot | Equipment slot | `0` weapon, `1` shield, `2` helm, `3` armor, `4` pants, `5` gloves, `6` boots, `7` wings, `8` pet |
| Type | Item group | |
| Index | Item index | |
| Level | Item level (glow) | `0..15` |
| Opt | Additional option step | |

### Section 4 - Buffs (max 5 per bot)

| Column | Meaning | Values |
|---|---|---|
| BotIndex | Bot id | |
| Skill | Buff to cast (see list) | |
| Min | Duration in **minutes** | |
| %PowerSkill | Buff strength (value passed to the effect) | |

Supported buff codes: `16` Mana Shield, `27` Greater Defense, `28` Greater Damage, `48` Greater Life, `64` Critical Damage, `217` Damage Reflect, `82` Magic Circle, `129` Greater Ignore Defense Rate, `130` Fitness. Other codes are ignored.

## Example

```
1
//BotIndex Class Enabled ColorName Zen   PCPoint MaxLvl Name          Map X   Y   Dir
0          34    1       0         2000  0       250    "[BUFF] ELF"  0   156 129 1
1          2     1       0         2000  0       250    "[BUFF] SM"   0   159 129 1
end

2
//BotIndex Only_AL ReqGens GensContribution GensFamily
0          0       0       100              0
1          1       0       100              0     // SM bot: VIP only
end

3
//BotIndex Slot Type Index Level Opt
0          7    12   38    15    15     // Elf bot wears Wings of Illusion +15
1          7    12   37    15    15
end

4
//BotIndex Skill Min %Power
0          27    60  70     // Greater Defense, 60 minutes
0          28    60  70     // Greater Damage, 60 minutes
1          16    60  70     // Mana Shield
1          82    60  70     // Magic Circle
end
```

## Rewards

The buffs listed in Section 4, applied with the given duration and power. Cost: Zen and/or PC points from Section 1.

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload bots` | Admin | Removes the current bots and re-creates them from `BotBuffer.txt`. |

## Pitfalls

- Section 4 must exist and have at least one row: the whole bot system is switched on only when that section is read. Without it, no bot spawns.
- A BotIndex outside `0..99` or a Slot outside `0..8` stops the loading of the file with an error.
- More than 5 buffs on the same bot stops the loading of the remaining rows.
- Keep bot names at 10 characters or less (character name limit).
- Players above `MaxLvl` are refused ("You no longer need my help").
- Bots are not in MonsterSetBase: do not also spawn an NPC at the same spot.

## Reload module

`/reload bots` (admin) re-reads `Data/Custom/BotSystem/BotBuffer.txt` and respawns all bots. It is also loaded at server start and by `/reload all`.
