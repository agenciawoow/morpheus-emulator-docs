# Quests

The server has two quest systems:

| System | Files | Used for |
|---|---|---|
| **Classic quests** | `Quest/Quest.txt`, `QuestObjective.txt`, `QuestReward.txt` | Class-change quests (2nd class, Marlon/Combo, 3rd class) and their stat-point rewards |
| **Quest World** | `QuestWorld/QuestWorld.txt`, `QuestWorldObjective.txt`, `QuestWorldReward.txt` | The Season 6 quest window (NPC quest chains, Gens quests, repeatable quests) |

Both use the same three-part structure: **quest** (who starts it and when) → **objectives** (what to bring/kill/pay) → **rewards**.

{% hint style="warning" %}
Quest texts, dialogs and the quest window are in the client. The server files decide requirements, objectives and rewards. Keep `Index`/`Group` numbers aligned with the client's quest data, or players see quests that can't be completed.
{% endhint %}

**Quest states** used by the `CurrentState` / `RequireState` columns:

| State | Meaning |
|---|---|
| `0` | Not started (normal) |
| `1` | Accepted / in progress |
| `2` | Finished |
| `3` | Cancelled / not available yet (classic quests use 3 as "start") |

**Class columns** — 15 flags in the same order as [Item.txt](items.md) (`DW DK FE MG DL SU RF GL RW SL GC KUN LEM KIN` + Alchemist). `0` = not for this class; `1` = base class; `2` = after 2nd class change; `3` = after 3rd class change.

---

## Quest.txt

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0+ | Quest number. Several lines with the same Index = alternative requirements (e.g. different per class). |
| `StartType` | 0 | `0` = started by talking to an NPC. |
| `MonsterClass` | NPC index | NPC that gives the quest. |
| `CurrentState` | state | State the quest must be in for this line to apply. |
| `RequireIndex` | quest or `*` | Another quest that must be in `RequireState` first. |
| `RequireState` | state or `*` | State required for `RequireIndex`. |
| `RequireMinLevel` / `RequireMaxLevel` | level or `*` | Level range. |
| Class flags | 0–3 | See above. |

```
//Index   StartType   MonsterClass   CurrentState   RequireIndex   RequireState   RequireMinLevel   RequireMaxLevel   DW DK FE MG DL SU RF ...
1         0           235            3              0              2              150               *                 1  1  1  0  0  1  0  ...
```

Quest 1 is given by NPC 235 at level 150+, after quest 0 is finished, to DW, DK, FE and SU.

## QuestObjective.txt

| Column | Values | Meaning |
|---|---|---|
| `#` (Number) | 0+ | Objective number inside the quest. |
| `Type` | 1, 2, 4 | `1` = bring an item, `2` = pay Zen, `4` = kill monsters. |
| `Index` | item / monster | Item index (Type 1) or monster class (Type 4). `0` for Zen. |
| `Quantity` | number | Items, Zen amount or kills. |
| `Level` `Option1` `Option2` `Option3` `NewOption` | numbers | Required item level / options (Type 1). |
| `DropMinLevel` / `DropMaxLevel` | level or `*` | Monster level range that drops the quest item. If `DropMinLevel` is `*`, `DropMaxLevel` is read as a **monster class** instead. |
| `ItemDropRate` | per 10,000 | Chance a matching monster drops the quest item (only while the player still needs it). |
| `RequireIndex` | quest | Quest this objective belongs to. |
| `RequireState` | state | Quest state in which this objective applies. |
| Class flags | 0–3 | Classes that get this objective. |

```
//#   Type   Index   Quantity   Level   Option1   Option2   Option3   NewOption   DropMinLevel   DropMaxLevel   ItemDropRate   RequireIndex   RequireState   DW DK FE ...
0     1      7191    1          0       0         0         0         0           45             60             600            0              1              1  1  1  ...
```

While quest 0 is in progress, monsters level 45–60 drop item 7191 (Scroll of the Emperor) with 6% chance until the player has 1.

## QuestReward.txt

| Column | Values | Meaning |
|---|---|---|
| `Number` | 0+ | Reward number. |
| `Type` | 1, 2, 4, 8, 16, 32 | `1` = stat points, `2` = 1st class change (to the 2nd class), `4` = hero bonus (extra level-up points), `8` = Combo skill, `16` = 2nd class change (to the 3rd class), `32` = 3rd class change (later seasons). |
| `Index` | number | Reward id (as in the client). For item-type rewards, the item index. |
| `Quantity` | number | Points or quantity. |
| `Level` … `NewOption` | numbers | Item options (item rewards). |
| `RequireIndex` | quest | Quest that gives this reward. |
| `RequireState` | state | State in which it is given (normally `1` → on completion). |
| Class flags | 0–3 | Classes that receive it. |

```
//Number  Type   Index   Quantity   Level ... RequireIndex   RequireState   DW DK FE ...
0         1      200     10         0     ... 0              1              1  1  1  ...
1         2      201     0          0     ... 1              1              1  1  1  ...
```

Quest 0 gives 10 stat points; quest 1 gives the class change to the 2nd class.

---

## QuestWorld.txt

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0+ | Quest number. |
| `Group` | number | Quest group (chain). Index + Group identify the quest. |
| `Number` | number | Step inside the chain. |
| `StartType` | 0 / 1 / `*` | `0` = started by talking to the NPC; `1` = started from the quest window or by another trigger; `*` = continuation step. |
| `MonsterClass` | NPC index | NPC of the quest. |
| `CurrentState` | state | Current state for this line. |
| `NextIndex1`/`NextState1` … `NextIndex3`/`NextState3` | quest / state or `*` | Up to three follow-up quests and the state they move to (choices in the dialog). |
| `RequireIndex` / `RequireGroup` / `RequireState` | numbers or `*` | Prerequisite quest and its state. |
| `RequireMinLevel` / `RequireMaxLevel` | level or `*` | Level range. |
| `RequireFamily` | Gens family or `*` | Required Gens (Duprian / Vanert). |
| `RequireCooldown` | days or `*` | Days before the quest can be repeated. |
| Class flags | 0–3 | Classes. |

```
//Index  Group  Number  StartType  MonsterClass  CurrentState  NextIndex1  NextState1  NextIndex2  NextState2  NextIndex3  NextState3  RequireIndex  RequireGroup  RequireState  RequireMinLevel  RequireMaxLevel  RequireFamily  RequireCooldown  DW DK FE ...
0        1      0       1          257           0             1           0           *           *           *           *           *             *             *             1                50               *              *                1  1  1  ...
```

## QuestWorldObjective.txt

| Column | Values | Meaning |
|---|---|---|
| `Number` | 0+ | Objective number. |
| `Type` | 1, 2, 4, 8, 16 | `1` = kill monsters, `2` = learn/use a skill, `4` = bring items, `8` = reach a level, `16` = special (talk/event). |
| `Index` | monster / skill / item | Target of the objective. |
| `Quantity` | number | How many. |
| `Level` … `NewOption` | numbers | Item options (Type 4). |
| `RequireIndex` / `RequireGroup` / `RequireState` | numbers | Quest it belongs to and the state. |
| Class flags | 0–3 | Classes. |

```
//Number  Type  Index  Quantity  Level  Option1  Option2  Option3  NewOption  RequireIndex  RequireGroup  RequireState  DW DK FE ...
0         4     7168   5         0      0        0        0        0          20            1             1             1  1  1  ...
```

Quest 20 of group 1: bring 5 Apples (7168).

## QuestWorldReward.txt

| Column | Values | Meaning |
|---|---|---|
| `Number` | 0+ | Reward number. |
| `Type` | 1, 2, 4, 8, 16 | `1` = experience, `2` = Zen, `4` = item, `8` = buff (effect), `16` = Gens contribution. |
| `Index` | item / effect | Item index (Type 4) or effect (Type 8). `0` otherwise. |
| `Quantity` | number | Amount (experience, Zen, items, contribution points). |
| `Level` … `NewOption` | numbers | Item options. |
| `RequireIndex` / `RequireGroup` / `RequireState` | numbers | Quest that gives it. |
| Class flags | 0–3 | Classes. |

```
//Number  Type  Index  Quantity  ...  RequireIndex  RequireGroup  RequireState
0         1     0      100       ...  20            1             1
1         2     0      1000      ...  20            1             1
```

Quest 20 of group 1 gives 100 experience and 1000 Zen.

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md). For server-side custom quests see [Custom Quest](../customs/custom-quest.md).

## Reload module

```
/reload quest
```
