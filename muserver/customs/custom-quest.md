# Custom Quest

A sequential quest chain driven by the `/quest` command: each quest has requirements (stats, coins, items, monster kills) and rewards (coins, items, buffs, experience).

## What it is

The player types `/quest`. The server looks at the next quest in their chain (quest 1, then 2, then 3...) and checks its requirements:

- If something is missing, a notice says what (minimum level, resets, Zen, coins, VIP, rank, kills, items).
- If the quest needs monster kills, the first `/quest` starts the counter ("Quest 1: kill 1000 Spider, N left"). Each kill of that monster shows how many are left, then "Mission Completed!". The player types `/quest` again to turn it in.
- When everything is met, required items, Zen and coins are taken, the rewards are given, fireworks go off and the chain advances to the next quest.

When there is no next quest configured, the player sees "Congratulations, you completed all quests!". Progress (the last completed quest) is stored per character in the database.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomQuestSwitch` | `1` | `1` = `/quest` works. `0` = `/quest` does nothing. |
| `Data/Command.txt` | row `48 "/quest"` | enabled for all, GameMaster `0` | Command availability and its own costs/requirements. |

## Files & syntax

File: `Data/Custom/CustomQuest.txt`. Sections `0` to `6`, each closed by `end`. Every row starts with the **Quest** number. Quests are done in order `1, 2, 3...`; a quest "exists" if it has at least one row in Section 0, 1 or 2.

### Section 0 - Character requirements (max 1 row per quest)

| Column | Meaning | Values |
|---|---|---|
| Quest | Quest number | `1+` |
| Level | Minimum level | `*`/`0` = none |
| Reset | Minimum resets | `*`/`0` = none |
| MReset | Minimum master resets | `*`/`0` = none |
| Zen | Zen required **and charged** | `*`/`0` = none |
| WC / WP / GP | WCoinC / WCoinP / GoblinPoint required **and charged** | `0` = none |
| VipLvl | Minimum account level | `*` = none, `0..3` |
| RankUser | Minimum rank index from [Rank User](rank-user.md) | `*` = none |
| Kills | Minimum PvP kills | `*`/`0` = none |

### Section 1 - Required items (removed on completion)

| Column | Meaning | Values |
|---|---|---|
| Quest | Quest number | |
| Cat, Item | Item group and index | |
| Level | Exact item level | `*` = any |
| Luck | `0` without, `1` with | `*` = any |
| Skill | `0` without, `1` with | `*` = any |
| Qtd | Quantity | |

### Section 2 - Required monster kills (max 1 monster per quest)

| Column | Meaning |
|---|---|
| Quest | Quest number |
| Monster | Monster class (index in `Monster.txt`) |
| Qtd | How many must be killed |

### Section 3 - Coin reward

| Column | Meaning |
|---|---|
| Quest | Quest number |
| Wcoin | WCoinC given |
| WcoinP | WCoinP given |
| GP | GoblinPoint given |

### Section 4 - Item reward

| Column | Meaning | Values |
|---|---|---|
| Quest | Quest number | |
| Section, Type | Item group and index | |
| Level | Item level | `0..15` |
| Dur | Read but ignored (default durability is used) | |
| Skill | `0`/`1` | |
| Luck | `0`/`1` | |
| Option | Additional option step | `0..7` |
| ExcOp | Excellent option bitmask | `0..63` |
| SetOp | Ancient (set) option | `0`, `5`, `6`... |
| JoH | Harmony option byte | `0` = none |
| OEx | +380 option byte | `0` = none |
| Socket1..Socket5 | Socket options | `255` = no socket |
| Time | Duration in **seconds** | `0` = permanent; `259200` = 3 days |
| Class | Class allowed to receive this row | `*` all, `0` DW, `1` DK, `2` FE, `3` MG, `4` DL, `5` SU, `6` RF |

Write one row per class to give each class its own item.

### Section 5 - Buff reward

| Column | Meaning |
|---|---|
| Quest | Quest number |
| Effect | Effect index from `Data/Effect.txt` |
| Power1, Power2 | Effect values |
| Time | Duration in **minutes** |

### Section 6 - Experience reward

| Column | Meaning |
|---|---|
| Quest | Quest number |
| Experience | Experience given (not given while a Seal of Sustenance is active) |

{% hint style="warning" %}
The header of Section 6 lists `Experience LevelUpPoints Money BattlePassStart`, but the server reads only **Quest** and **Experience**. Use two columns per row.
{% endhint %}

## Example

```
0
//Quest Level Reset MReset Zen WC WP GP VipLvl RankUser Kills
  1     150   1     *      *   0  0  0  *      *        *     // level 150 and 1 reset
  2     200   1     *      *   0  0  0  *      *        *
end

1
//Quest Cat Item Level Luck Skill Qtd
  2     14  14   *     *    *     5       // quest 2 also takes 5 Jewels of Soul
end

2
//Quest Monster Qtd
  1     3       1000    // kill 1000 Spiders
  2     24      1000    // kill 1000 Worms
end

3
//Quest WC WP GP
  1     0  0  200
end

4
//Quest Section Type Level Dur Skill Luck Option ExcOp SetOp JoH OEx S1  S2  S3  S4  S5  Time   Class
  1     7       67   0     0   0     1    4      2     5     0   0   255 255 255 255 255 259200 1   // DK: Dragon helm, 3 days
  1     7       69   0     0   0     1    4      2     5     0   0   255 255 255 255 255 259200 0   // DW: Legendary helm, 3 days
end

5
//Quest Effect Power1 Power2 Time
end

6
//Quest Experience
  2     900000
end
```

## Rewards

Coins (Section 3), items filtered by class (Section 4, delivered to the inventory), buffs (Section 5) and experience (Section 6). The quest is not completed if the inventory has no room for the reward items.

## Commands related

| Command | Who | What |
|---|---|---|
| `/quest` | Player | Checks / turns in the next quest of the chain. |

An [NPC Command](npc-customs.md#npc-command) row with `"/quest"` lets players do this by clicking an NPC (messages then also appear in the NPC's speech bubble).

## Pitfalls

- The monster kill counter is **not saved**: it resets when the character logs out or changes server. Tell players to finish the hunt in one session (or use [NPC Quest](npc-customs.md#npc-quest), which saves it).
- Kills only count after the player typed `/quest` for that quest.
- Section 0 Zen and coins are charged, not just checked.
- Quest numbers must be continuous: a gap ends the chain ("completed all quests").
- `RankUser 0` requires the character to be inside some Rank User range; use `*` for no requirement.

## Reload module

`/reload custom` (admin) re-reads `CustomQuest.txt` and `CustomQuestSwitch`. Players' saved progress is not affected.
