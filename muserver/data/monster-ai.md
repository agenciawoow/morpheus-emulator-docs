# Monster AI and monster skills

## What it does

These files give **special behaviour** to certain monsters: the Crywolf Dark Elves and Balgass, Kanturu's Maya hands, Raklion's Selupan, Ballista and others. They control how those monsters move, attack, heal, flee or summon, and which special skills they cast.

Ordinary monsters do not need any of this: they use the default behaviour from `Monster.txt`.

{% hint style="warning" %}
These tables reference each other by number. They are shipped working for the official events. Change values (delays, rates, coordinates) freely, but add or remove rows only if you understand the whole chain below.
{% endhint %}

## Where it lives

```
MuServer/Data/Monster/
├── MonsterAIAutomata.txt     state machine (when to switch behaviour)
├── MonsterAIElement.txt      single actions (move here, attack, heal...)
├── MonsterAIUnit.txt         an AI "brain": one action per behaviour type
├── MonsterAIGroup.txt        groups of monsters spawned with an AI
├── MonsterAIRule.txt         time-based AI switches (shipped empty)
├── MonsterSkill.txt          which special skills a monster has
├── MonsterSkillUnit.txt      how a skill is cast (target, area, delay)
└── MonsterSkillElement.txt   what a skill does (buff, debuff, damage change)
```

How they connect:

```
MonsterAIGroup ──(StartAI / AI1..AI3)──► MonsterAIUnit ──(Automata)──► MonsterAIAutomata
                                               └──(AIClass* columns)──► MonsterAIElement
Monster.txt class ──► MonsterSkill ──(UnitN)──► MonsterSkillUnit ──(SlotN)──► MonsterSkillElement
```

---

## MonsterAIGroup.txt

Monsters spawned together under an AI (for example a Dark Elf leader and its guards in Crywolf).

| Column | Values | Meaning |
|---|---|---|
| `Number` | group id | Group number. |
| `Guid` | 0+ | Member id inside the group. |
| `Monster` | monster index | Monster class. |
| `Rank` | 0 or `*` | `0` = leader of the group. |
| `StartAI` | AI unit | AI unit used when the group starts. |
| `AI1` `AI2` `AI3` | AI unit | AI units the member can switch to. |
| `CreateType` | code | How the member is created. |
| `Map` / `X` / `Y` / `Dir` | position | Spawn position and facing (`*` = random). |
| `RegenType` | code | Respawn behaviour. |

```
//Number   Guid   Monster   Rank   StartAI   AI1   AI2   AI3   CreateType   Map   X      Y      Dir   RegenType
1          0      340       0      10        11    31    2     0            34    110    77     *     1
1          1      341       *      10        1     1     1     0            34    108    73     *     1
```

## MonsterAIUnit.txt

An AI "brain": for each behaviour type, which action (MonsterAIElement) to run.

| Column | Values | Meaning |
|---|---|---|
| `Index` | unit id | Referenced by AIGroup and AIRule. |
| `Name` | `"text"` | Name. |
| `DelayTime` | ms | Time between AI decisions. |
| `Automata` | automata id | State machine from MonsterAIAutomata. |
| `AIClassNormal` `AIClassMove` `AIClassAttack` `AIClassHeal` `AIClassAvoid` `AIClassHelp` `AIClassSpecial` `AIClassEvent` | element id or `*` | Action used in each state. `*` = none. |

```
//Index   Name              DelayTime   Automata   AIClassNormal   AIClassMove   AIClassAttack   AIClassHeal   AIClassAvoid   AIClassHelp   AIClassSpecial   AIClassEvent
1         "Monster General" 1700        1          1               8             3               5             4              *             *                *
```

## MonsterAIElement.txt

A single action.

| Column | Values | Meaning |
|---|---|---|
| `Index` | element id | Referenced by AIUnit. |
| `Name` | `"text"` | Name. |
| `Class` | 0–7 | Behaviour type (0 normal, 1 move, 2 attack, 3 heal, 4 avoid, 5 help, 6 special, 7 event). |
| `Type` | code | Exact action inside the class (e.g. move to a point, group summon, group heal). |
| `SuccessRate` | % | Chance the action runs when chosen. |
| `DelayTime` | ms | Delay after the action. |
| `TargetType` | code or `*` | Target selection. |
| `X` / `Y` | coordinate or `*` | Target point for move/position actions. |

```
//Index   Name              Class   Type   SuccessRate   DelayTime   TargetType   X      Y
2         "Item Move"       1       12     100           1000        2            121    36
```

## MonsterAIAutomata.txt

The state machine: from the current state, under which condition the monster moves to the next state.

| Column | Values | Meaning |
|---|---|---|
| `Number` | automata id | Referenced by AIUnit. |
| `Name` | `"text"` | Name (e.g. `"Go -> Attack"`). |
| `Priority` | number | Order in which transitions are tested. |
| `CurrentState` / `NextState` | state | 0 normal, 1 move, 2 attack, 3 heal, 4 avoid, 5 help, 6 special, 7 event. |
| `TransitionType` | code | Condition that triggers the transition (copy from an existing row). |
| `TransitionRate` | % | Chance the transition fires when the condition is true. |
| `TransitionValueType` / `TransitionValue` | code / number | Extra parameter of the condition (`*` = none). |
| `DelayTime` | ms | Delay before the transition. |

```
//Number   Name                 Priority   CurrentState   NextState   TransitionType   TransitionRate   TransitionValueType   TransitionValue   DelayTime
1          "Attack -> Avoid"    1          2              4           4                100              *                     10000             1000
```

## MonsterAIRule.txt

Switches a monster class to another AI unit on a schedule. Shipped empty.

| Column | Meaning |
|---|---|
| `Index`, `Monster`, `Name` | Rule id, monster class, name. |
| `AIUnit` | AI unit to apply. |
| `RuleCondition` | Condition code. |
| `WaitTime` / `ContinuanceTime` | Delay before applying and how long it lasts. |
| `Month` `Day` `Week` `Hour` `Minute` | When the rule applies (`*` = any). |

---

## MonsterSkill.txt

Which special skills a monster class can use: up to 10 pairs of `Type` + `Unit`.

| Column | Values | Meaning |
|---|---|---|
| `Monster` | monster index | Monster class. |
| `Name` | `"text"` | Name. |
| `Type1`/`Unit1` … `Type10`/`Unit10` | code / unit id | Skill type and the MonsterSkillUnit it uses. `*` = empty slot. |

```
//Monster   Name             Type1   Unit1   Type2   Unit2   Type3   Unit3 ...
340         "Dark Elf"       30      6       21      7       0       8     ...
```

## MonsterSkillUnit.txt

How a skill is cast.

| Column | Values | Meaning |
|---|---|---|
| `Index` | unit id | Referenced by MonsterSkill. |
| `Name` | `"text"` | Name. |
| `TargetType` | code | Who is targeted (copy the code from an existing unit with the same purpose). |
| `ScopeType` | code or `*` | Area shape. `*` = no area. |
| `ScopeValue` | tiles | Area size. |
| `Delay` | ms | Cast delay. |
| `Slot1`–`Slot5` | element id or `*` | MonsterSkillElement effects applied. |

```
//Index   Name                 TargetType   ScopeType   ScopeValue   Delay   Slot1   Slot2 ...
2         "Monster Skill Unit" 1            0           6            300     14      7     ...
```

## MonsterSkillElement.txt

What the effect does.

| Column | Values | Meaning |
|---|---|---|
| `Index` | element id | Referenced by MonsterSkillUnit. |
| `Name` | `"text"` | Name. |
| `Type` | code | Effect type (stun, poison, buff, debuff, summon...). |
| `SuccessRate` | % | Chance to apply. |
| `ContinuanceTime` | seconds | Duration. |
| `IncAndDecType` / `IncAndDecValue` | code / number | Which stat is raised or lowered, and by how much. |
| `NullifiedMagic` | skill or `*` | Skill that cancels it. |
| `CharacterClass` / `CharacterLevel` | number | Target restrictions (`0` = any). |

```
//Index   Name                    Type   SuccessRate   ContinuanceTime   IncAndDecType   IncAndDecValue   NullifiedMagic   CharacterClass   CharacterLevel
3         "Monster Skill Element" 3      50            10                2               50               *                0                0
```

---

## Rules & pitfalls

- A reference to an id that doesn't exist (unit, element, automata) makes that monster fall back to default or idle behaviour.
- Coordinates in AIElement and AIGroup are map tiles of the event map (Crywolf = 34). Moving them outside walkable tiles freezes the monsters.

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload monster
```
