# Combo

Skill combos for every class: you choose which skills open a combo and which skills continue it.

## What it is

In original MU only the Dark Knight has a combo, and only after a quest. With this system every class can trigger a combo: the player casts a **primary** skill, then two **different secondary** skills, each within 3 seconds of the previous one. The third skill lands as a combo hit with the combo damage bonus and effect.

The shipped file sets one or two sequences per class, for example:

| Class | Sequence (skill IDs) |
|---|---|
| BK | Cyclone -> Twisting Slash -> Rageful Blow (22 -> 41 -> 42) |
| SM | Energy Ball -> Lightning -> Aqua Beam (17 -> 3 -> 12) |
| Elf | Triple Shot -> Starfall -> Ice Arrow (24 -> 46 -> 51) |
| MG | Fire Slash -> Twisting Slash -> Flame Strike (55 -> 41 -> 236) |
| DL | Force -> Chaotic Diseier -> Fire Scream (60 -> 238 -> 78) |
| SUM | Lance -> Chain Lightning -> Lightning Shock (45 -> 215 -> 230) |
| RF | Charge -> Dark Side -> Dragon Roar (269 -> 263 -> 264) |

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Common.dat` | `EnableComboToAllSwitch` | `1` | `1` = every class can combo, using the skill lists of `CustomCombo.txt`, without the combo quest. `0` = original MU: only the BK skills, and only after finishing the combo quest. |
| `GameServer/Data/GameServerInfo - Character.dat` | `ComboDamageConstA` / `ComboDamageConstB` | `2` / `1` | Combo damage constants (how much extra damage the combo hit does). |

`CustomCombo.txt` is only used when `EnableComboToAllSwitch = 1`.

## Files & syntax

File: `Data/Custom/CustomCombo.txt`. Two sections, one skill ID per row. Skill IDs are the indexes of `Data/Skill/Skill.txt`.

### Section 0: primary skills (step 1)

| Column | Meaning | Values |
|---|---|---|
| `SkillID` | A skill that starts a combo | Skill index |

### Section 1: secondary skills (steps 2 and 3)

| Column | Meaning | Values |
|---|---|---|
| `SkillID` | A skill that continues a combo. Steps 2 and 3 must be two different skills from this list | Skill index |

There is no link between a primary and specific secondaries: any primary followed by any two different secondaries is a combo. The "sequences" above are simply what the shipped lists make possible for each class.

## Example

```
0
	22	//Cyclone             - BK/MG/DL (weapon skill: Blade, Knight Blade, ...)
	19	//Falling Slash       - axes, maces
	17	//Energy Ball         - SM
end

1
	41	//Twisting Slash      - BK/MG
	42	//Rageful Blow        - BK
	3	//Lightning           - SM
	12	//Aqua Beam           - SM
end
```

With these lists a Soul Master can do Energy Ball -> Lightning -> Aqua Beam, and a Blade Knight can do Cyclone -> Twisting Slash -> Rageful Blow.

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload custom` | Admin | Re-reads `CustomCombo.txt`. |
| `/reload common` | Admin | Re-reads `EnableComboToAllSwitch`. |

## Pitfalls

* The second and third skill must be **different**. Repeating the same secondary skill resets the combo.
* Each step must come within **3 seconds** of the previous one. Any skill that is not in either list breaks the combo.
* A skill must be in only one section. If it is in section 0, it always counts as a combo starter.
* Weapon skills (Cyclone, Falling Slash, Lunge, Uppercut, Slash) come from the equipped weapon, not from the character. List all of them in section 0 or players with some weapon types cannot start a combo.
* Slash (23) only exists on two-handed swords. A two-handed sword holder cannot have Cyclone at the same time, so a Cyclone -> ... -> Slash chain can never happen. The shipped file keeps 23 in section 1, which also means a two-handed sword holder has no primary skill. Move 23 to section 0 if you want them to combo.
* `EnableComboToAllSwitch = 0` ignores this file completely.

## Reload module

* `/reload custom` re-reads `Data/Custom/CustomCombo.txt`.
* `/reload common` re-reads `EnableComboToAllSwitch` from `GameServerInfo - Common.dat`.
* Both take effect immediately for the next skill cast.
