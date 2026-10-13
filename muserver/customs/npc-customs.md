# Custom NPCs

Four small systems that give behavior to NPCs you place in the world: the jewel Collector, NPCs that run a command, NPCs that teleport, and NPCs that hand out quests.

All four identify an NPC by **class + map + X + Y**. The NPC itself must be spawned by your `Data/MonsterSetBase` files at exactly that position; these files only add behavior to it. When a player clicks an NPC, the server checks, in this order: NPC Quest, NPC Command, NPC Move, then the NPC's normal function (shop, warehouse...). The Collector is tied to NPC class 236.

---

## NPC Collector

### What it is

The Golden Archer (NPC class 236) buys complete inventories of one item. The player fills the **entire 8x8 main inventory (64 slots)** with the same item, clicks the NPC and receives coins; all 64 items are removed and fireworks go off. If the inventory has anything else (or an empty slot), the NPC says it needs 64 pieces of the same type.

### Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomNpcCollectorSwitch` | `0` | `1` = the Collector trades. `0` = the NPC only says it is deactivated. |

### Files & syntax

File: `Data/Custom/CustomNpcCollector.txt`. Section `0`, closed by `end`.

| Column | Meaning | Values |
|---|---|---|
| Category | Item group | e.g. `14` |
| ItemIndex | Item index in the group | e.g. `13` (Jewel of Bless) |
| Cash | WCoinC paid for the 64 items | `0` = none |
| Gold | WCoinP paid | `0` = none |
| PcPoint | GoblinPoint paid | `0` = none |

### Example

```
0
//  Category  ItemIndex  WC  WP  GP
    14        13         0   0   10000   //64x Jewel of Bless -> 10000 GP
    14        14         0   0   10000   //64x Jewel of Soul  -> 10000 GP
    12        15         0   0   10000   //64x Jewel of Chaos -> 10000 GP
end
```

As shipped, every row is commented out and the switch is `0`, so the Collector does nothing until you enable both.

### Rewards

Coins only (WCoinC / WCoinP / GoblinPoint), added to the account.

### Pitfalls

- Only 1x1 items work: every one of the 64 slots must hold the item. A larger item leaves the other slots it covers "empty" and the trade is refused.
- The 64 items must be the exact same item (stack size is not counted, only slots).
- The NPC must be class `236` and must be spawned in MonsterSetBase.

---

## NPC Command

### What it is

Clicking the NPC runs a chat command for the player, exactly as if they had typed it: a "Reset NPC" that runs `/reset`, a "PK Clear NPC" that runs `/pk`, a teleporter that runs one of your [Custom Move](custom-move.md) commands, and so on. All requirements and costs of that command still apply.

### Enable it

No switch: a row in the file is enough.

### Files & syntax

File: `Data/Custom/CustomNpcCommand.txt`. A single list (no section number) closed by `end`.

| Column | Meaning | Values |
|---|---|---|
| Index | Unique row id | `0, 1, 2...` |
| Npc | NPC class (monster index) | e.g. `501` |
| NpcMap | Map where that NPC stands | map number |
| NpcX, NpcY | Exact NPC position | |
| NpcTalk | Show replies as the NPC's speech bubble too | `1` yes, `0` notices only |
| Command Text | The command to run, in quotes | `"/reset"`, `"/pk"` |

### Example

```
//Index  Npc  NpcMap  NpcX  NpcY  NpcTalk  Command
  0      501  0       126   143   1        "/reset"   //Reset NPC in Lorencia
  1      502  0       128   143   1        "/pk"      //PK Clear NPC
end
```

The shipped rows are commented out.

### Pitfalls

- The command must exist and be enabled in `Data/Command.txt` for the player's account level.
- Commands that need arguments cannot receive them from the NPC.
- If the same NPC also has an [NPC Quest](#npc-quest) row, the quest wins.

---

## NPC Move

### What it is

Clicking the NPC teleports the player to a fixed spot, with the same requirement checks as [Custom Move](custom-move.md) (level, resets, master resets, VIP, PK, mounts for Atlans/Icarus/Kanturu 3, Gens battle maps).

### Enable it

No switch: a row in the file is enough.

### Files & syntax

File: `Data/Custom/CustomNpcMove.txt`. A single list (no section number) closed by `end`.

| Column | Meaning | Values |
|---|---|---|
| Index | Unique row id | |
| Npc | NPC class | |
| NpcMap, NpcX, NpcY | Exact NPC position | |
| MoveMap, MoveX, MoveY | Destination | |
| MinLevel, MaxLevel | Level window | `*` = no limit |
| MinReset, MaxReset | Reset window | `*` = no limit |
| MinMReset, MaxMReset | Master reset window | `*` = no limit (see pitfall) |
| AccountLevel | Minimum account level | `0` free, `1..3` VIP |
| PKMove | Can a PK use it? | `0` no, `1` yes |

### Example

```
//Index  Npc  NpcMap NpcX NpcY  MoveMap MoveX MoveY  MinLvl MaxLvl MinRst MaxRst MinMR MaxMR  AL  PKMove
  0      501  0      126  143   2       222   62     50     *      *      *      *     *      0   1   //Lorencia -> Devias
end
```

The shipped rows are commented out.

### Pitfalls

- `MinMReset` is only checked when `MinReset` is not `*`, and `MaxMReset` only when `MaxReset` is not `*`. To limit by master reset, also set a numeric reset limit (for example `MinReset 0`).
- An NPC Command or NPC Quest row on the same NPC is checked first.

---

## NPC Quest

### What it is

Quest-giver NPCs. Each NPC row **is** a quest: clicking the NPC checks the quest's requirements (stats, coins, items, monster kills). If everything is met, the requirements are consumed, the rewards are given and fireworks go off. If a monster kill is required, the first click starts the kill counter and the player sees "kill N more monsters" notices while hunting; the counter is saved when they log out. A quest can be repeated up to `MaxQuest` times per character; after that the NPC either says the quest is complete or opens its normal function (`OpenNpc`).

### Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomNpcQuestSwitch` | `1` | `1` = NPC quests active. `0` = NPCs behave normally. |

### Files & syntax

File: `Data/Custom/CustomNpcQuest.txt`. Sections `0` to `7`, each closed by `end`. Every row starts with the **Quest** number, which links the sections together. Use quest numbers `1` and up (`0` or negative rows are ignored).

#### Section 0 - Quest NPCs

| Column | Meaning | Values |
|---|---|---|
| Quest | Quest number | `1+` |
| NpcIndex | NPC class | e.g. `231` |
| NpcMap, NpcX, NpcY | Exact NPC position | |
| MaxQuest | How many times a character can complete it | `0` = unlimited |
| OpenNpc | After the limit: open the NPC's normal function? | `0` = say "already completed", `1` = open the NPC normally |

#### Section 1 - Character requirements (max 1 row per quest)

| Column | Meaning | Values |
|---|---|---|
| Quest | Quest number | |
| Level | Minimum level | `*`/`0` = none |
| Reset | Minimum resets | `*`/`0` = none |
| MReset | Minimum master resets | `*`/`0` = none |
| Zen | Zen required **and charged** | `0` = none |
| WC / WP / GP | WCoinC / WCoinP / GoblinPoint required **and charged** | `0` = none |
| VipLvl | Minimum account level | `*` = none, `0..3` |
| RankUser | Minimum rank index from [Rank User](rank-user.md) | `*` = none |
| Kills | Minimum PvP kills | `*`/`0` = none |

#### Section 2 - Required items (removed on completion)

| Column | Meaning | Values |
|---|---|---|
| Quest | Quest number | |
| Cat, Item | Item group and index | |
| Level | Exact item level | `*` = any |
| Luck | `0` without, `1` with | `*` = any |
| Skill | `0` without, `1` with | `*` = any |
| Qtd | Quantity | |

#### Section 3 - Required monster kills (max 1 monster per quest)

| Column | Meaning |
|---|---|
| Quest | Quest number |
| Monster | Monster class (index in `Monster.txt`) |
| Qtd | How many must be killed |

#### Section 4 - Coin reward

| Column | Meaning |
|---|---|
| Quest | Quest number |
| WC | WCoinC given |
| WP | WCoinP given (the file header says "HP") |
| GP | GoblinPoint given |

#### Section 5 - Item reward

| Column | Meaning | Values |
|---|---|---|
| Quest | Quest number | |
| Section, Type | Item group and index | |
| Level | Item level | `0..15` |
| Dur | Read but ignored: the item gets its default durability | |
| Skill | `0`/`1` | |
| Luck | `0`/`1` | |
| Option | Additional option step | `0..7` |
| ExcOp | Excellent option bitmask | `0..63` |
| SetOp | Ancient (set) option | `0`, `5`, `6`... |
| JoH | Harmony option byte | `0` none |
| OEx | +380 option byte | `0` none |
| Socket1..Socket5 | Socket options | `255` = no socket |
| Time | Duration in **seconds** (temporary item) | `0` = permanent |
| Class | Class allowed to receive this row | `*` all, `0` DW, `1` DK, `2` FE, `3` MG, `4` DL, `5` SU, `6` RF |

The reward lands in the inventory; if there is no room for it the quest is not completed.

#### Section 6 - Buff reward

| Column | Meaning |
|---|---|
| Quest | Quest number |
| Effect | Effect index from `Data/Effect.txt` |
| Power1, Power2 | Effect values |
| Time | Duration in **minutes** |

#### Section 7 - Experience reward

| Column | Meaning |
|---|---|
| Quest | Quest number |
| Experience | Experience given (not given while a Seal of Sustenance is active) |

{% hint style="warning" %}
The header of Section 7 lists `Experience LevelUpPoints Money BattlePassStart`, but the server only reads **Quest** and **Experience**. Write two columns per row; extra columns break the parsing of the next rows.
{% endhint %}

### Example

```
0
//Quest  NpcIndex  NpcMap  NpcX  NpcY  MaxQuest  OpenNpc
  1      231       0       130   120   100       0    //Quest NPC in Lorencia
end

1
//Quest Level Reset MReset Zen WC WP GP VipLvl RankUser Kills
  1     *     1     *      0   0  0  0  *      *        *    //needs 1 reset
end

2
//Quest Cat Item Level Luck Skill Qtd
  1     14  14   *     *    *     5      //5 Jewels of Soul (removed)
end

3
//Quest Monster Qtd
  1     4       3000    //kill 3000 Elite Bull Fighters
end

4
//Quest WC WP GP
  1     0  0  500       //500 GoblinPoint
end

5
//Quest Section Type Level Dur Skill Luck Option ExcOp SetOp JoH OEx S1  S2  S3  S4  S5  Time   Class
  1     7       67   0     0   0     1    4      2     5     0   0   255 255 255 255 255 259200 1   //DK helm, 3 days
end

6
//Quest Effect Power1 Power2 Time
end

7
//Quest Experience
  1     900000
end
```

### Rewards

Coins (Section 4), items (Section 5, filtered by class), buffs (Section 6) and experience (Section 7). Zen and coins from Section 1 are charged; items from Section 2 are removed.

### Pitfalls

- Section 1 coin and Zen values are both a requirement and a **cost**.
- Use `*` for "no limit" in `VipLvl` and `RankUser`. In these two columns `0` is a real value: `RankUser 0` means "must hold rank 0 or higher", which fails for a character outside every Rank User range.
- Only one monster-kill quest is tracked at a time per character; starting another one saves the previous counter.
- Reward items need inventory space for all class-matching rows, otherwise nothing is given.
- Keep the NPC position identical to MonsterSetBase, or clicks fall through to the normal NPC.

---

## Commands related

| Command | Who | What |
|---|---|---|
| (none) | | These systems are triggered by clicking NPCs. NPC Command can run any command from `Data/Command.txt`. |

## Reload module

`/reload custom` (admin) re-reads `CustomNpcCollector.txt`, `CustomNpcCommand.txt`, `CustomNpcMove.txt`, `CustomNpcQuest.txt` and the switches in `GameServerInfo - Custom.dat`.

{% hint style="warning" %}
Reloading `CustomNpcQuest.txt` resets the in-memory quest and kill-counter state of every online character. Do it when few players are mid-quest.
{% endhint %}
