# Morpheus Tournament

A one-on-one single-elimination tournament in the Colosseum that the server runs on its own: it opens registration, draws the bracket, calls the fighters, judges every match and pays the prizes.

## What it is

Players follow everything in the **Colosseum panel**, opened by clicking the Colosseum NPC (Lorencia 131/134 by default). There is no HUD button. The panel works on every client platform and has six tabs:

| Tab | What it shows |
|---|---|
| Bracket | The tournament tree, from the first match to the final |
| Matches | The current round, who entered and who advanced |
| Betting | Odds, your bets and your balance. Locked while betting is off (the default) |
| Ranking | Accumulated standings from the database |
| Rules | The format and the settings of **this** edition |
| History | Past editions, paginated: date, participants, champion and prize, final/semi/quarter brackets |

An edition runs like this:

1. **Countdown:** global notices `WarnMin` minutes before registration opens.
2. **Registration** (`RegisterSec`): players near the NPC press *Join* in the panel. The entry item is charged at that moment. Up to 64 fighters can register. Registration closes early when `MaxPart` is reached.
3. **Draw** (`DrawSec`): the bracket is drawn at random and rounded up to 4, 8, 16, 32 or 64 slots. Empty slots become **BYEs** (automatic pass). If fewer than `MinPart` registered, the edition is cancelled and entry items are returned.
4. **Matches**, one at a time on the same arena. Both fighters are teleported to their spawn points and get a personal notice. A briefing (phase, prize, rules) runs for `IntroSec`, then "FIGHT!". Each match is a single sudden-death fight: the first death loses.
5. Between matches of the same round the server waits `IntermSec`. Between rounds it waits `RoundIntervalSec`. The next two fighters get a personal warning `MatchWarnSec` before their match.
6. **Finish:** all entrants still in the arena gather in the center, the champion and runner-up are announced and paid, then the arena is emptied to `ExitGate`.

A match always records the winner and the reason: kill, walkover (no-show), disconnection, time limit by HP, time limit by damage, GM decision, BYE, or no winner.

Players who aren't fighting can press *Watch* in the panel and are teleported to the stands, which become a safe zone during the tournament. Anyone who walks into the ring is pushed back to the stands. Fighters who die or lose respawn in the stands.

A registration survives logging out **before** the tournament starts: the player is re-bound on reconnect. Once a fight is running, disconnecting is an immediate loss.

## Enable it

All keys are in `GameServer/Data/GameServerInfo - Event.dat`:

| Key | Default | Meaning |
|---|---|---|
| `SwitchMorpheusTournamentSystem` | `1` | Master switch. `0` turns everything off |
| `EnableColiseumTournament` | `1` | The 1x1 mode. **Both** this key and the master switch must be `1` for the tournament to run |
| `EnableMorpheusTournamentRegistration` | `1` | `0` closes registration, so every edition is cancelled for lack of entrants |
| `EnableMorpheusTournamentSpectator` | `1` | Read but not used by the current GameServer. Spectating is controlled by section 4 `Enable` |
| `EnableMorpheusTournamentRanking` | `1` | Saves editions, matches, rewards, bets and the cumulative ranking to the database. With `0` the tournament still runs, but the **Ranking** and **History** tabs stay empty. Code default is `0` if the key is missing |

## Files & syntax

File: `Data/Event/MorpheusTournament.dat`. Sections `0` to `11`, each closed by `end`. `*` means `-1` (no limit).

{% hint style="info" %}
Announcement texts are not in this file. They are lines 906-926 of `Data/Lang/<NNN> - <Lang>_Message.txt`, so each player reads them in their own language.
{% endhint %}

### Section 0: cycle timings (all in seconds, except WarnMin)

| Column | Meaning | Shipped |
|---|---|---|
| WarnMin | Minutes of global countdown before registration opens | `10` |
| RegisterSec | How long registration stays open | `100` |
| DrawSec | Pause after the draw before the first match | `5` |
| IntroSec | Briefing between the teleport and "FIGHT!" (not the no-show deadline) | `30` |
| FightSec | Maximum length of one fight | `180` |
| IntermSec | Pause between matches **of the same round**, and before the finish | `8` |
| FinishSec | Champion ceremony before the arena is emptied | `20` |
| CloseSec | Empty-arena pause before the next edition is scheduled | `30` |
| RoundIntervalSec | Pause between **rounds** | `1200` (20 min) |
| MatchWarnSec | Personal warning to the next two fighters, this many seconds before their match | `120` |

### Section 1: start times

`Year Month Day DoW Hour Minute Second`, one row per start, standard format. See [Schedule syntax](schedule-syntax.md). The shipped file starts every day at 20:00.

{% hint style="warning" %}
The comment in this section says `DayOfWeek: 0=Sun ... 6=Sat`. That is wrong. The scheduler is the same as every other event: **1 = Sunday ... 7 = Saturday**.
{% endhint %}

### Section 2: registration / spectator NPC

| Column | Meaning | Shipped |
|---|---|---|
| NpcClass | NPC monster class | `581` ("Morpheus") |
| NpcMap | NPC map | `0` (Lorencia) |
| NpcX | NPC X | `131` |
| NpcY | NPC Y | `134` |

All four must match **exactly** a spawn in `Data/MonsterSetBase`. The shipped `000 - Lorencia.txt` has `581 00 00 131 134 03 //Morpheus Event`. If one field differs, the NPC acts like a normal NPC and the tournament looks like it doesn't exist. Panel actions (*Join*, *Watch*, *Bet*) only work within 5 tiles of this NPC.

### Section 3: arena

| Column | Meaning | Shipped |
|---|---|---|
| ArenaMap | Arena map. `-1` disables the tournament | `97` (Coliseum) |
| RingX1, RingY1, RingX2, RingY2 | Rectangle of the fighting ring. Used to push spectators out and as the default gathering point | `60 159 72 166` |
| P1X, P1Y, P1Dir | Spawn and facing direction (0-7) of fighter 1 | `66 166 1` |
| P2X, P2Y, P2Dir | Spawn and facing direction of fighter 2 | `66 159 5` |
| ExitGate | Gate (`Data/Move/Gate.txt`) everyone is sent to at the end, and after a reload | `17` (Lorencia) |
| CenterX, CenterY | Gathering point for the prize ceremony. `-1` = center of the ring rectangle | `-1 -1` |

### Section 4: spectators

| Column | Meaning | Shipped |
|---|---|---|
| Enable | `1` lets players watch from the stands, and losers respawn there. `0` = no spectating, losers go to `ExitGate` | `1` |
| Z1X1, Z1Y1, Z1X2, Z1Y2 | Stand 1 rectangle | `74 160 78 164` |
| Z2X1, Z2Y1, Z2X2, Z2Y2 | Stand 2 rectangle | `52 160 56 164` |
| SafeZoneAttr | `1` makes both stands a safe zone from the draw until the finish | `1` |
| SpecX, SpecY | Exact tile where spectators land and respawn. `-1` = center of stand 1 | `-1 -1` |

### Section 5: registration and eligibility

| Column | Meaning | Shipped |
|---|---|---|
| MinPart | Minimum entrants. Fewer cancels the edition and returns the entry items | `2` |
| MaxPart | Maximum entrants, up to `64`. Registration closes as soon as it fills | `16` |
| MinLvl / MaxLvl | Level range | `100` / `*` |
| MinRst / MaxRst | Reset range | `0` / `*` |
| MinMRst / MaxMRst | Master reset range | `0` / `*` |
| ReqItemIdx | Entry item, as `Category*512 + Index` | `*` |
| ReqItemLvl | Entry item level | `*` |
| ReqItemQty | How many are charged. `0` = free entry | `0` |

Players in a party, a guild war, a duel or a personal shop can't register.

### Section 6: walkover and time limit

| Column | Meaning | Shipped |
|---|---|---|
| WalkOverSec | Extra wait, counted from the teleport, only when a fighter is missing. After it, the absent side loses by W.O. | `120` |
| WalkOverIfOffline | `1` = a fighter who is offline counts as absent | `1` |
| WalkOverIfNotTeleport | `1` = a fighter who isn't on the arena map counts as absent | `1` |
| BothAbsentAction | Both fighters absent: `0` eliminate both, `1` reschedule the match (to the end of the queue), `2` wait for a GM (`/mtwin`) | `0` |
| TimeLimitAction | `FightSec` runs out: `0` eliminate both, `1` higher % HP wins, `2` more damage dealt wins, `3` wait for a GM (`/mtwin`). A tie in 1 or 2 eliminates both | `1` |
| MaxReschedule | Maximum reschedules of the same match (only with `BothAbsentAction 1`). After that the match has no winner | `1` |
| GMDecideTimeoutSec | Safety net: if no GM decides within this time, the match ends with no winner. `0` or less = 300 | `300` |

When a match ends with no winner, the next round treats that slot as a BYE. If the final has no winner, there is no champion.

### Sections 7, 8, 9: prizes

| Section | Who gets it |
|---|---|
| 7 | Champion |
| 8 | Runner-up (loser of the final) |
| 9 | **Every match won**, including walkovers and disconnections, but not BYEs |

Row format: `Coin1 Coin2 Coin3 ItemCount`, then `ItemCount` blocks of 12 numbers (up to 5 items).

| Column | Meaning |
|---|---|
| Coin1 / Coin2 / Coin3 | WCoinC / WCoinP / GoblinPoint |
| ItemCount | Number of item blocks that follow (max 5) |
| Cat, Index | Item category and index, written separately (for example `14 13` = Jewel of Bless) |
| Lvl | Item level |
| Dur | Durability / stack amount |
| Op1, Op2, Op3 | Skill, Luck, additional option level |
| New | Excellent options bitmask |
| Set | Ancient (set) option |
| JOH | Jewel of Harmony option |
| OpE | 380 option |
| Dur (last) | Time-limited item: seconds until it expires. `0` = permanent |

Items go straight into the inventory.

### Section 10: betting currency and limits

| Column | Meaning | Shipped |
|---|---|---|
| Currency | `0` Zen, `1` WCoinC, `2` WCoinP, `3` GoblinPoint | `0` |
| Min | Minimum bet. Values below 1 become 1 | `10000` |
| Max | Maximum bet. `0` = no maximum | `0` |

{% hint style="info" %}
The comment above section 10 says betting "doesn't exist yet". That note is outdated: betting is implemented and controlled by section 11.
{% endhint %}

### Section 11: betting behaviour

| Column | Meaning | Shipped |
|---|---|---|
| Enable | `1` turns betting on. `0` (default) shows the Betting tab locked | `0` |
| TaxPercent | House cut of the **losing** pool, `0`-`90` | `5` |
| CloseSec | Betting closes this many seconds before "FIGHT!" | `15` |
| Multiple | `0` = one live bet per match (can be cancelled and placed again). `1` = several bets on the same match | `0` |
| MaxPerPlayer | Maximum live bets per player. `0` or more than 8 = 8 | `4` |

How betting pays: bets open for one match at a time, as soon as both fighters are known. The losing pool, minus the tax, is shared among the winning bettors in proportion to their stake, and each winner also gets their own stake back. A win by walkover or disconnection **pays normally**. Bets are refunded only when the match has no winner, when one side has no bets at all, or on a reload.

{% hint style="danger" %}
Don't use `Currency 0` (Zen) on a live server. Zen payouts are only stored when the character is saved, so a GameServer crash between payout and save loses the money. Use `1`, `2` or `3`, which are written to the account at once.
{% endhint %}

## Example

```
0
//WarnMin RegisterSec DrawSec IntroSec FightSec IntermSec FinishSec CloseSec RoundIntervalSec MatchWarnSec
10        300         10      30       180      15        30        30       300              60
end

1
//Year  Month  Day  DoW  Hour  Min  Sec
*       *      *    7    20    00   00        // Saturdays at 20:00
end

2
//NpcClass  NpcMap  NpcX  NpcY
581         0       131   134
end

3
//ArenaMap RingX1 RingY1 RingX2 RingY2 P1X P1Y P1Dir P2X P2Y P2Dir ExitGate CenterX CenterY
97         60     159    72     166    66  166 1     66  159 5     17       -1      -1
end

4
//Enable Z1X1 Z1Y1 Z1X2 Z1Y2 Z2X1 Z2Y1 Z2X2 Z2Y2 SafeZoneAttr SpecX SpecY
1        74   160  78   164  52   160  56   164  1            -1    -1
end

5
//MinPart MaxPart MinLvl MaxLvl MinRst MaxRst MinMRst MaxMRst ReqItemIdx ReqItemLvl ReqItemQty
4         32      200    *      0      *      0       *       7181       0          1      // costs 1 Jewel of Bless +0
end

6
//WalkOverSec WalkOverIfOffline WalkOverIfNotTeleport BothAbsentAction TimeLimitAction MaxReschedule GMDecideTimeoutSec
120           1                 1                     0                1               1             300
end

7
//Coin1 Coin2 Coin3 ItemCount [Cat Index Lvl Dur Op1 Op2 Op3 New Set JOH OpE Dur]
5000    0     0     2          14  13    0   0   0   0   0   0   0   0   0   0     14  14  0  0  0  0  0  0  0  0  0  0
end

8
//Coin1 Coin2 Coin3 ItemCount
2500    0     0     1          14  13    0   0   0   0   0   0   0   0   0   0
end

9
//Coin1 Coin2 Coin3 ItemCount
500     0     0     0
end

10
//Currency Min  Max
1          10   500        // bets in WCoinC, 10 to 500 per bet
end

11
//Enable TaxPercent CloseSec Multiple MaxPerPlayer
1        5          15       0        4
end
```

## Schedule

Section 1, standard format: see [Schedule syntax](schedule-syntax.md). The time left shows in the event timer window. `/mtstart` adds a one-off start about 2 minutes ahead, so registration opens then (with the `WarnMin` countdown cut short). The one-off start lasts until the next `/reload event` or restart.

With the shipped `RoundIntervalSec 1200`, a 16-player edition has four rounds and takes over an hour. Lower it for shorter tournaments.

## Rewards

| Who | Configured in | Paid with |
|---|---|---|
| Champion | Section 7 | Coins (WC/WP/GP) and up to 5 items into the inventory |
| Runner-up | Section 8 | Same |
| Each match winner | Section 9 | Same, once per won match (not for BYEs) |
| Bettors | Sections 10-11 | Pool share in the betting currency |

If the player is **offline** when a prize is due, it isn't paid. The server log keeps a line with the name, account and prize so you can pay it by hand, and with `EnableMorpheusTournamentRanking = 1` it's also recorded in the database. No EventItemBag is used.

## Commands related

| Command | Who | What |
|---|---|---|
| `/mtstart` | Admin (`#GameMaster` 2) | Schedules a start about 2 minutes from now. The reply ("inscricoes abrem em instantes") is fixed Portuguese text |
| `/mtwin <1\|2>` | Admin (`#GameMaster` 2) | Decides a match that is waiting for a GM (`BothAbsentAction 2` or `TimeLimitAction 3`). `1` = the first fighter announced, `2` = the second. Outside that state it replies that no match is waiting |

## Pitfalls

* The NPC in section 2 must exactly match the `MonsterSetBase` spawn (class, map, X, Y). If you move one, move the other.
* The arena map (97, "Coliseum") must exist on the server (terrain file) **and** in the client. Changing `ArenaMap` to a map players don't have breaks the event.
* `EnableColiseumTournament = 0` turns the tournament off even if `SwitchMorpheusTournamentSystem = 1`.
* `WalkOverSec` is measured from the teleport, so values below `IntroSec` have no effect.
* With `BothAbsentAction 2` or `TimeLimitAction 3`, someone must be online to use `/mtwin`. Otherwise every such match waits `GMDecideTimeoutSec` and ends with no winner.
* Set an explicit `ReqItemLvl` when you charge an entry item. Refunds create the item with that exact level.
* With section 4 `Enable 0`, players can't watch, and losers are sent to `ExitGate`.
* The prize ceremony spreads entrants over 25 tiles around the center. With more than about 24 entrants still in the arena, some stand on the same tile.
* Never add extra columns to an existing section. A section with too many values swallows its `end` and the server closes at boot or on reload. New options go in new sections.

## Reload module

`/reload event` re-reads the switches and `Data/Event/MorpheusTournament.dat`, then schedules the next edition again. Unlike other events, a reload in the middle of an edition is handled safely: everyone in the arena is sent to `ExitGate`, the stand safe zone is removed, entry items are returned to online entrants, and live bets are refunded in the currency they were placed in. The edition is still cancelled, so reload between editions whenever you can.
