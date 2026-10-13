# Doppelganger

On-demand party event (Double Goer): one party at a time stops waves of monsters walking down a path, with monster strength scaled to the party's level and size.

## What it is

* A party enters with a **Mirror of Dimensions** (item 14,111) or an **Access Ticket to Doppelganger** (item 13,125). One item is consumed. Up to 5 players, one party at a time.
* The server picks one of 4 Doppelganger maps at random.
* Monsters walk toward the end of the path. If **3** of them reach the end, the run fails.
* Mid-bosses (Slaughterer and Furious Slaughterer) drop a **silver chest** when killed. If the party keeps fewer than 3 monsters from passing and kills the Ice Walker, a **golden chest** appears at the end.
* Monster strength is scaled from the **highest level in the party** (normal level + master level, capped at 600) and from the **number of players** (1 to 5).

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `DoubleGoerEvent` | `0` | `1` = parties can enter |
| `GameServerInfo - Event.dat` | `DoubleGoerDifficultRate` | `70` | Global difficulty in %, on top of section 2: `100` = table values |

The GameServerCS ships with `DoubleGoerDifficultRate = 100` (event off).

## Files & syntax

`Data/Event/DoubleGoer.dat`, 3 sections. **Every row of the shipped file is commented out**: uncomment all three sections before enabling the event.

### Section 0 - Timings (one row, minutes)

| Column | Meaning | Values |
|---|---|---|
| NotifyTime | Wait before the monsters start walking | minutes, suggested `1` |
| EventTime | Duration of the run | minutes, suggested `10` |
| CloseTime | Closing phase (chests can still be opened) before players are sent out | minutes, suggested `4` |

### Section 1 - Monster AI per map

| Column | Meaning | Values |
|---|---|---|
| MonsterAIUnitNumber1 ... 4 | `MonsterAIUnit.txt` unit used on map 1 ... 4 | suggested `74 75 76 77` |

### Section 2 - Difficulty by party level and size

The row used is `Index = (highest party level - 1) / 10` (normal + master level, capped at 600), so Index `0` covers levels 1-10 and Index `59` covers 591-600. Inside the row, the column used is the number of players in the party (1 to 5).

| Column | Meaning | Values |
|---|---|---|
| Index | Level bracket | `0`-`59` |
| Level1 ... Level5 | Multiplier on monster level, for 1 ... 5 players | decimal |
| MaxLife1 ... MaxLife5 | Multiplier on monster HP | decimal |
| Damage1 ... Damage5 | Multiplier on monster min and max damage | decimal |
| Defense1 ... Defense5 | Multiplier on monster defense | decimal |

Final HP / damage / defense = base x multiplier x `DoubleGoerDifficultRate` / 100. Level = base x multiplier.

## Example

```
0
//NotifyTime   EventTime   CloseTime
1              10          4
end

1
//MonsterAIUnitNumber1   MonsterAIUnitNumber2   MonsterAIUnitNumber3   MonsterAIUnitNumber4
74                       75                     76                     77
end

2
//Index   Level1 Level2 Level3 Level4 Level5   MaxLife1 ... MaxLife5   Damage1 ... Damage5   Defense1 ... Defense5
0         1.00   1.00   1.00   1.00   1.00     1.00 2.00 3.00 3.80 4.80   1.00 2.00 3.00 3.70 4.70   1.00 2.00 3.00 3.70 4.50
end
```

Each row has 21 numbers: Index + 5 x 4 multipliers.

## Schedule

Doppelganger has no schedule: a party can enter whenever it is empty. It is not listed in [Schedule syntax](schedule-syntax.md).

## Rewards

There is no EXP or coin reward. Chests give one drop from an item bag chosen by the highest party level:

| Highest level | Silver chest bag | Golden chest bag |
|---|---|---|
| 1 - 100 | `078 - Double Goer Silver Chest 1.txt` | `083 - Double Goer Golden Chest 1.txt` |
| 101 - 200 | `079 - ... Silver Chest 2.txt` | `084 - ... Golden Chest 2.txt` |
| 201 - 300 | `080 - ... Silver Chest 3.txt` | `085 - ... Golden Chest 3.txt` |
| 301 - 400 | `081 - ... Silver Chest 4.txt` | `086 - ... Golden Chest 4.txt` |
| 401 and above | `082 - ... Silver Chest 5.txt` | `087 - ... Golden Chest 5.txt` |

All files are in `Data/EventItemBag/` (indices 78 to 87 in `Data/EventItemBagManager.txt`).

## Commands related

There is no command to start Doppelganger.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | Re-reads `DoubleGoer.dat` and the keys |
| `/reload eventitembag` | Admin | Re-reads the chest bags |

## Pitfalls

* With the shipped file (all commented) the times are `0` and every multiplier is `0`: monsters spawn with no HP. Uncomment all three sections before setting `DoubleGoerEvent = 1`.
* Keep all 60 rows of section 2 and all 21 columns per row. A missing column shifts every following value.
* Damage1-5 apply to both min and max damage.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `DoubleGoer.dat`. A run in progress keeps going; new values apply to the next monsters and runs.

{% hint style="warning" %}
Reload when no party is inside, so one run does not mix old and new values.
{% endhint %}
