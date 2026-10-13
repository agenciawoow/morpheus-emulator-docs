# Custom Monster

Make specific monsters stronger or weaker, give coins and a server-wide announcement when they die, and let them summon others on death.

## What it is

For each monster you list, the server changes its HP, damage, defense, accuracy and experience **as a percentage of its normal values** (from `Data/Monster/Monster.txt`). When it dies:

* A message can be announced to the whole server, for example `Player Hero defeated Gorgon!`.
* The player who did the most damage can receive WCoinC, WCoinP and/or GoblinPoint, with a personal notice.
* It can summon other monsters at the spot where it died.

The shipped file tunes about 45 bosses and golden monsters (Gorgon, Red Dragon, Golden Budge Dragon, Kundun, Selupan, Medusa, ...) and gives each a defeat announcement.

## Enable it

There is no on/off key: every row in `CustomMonster.txt` applies. Related settings:

| File | Key / entry | Meaning |
|---|---|---|
| `Data/Lang/NNN - Xxx_Message.txt` | Indexes used in `KillMessage` / `InfoMessage` | The texts, translated per player language. Shipped kill messages use indexes 202..207, 322 and 5000..5034. |
| `Data/Lang` | 730 / 731 / 732 | `%d WC/WP/GP added to your account!`, shown for each coin credited. |

## Files & syntax

File: `Data/Custom/CustomMonster.txt`. One row per monster, then `end`. No sections. Each row has **17** columns.

| Column | Meaning | Values |
|---|---|---|
| `Index` | Monster class | Index from `Data/Monster/Monster.txt` |
| `Map` | Only on this map | Map number, `*` = every map |
| `MaxLife` | HP | **% of normal**, `100` = unchanged, `*` = 100 |
| `DamageMin` | Minimum damage (physical and elemental) | %, `*` = 100 |
| `DamageMax` | Maximum damage (physical and elemental) | %, `*` = 100 |
| `Defense` | Defense (physical and elemental) | %, `*` = 100 |
| `AttackRate` | Attack success rate | %, `*` = 100 |
| `DefenseRate` | Defense success rate | %, `*` = 100 |
| `ExperienceRate` | Experience and master experience given | %, `*` = 100 |
| `KillMessage` | Lang index announced to everyone on death. `%s` = top-damage player | Lang index, `*` = none |
| `InfoMessage` | Lang index sent only to the top-damage player. `%d %d %d` = the three coin amounts | Lang index, `*` = none |
| `RewardValue1` | WCoinC given to the top-damage player | Number, `0` / `*` = none |
| `RewardValue2` | WCoinP given | Number, `0` / `*` = none |
| `RewardValue3` | GoblinPoint given | Number, `0` / `*` = none |
| `SummonMonster` | Monster class summoned on death | Monster index, `*` / `0` = none |
| `SummonMonsterCount` | How many to summon | Number, `*` = 1 |
| `SummonMonsterRate` | Chance to summon | 0..100 % |

{% hint style="warning" %}
Every stat column is a **percentage**, even though the header says `MaxLife`, `Defense`, and so on. `20000` in `MaxLife` means 200 times the normal HP, and `8` means 8% of it. Read the shipped values that way before copying them.
{% endhint %}

Rows are checked in order and the first one that matches the monster and map wins. Put map-specific rows **before** a `*` row for the same monster.

## Example

```
//Index Map MaxLife DamageMin DamageMax Defense AttackRate DefenseRate ExperienceRate KillMessage InfoMessage RewardValue1 RewardValue2 RewardValue3 SummonMonster SummonMonsterCount SummonMonsterRate
  55    *   500     150       150       120     100        100         300            202         *           0            0            50           *             *                  *    // Death King: 5x HP, 1.5x damage, 3x exp, 50 GP, announced
  43    *   100     100       100       100     100        100         200            5011        *           10           0            0            78            3                  50   // Golden Budge Dragon: 10 WC, 50% chance to summon 3 Golden Goblins
end
```

The shipped Lang text for index 202 is `Player %s defeated the Skeleton King!`. Use your own Lang index (and add it in every language file) to write a new announcement.

## Rewards

Coins go to the player who did the most damage, not the one who landed the last hit, and only if that is a player (not a summon). Each coin credited shows its own notice (Lang 730/731/732).

Summoned monsters appear around the dead monster and disappear after 30 minutes if nobody kills them.

## Client side

This server file only changes existing monster classes, so it needs no client change.

A completely new monster (new model) is a different job: it needs a new class in `Data/Monster/Monster.txt` and, on the client, the client custom files `CustomMonster.txt` (model, size, folder) and `CustomMonsterRenderMesh.txt` (render effects), edited in the Client Hub. Despite the same name, the client `CustomMonster.txt` has a completely different format. See [Client custom files](../client/custom-files.md).

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload custom` | Admin | Re-reads `CustomMonster.txt`. |
| `/reload monster` | Admin | Re-reads `Monster.txt` and `MonsterSetBase` and respawns monsters. |
| `/reload lang` | Admin | Re-reads the kill/info messages. |

## Pitfalls

* **Keep exactly 17 columns per row.** A missing or extra column shifts every following row. The server logs a warning with the line and writes the table it actually read to the general log on each load. Check it after editing.
* Stat changes apply when a monster **spawns**. Monsters already alive keep their old stats until they respawn (or after `/reload monster`).
* A `KillMessage` or `InfoMessage` index that does not exist in `Data/Lang` shows an empty or wrong text. Add the index to every language file.
* `InfoMessage` texts receive three numbers (WC, WP, GP) in that order. A text with `%s` there shows garbage.

## Reload module

* `/reload custom` re-reads `Data/Custom/CustomMonster.txt`. Rewards, messages and experience apply at once; HP, damage and defense apply to monsters that spawn afterwards.
* `/reload monster` respawns monsters so the new stats take effect everywhere.
* Client custom files (`CustomMonster.txt`, `CustomMonsterRenderMesh.txt`) never hot-reload: they must be [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).
