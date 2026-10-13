# Balance Class

One PvP damage formula for every class, fed by base stats, plus per-item bonuses that decide how much gear matters.

## What it is

In original MU each class turns its stats into damage with its own constant, so two characters with exactly the same stats hit differently just because of their class. With Balance Class on, every **player-versus-player** hit is calculated by one formula, identical for every class:

* Damage comes from the character's **base** stats (the points the player distributed). Stat bonuses from items and buffs do not count.
* Gear matters only through the **item table**: each equipped item listed there adds a % bonus, under conditions you choose (minimum level, excellent, ancient, 380, sockets, harmony, lucky).
* A per-class rate is available as a fine adjustment on top of the formula.

Damage against monsters is not changed (except HP, if you turn on the life option).

{% hint style="success" %}
**Nothing breaks when it is off.** While the switch is off, or if the file is missing, has no section 0 or cannot be read, the whole server uses MU's original calculation. The problem is written to the log and the GameServer keeps running.
{% endhint %}

The item bonus is recalculated when the player changes equipment, logs in, levels up or receives a buff, not on every hit, so it costs no performance.

Morpheus also ships a separate analysis tool that simulates every class matchup and proposes adjustments; it only suggests changes and you decide what to apply.

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Custom.dat`.

| Key | Default (shipped) | Meaning |
|---|---|---|
| `CustomBalanceClassSwitch` | `1` | Master switch. `0` = original MU damage everywhere. |
| `CustomBalanceClassLifeSwitch` | `0` | `1` = max HP and SD are calculated from `LifeBase`/`LifeVitDiv` and `ShieldBase`/`ShieldVitDiv`. **This also changes HP in PvM.** |
| `CustomBalanceClassItemBonus` | `1` | `1` = apply the item bonus table (section 2). `0` = gear adds nothing in PvP. |
| `CustomBalanceClassNoMiss` | `1` | `1` = PvP attacks never miss. |
| `CustomBalanceClassKeepEventRate` | `1` | `1` = keep the damage reductions that events apply on top of the formula. |

The file is read by both the GameServer and the Castle Siege GameServer.

## Files & syntax

File: `Data/Custom/CustomBalanceClass.dat`. Three sections.

### The formula

```
base   = Str/StrDiv + Agi/AgiDiv + Ene/EneDiv + Vit/VitDiv + Cmd/CmdDiv      (base stats only)
damage = random(base*MinRate/100 .. base*MaxRate/100)
damage = damage * (SklRate if a skill or combo, else NrmRate) / 100
damage = damage * ClassRate(attacker) / 100                              (section 1)
damage = damage * PetRate / 100                                          (only pet hits)
damage = damage * (100 + item bonus) / 100                               (section 2, capped at MaxItemBonus)
damage = damage - (DefBase + target Agi / DefAgiDiv), never below DmgFloor
```

### Section 0: formula settings (one row, required)

The row is written on two comment lines in the shipped file but it is one record of 20 values, in this order:

| Column | Meaning | Values |
|---|---|---|
| `StrDiv`, `AgiDiv`, `EneDiv`, `VitDiv`, `CmdDiv` | Stat points per 1 point of base damage | Number, `0` = that stat does not count |
| `MinRate` / `MaxRate` | Damage range around the base | % (shipped 90 / 110) |
| `NrmRate` | Multiplier for normal hits | % |
| `SklRate` | Multiplier for skill and combo hits | % |
| `PetRate` | Multiplier for pet hits (Dark Spirit, etc.) | % |
| `DefBase` | Flat defense subtracted from every hit | Number |
| `DefAgiDiv` | Target agility points per 1 point of defense | Number, `0` = agility gives no defense |
| `DmgFloor` | Minimum damage that always lands | Number (below 0 counts as 0) |
| `MaxItemBonus` | Cap of the total item bonus | % |
| `LifeBase` / `LifeVitDiv` | Max HP = `LifeBase + Vit/LifeVitDiv` (only with `CustomBalanceClassLifeSwitch = 1`) | `LifeBase` at least 1 |
| `ShieldBase` / `ShieldVitDiv` | Max SD = `ShieldBase + Vit/ShieldVitDiv` (only with the life switch). Both `0` = SD does not absorb PvP damage | Number |
| `DotDmg` | Fixed damage of each damage-over-time tick in PvP (poison, drain life, ...) | Number |
| `RefRate` | Reflected damage in PvP, as % of the reflected value | %, `0` = no reflect |

Without section 0 the feature stays off.

### Section 1: per-class rate

| Column | Meaning | Values |
|---|---|---|
| `Class` | Class number: 0=DW 1=DK 2=FE 3=MG 4=DL 5=SU 6=RF 7=GL 8=RW 9=SL 10=GC 11=KUN 12=LEM 13=KIN 14=ALQ | 0..14 |
| `Rate` | Damage multiplier for that class's attacks | %, `100` = no change |

A class without a row uses 100.

### Section 2: item bonus rules

Only items in equipment slots count (weapons, armor, wings, pendant, rings), not items in the inventory. **Every** rule that matches an equipped item adds its bonus, so an item can match several rules.

| Column | Meaning | Values |
|---|---|---|
| `Type` | Item type (group) | 0..15, `*` = any type |
| `Index` | Item index inside the type | Number, `*` = every item of the type |
| `MinLvl` | Minimum item level | 0..15, `*` = any |
| `Exc` | `1` = item must be excellent | `0` / `*` = not required |
| `Anc` | `1` = item must be ancient | `0` / `*` = not required |
| `380` | `1` = item must have the 380 option | `0` / `*` = not required |
| `Sock` | Minimum number of **filled** sockets | `0` / `*` = not required |
| `Harm` | `1` = item must have a Jewel of Harmony option | `0` / `*` = not required |
| `Luck` | `1` = item must be a **Lucky** item (Lucky set), not the Luck option | `0` / `*` = not required |
| `Bonus` | Damage bonus added | % |

## Example

```
0
//StrDiv AgiDiv EneDiv VitDiv CmdDiv  MinRate MaxRate  NrmRate SklRate PetRate
    62     62     62      0     62      90     110       100     100     30
//DefBase DefAgiDiv DmgFloor MaxItemBonus  LifeBase LifeVitDiv ShieldBase ShieldVitDiv DotDmg RefRate
     0        0         50       1200         5000       5          0          0        150     0
end

1
//Class  Rate
    3     115      // MG: no helm slot, compensate
    6     132      // RF: no shield and weaker gloves
end

2
//Type Index MinLvl Exc Anc 380 Sock Harm Luck  Bonus
     0    17     0     0   0   0    0    0    0    105      // Dark Breaker: +105%
     *     *    13     1   0   0    0    0    0     10      // any excellent item +13 or more: +10% extra
end
```

The shipped server uses the same divisor (62) for every damage stat, so a point is worth the same in any stat, and Vitality gives no damage (`VitDiv = 0`). Section 2 is a generated table that gives every item a bonus equal to its tier, so the five class versions of the same set add the same.

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload custom` | Admin | Re-reads `CustomBalanceClass.dat` and the `CustomBalanceClass*` keys. |

## Pitfalls

* After `/reload custom`, each online player's item bonus is reset to 0 until their attributes are recalculated (changing an item, relogging, levelling up or receiving a buff). Reload when few players are fighting, or ask them to relog.
* `CustomBalanceClassLifeSwitch = 1` changes max HP and SD for every player, in PvP **and** PvM. Test monsters against the new HP before turning it on.
* With the life switch on and `ShieldBase` and `ShieldVitDiv` both 0, SD stops absorbing PvP damage.
* Keep `MaxItemBonus` above the real sum of your best gear. When the total hits the cap it is cut silently, and classes with different gear end up equal again.
* `Luck` in section 2 means **Lucky items**, not the luck option.
* A `*` in a `Req` column (`Exc`, `Anc`, ...) means "not required", the same as `0`.
* Section 0 is one record of 20 numbers. A missing value shifts the rest; if reading fails, the feature turns itself off (check the log for `[CustomBalanceClass]`).

## Reload module

`/reload custom` re-reads `Data/Custom/CustomBalanceClass.dat` and the switches in `GameServerInfo - Custom.dat`. No client change is needed: the feature is server-only.
