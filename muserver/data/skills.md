# Skills

| File | What it controls |
|---|---|
| `Skill/Skill.txt` | Every skill: damage, mana/AG cost, range, area, requirements, classes |
| `Skill/SkillDamage.txt` | Extra damage multiplier per skill, separately against players and monsters |
| `Skill/MasterSkillTree.txt` | The Master Skill Tree: nodes, values per level, prerequisites |
| `Skill/SkillElect.hit` | Binary hit-box table for area skills (do not edit) |
| `Skill/Skill_eng.txt`, `Skill/Skill_chs.txt` | Translated copies of `Skill.txt` names (not read by the GameServer) |

Skill formulas and per-class balance switches live in `GameServerInfo - Skill.dat` and in [Balance Class](../customs/balance-class.md).

---

## Skill.txt

### What it does

Defines every skill the server knows (player skills, Castle Siege skills, monster summon skills).

### Where it lives

`MuServer/Data/Skill/Skill.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | skill id | Skill number. Must match the client. |
| `Name` | `"text"` | Name. |
| `Damage` | number | Base skill damage. |
| `MP` | number | Mana cost. |
| `BP` | number | AG (stamina) cost. |
| `Range` | tiles | Cast distance. |
| `Radio` | tiles | Area radius (0 = single target). |
| `Delay` | ms | Cooldown between casts. |
| `Type` | code or `*` | Skill attribute/element used by the damage calculation. Keep as shipped. |
| `Effect` | effect id | Effect applied (index in [Effect.txt](effects-and-mining.md)), e.g. `55` = Poison. `0` = none. |
| `ReqLevel` | level | Level required. |
| `ReqEnergy` | number | Energy required. |
| `ReqLeadership` | number | Command required. |
| `ReqKillCount` | number | Kill count required (Dark Lord skills). |
| `ReqGuildStatus` | 0, 32, 64, 128 | Guild rank required (Castle Siege skills): 32 Battle Master, 64 Assistant, 128 Guild Master. `0` = none. |
| Class flags (15) | 0–3 | Same order and meaning as [Item.txt](items.md): `0` = can't use, `1` = base class, `2` = 2nd class, `3` = 3rd class. |

{% hint style="info" %}
The header of the shipped file only names the first seven classes, but each line has **15** class values. Keep all 15.
{% endhint %}

### Example (real lines)

```
//Index   Name             Damage   MP   BP   Range   Radio   Delay   Type   Effect   ReqLevel   ReqEnergy   ReqLeadership   ReqKillCount   ReqGuildStatus   DW DK FE MG DL SU RF ...
1         "Poison"         12       42   0    6       0       0       1      55       0          0           0               0              0                1  0  0  1  0  0  0  ...
41        "Twisting Slash" 0        10   10   0       3       0       5      0        0          0           0               0              0                0  1  0  1  0  0  0  ...
67        "Stern"          0        70   50   0       5       0       *      61       0          0           0               4              32               1  1  1  1  1  1  1  ...
```

- Poison: 12 damage, 42 mana, range 6, applies effect 55 (Poison), DW and MG.
- Stern (Castle Siege): needs 4 kills and the Battle Master rank.

### Rules & pitfalls

- The client shows its own skill data (icons, tooltips, mana). Changing costs here changes the real cost, not the tooltip.
- `Delay` too low lets macros spam the skill; combine with [HackSkillCheck](anti-hack.md).

---

## SkillDamage.txt

### What it does

A final multiplier on the damage of specific skills, **separately** for player targets and monster targets. Use it to balance PvP without touching PvM (or the reverse).

### Where it lives

`MuServer/Data/Skill/SkillDamage.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Skill` | skill id | Skill from `Skill.txt`. |
| `DamageRateUser` | % | Damage against **players**, in percent of the normal damage. `100` = unchanged. |
| `DamageRateMonster` | % | Damage against **monsters**, in percent. `100` = unchanged. |

Skills not listed deal 100% against both.

### Example (real lines)

```
//	#Skill	#DamageRateUser	#DamageRateMonster
	17      40              25   // Energy Ball
    4       45              35   // Fire Ball
```

Energy Ball deals 40% of its normal damage to players and 25% to monsters.

### Rules & pitfalls

- It is a **percentage of the final damage**, not a bonus: `40` means "reduce to 40%", `150` means "+50%".
- Only the first line of a skill is used.

---

## MasterSkillTree.txt

### What it does

Defines the Master Skill Tree nodes: in which tree and row each node sits, how many points it takes, its value at every level and what it requires.

### Where it lives

`MuServer/Data/Skill/MasterSkillTree.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | skill id | Master skill id (300+). |
| `Group` | 1–3 | Tree (left, middle, right column of the window). |
| `Rank` | 1–9 | Row (tier) inside the tree. |
| `MinLevel` / `MaxLevel` | 1–20 | Level range of the node (`1 20` for most nodes; some special nodes use `1 10` or `10 10`). |
| `MainValue1`–`MainValue20` | decimal | Value of the bonus at levels 1 to 20 (damage, %, defense... depending on the skill). Values are read as decimals (`18.06`). |
| `RelatedSkill` | skill id or 0 | Normal skill this node improves. |
| `ReplaceSkill` | skill id or 0 | Skill that this node replaces (enhanced version). |
| `RequireSkill` (×2) | skill id or 0 | Master skills that must be learned first. |
| Class columns (15) | slot | Position of the node in that class's tree window. `0` = this class doesn't have the node. |

### Example (real line, values shortened)

```
//Index   Group   Rank   MinLevel   MaxLevel   MainValue1   MainValue2 ... MainValue20   RelatedSkill   ReplaceSkill   RequireSkill   RequireSkill   DW DK FE MG DL SU RF ...
300       1       1      1          20         18.06        30.27      ... 135.24        0              0              0              0              1  1  1  1  1  1  0  ...
```

### Rules & pitfalls

- The client draws the tree from its own data. The class slot numbers must match the client, or players click a node and learn another.
- Raising `MainValue*` changes the bonus immediately for everyone who has the node.

---

## SkillElect.hit

Binary table with the hit area of directional skills. **Do not edit.** Replace it only with a file from the same server version.

## Skill_eng.txt and Skill_chs.txt

English and Chinese copies of `Skill.txt` (names translated, extra class columns). The GameServer does **not** read them; only `Skill.txt` is loaded.

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload skill
```

Reloads `GameServerInfo - Skill.dat`, `MasterSkillTree.txt`, `SkillElect.hit`, `Skill.txt` and `SkillDamage.txt`. `/reload all` includes it.
