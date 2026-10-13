# GameServerInfo - Event

## What it does

The on/off switch and basic limits of every event. Schedules, rewards, maps and monsters of each event are in its own file in `Data/Event/` (for example `Data/Event/BloodCastle.dat`) and are explained on the event's page. See [Events](../events/README.md) for the full list and [Schedule syntax](../events/schedule-syntax.md) for the timetables.

## Where it lives

- `GameServer/Data/GameServerInfo - Event.dat`
- `GameServerCS/Data/GameServerInfo - Event.dat` (the Castle Siege server has its own copy with Castle Siege on and the other events off; see [Castle Siege server](castle-siege-server.md))

INI format, section `[GameServerInfo]`.

## Reload

`/reload event` re-reads this file and every file in `Data/Event/`, and re-initializes the event schedules.

{% hint style="warning" %}
Do not reload events while one is running: its state, players inside and timers are reset. Make event changes between events.
{% endhint %}

Coin rewards in this file are paid as **Coin1 = WCoinC, Coin2 = WCoinP, Coin3 = Goblin Points** (`AutoReward1`, `AutoReward2`, `AutoReward3`).

---

## Blood Castle Event Settings

Page: [Blood Castle](../events/blood-castle.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `BloodCastleEvent` | 0 | 0, 1 | `1` = Blood Castle on. |
| `BloodCastleMaxUser` | 0 | 1 to 15 | Maximum players per Blood Castle level. |

## Bonus Manager Settings

Timed bonuses (double experience, drop...) from `Data/Event/BonusManager.dat`. Page: [Bonus Manager](../events/bonus-manager.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `BonusManagerSwitch` | 0 | 0, 1 | `1` = bonus events on. |

## Castle Deep Event Settings

Page: [Castle Deep](../events/castle-deep.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CastleDeepEvent` | 0 | 0, 1 | `1` = Castle Deep invasion on. |
| `CastleDeepEventTime` | 0 | minutes | Duration of the event. |

## Castle Siege Event Settings

Only used by the Castle Siege server (GameServerCS). Page: [Castle Siege](../events/castle-siege.md) and [Castle Siege server](castle-siege-server.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CastleSiegeEvent` | 0 | 0, 1 | `1` = Castle Siege on. Set it in the **GameServerCS** copy. |
| `CastleSiegeCycleStartYear`, `...Month`, `...Day` | 0 | date, 0 = off | Force the start date of the siege cycle. `0` = follow the schedule in `Data/Event/MuCastleData.dat`. The date must not be so old that the whole cycle is already over, or the server shows an error at startup. |
| `CastleSiegeSpecificState` | 0 | 1 to 9 | Force the cycle into one stage: 1 register guilds, 2 idle, 3 register marks, 4 idle, 5 announce, 6 ready, 7 siege, 8 end of siege, 9 end of cycle. Used with the state date below. |
| `CastleSiegeStateStartYear`, `...Month`, `...Day`, `...Hour`, `...Minute` | 0 | date and time, 0 = off | When the forced stage starts. |
| `CastleSiegeOffensiveWeaponDamage` | 0 | damage | Damage of the attackers' catapult. |
| `CastleSiegeDefensiveWeaponDamage` | 0 | damage | Damage of the defenders' catapult. |
| `CastleSiegeLowerAccumulatedTimeValue` | 0 | ms | Time removed from the accumulated crown time when the registering guild master is interrupted. The crown needs 60,000 ms (60 s) of accumulated time. |
| `CastleSiegeDecayAccumulatedTimeValue` | 0 | ms | Accumulated crown time that decays periodically while nobody is registering. |

Optional keys read by the CS server (present only in the GameServerCS copy):

| Key | Default | Values | Description |
|---|---|---|---|
| `CastleSiegeSeniorMixLimitType` | -1 | -1, 0, 1+ | Limit on the castle senior's (NPC) mix: `-1` = once per day, `0` = no limit, `1` or more = up to `CastleSiegeSeniorMixQuantityLimit` mixes per day. |
| `CastleSiegeSeniorMixQuantityLimit` | 0 | number | Mixes per day when the type is 1 or more. |

`CastleSiegeGetCrownUserGuildLevel` in the CS file is not read.

## Chaos Castle Event Settings

Page: [Chaos Castle](../events/chaos-castle.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `ChaosCastleEvent` | 0 | 0, 1 | `1` = Chaos Castle on. |
| `ChaosCastleMinUser` | 0 | number | Minimum players for a Chaos Castle level to start. |
| `ChaosCastleBlowUserRate` | 0 | percent | Chance that a hit pushes the target back (and possibly off the platform). |

## Crywolf Event Settings

Crywolf runs on the Castle Siege server. Page: [Crywolf](../events/crywolf.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CrywolfEvent` | 0 | 0, 1 | `1` = Crywolf on. |
| `CrywolfBenefitSwitch` | 0 | 0, 1 | `1` = apply the benefit when the defense succeeds. |
| `CrywolfBenefitChaosRate` | 0 | percent | Chaos Machine success bonus while the benefit is active. |
| `CrywolfBenefitMonsterLife` | 0 | percent | Monster life multiplier while the benefit is active (`80` = monsters have 80 % life). |
| `CrywolfPenaltySwitch` | 0 | 0, 1 | `1` = apply the penalty when the defense fails. |
| `CrywolfPenaltyJewelDrop` | 0 | percent | Jewel drop rate while the penalty is active. |
| `CrywolfPenaltyExperience` | 0 | percent | Experience rate while the penalty is active. |

## Devil Square Event Settings

Page: [Devil Square](../events/devil-square.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `DevilSquareEvent` | 0 | 0, 1 | `1` = Devil Square on. |
| `DevilSquareMaxUser` | 0 | number | Maximum players per Devil Square level. |

## Double Goer Event Settings

Doppelganger. Page: [Doppelganger](../events/doppelganger.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `DoubleGoerEvent` | 0 | 0, 1 | `1` = Doppelganger on. |
| `DoubleGoerDifficultRate` | 0 | percent | Multiplies monster life and damage. `100` = normal. **0 makes monsters with 0 life and damage.** |

## Illusion Temple Event Settings

Page: [Illusion Temple](../events/illusion-temple.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `IllusionTempleEvent` | 0 | 0, 1 | `1` = Illusion Temple on. |
| `IllusionTempleMinUser` | 0 | even number | Minimum players to start; split between the two teams (each team needs half). |
| `IllusionTempleRewardFenrirRate` | 0 | per 10,000 | Chance of a Fenrir reward for the winners. `5` = 0.05 %. |

## Imperial Guardian Event Settings

Page: [Imperial Guardian](../events/imperial-guardian.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `ImperialGuardianEvent` | 0 | 0, 1 | `1` = Imperial Guardian (Fortress of Imperial Guardian) on. |
| `ImperialGuardianDifficultRate` | 0 | percent | Multiplies monster life and damage. `100` = normal. **0 makes monsters with 0 life and damage.** |

## Invasion Manager Settings

Monster invasions (Golden, White Wizard, Red Dragon...) from `Data/Event/InvasionManager.dat`. Page: [Invasions](../events/invasions.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `InvasionManagerSwitch` | 0 | 0, 1 | `1` = invasions on. |

## Kanturu Event Settings

Page: [Kanturu](../events/kanturu.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `KanturuEvent` | 0 | 0, 1 | `1` = Kanturu boss battle on. |

## Moss Merchant Event Settings

Page: [Moss Merchant](../events/moss-merchant.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `MossMerchantEvent` | 0 | 0, 1 | `1` = Moss the gambler appears on schedule. |
| `MossMerchantEventTime` | 0 | seconds | How long Moss stays after appearing. |

## Raklion Event Settings

Page: [Raklion](../events/raklion.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `RaklionEvent` | 0 | 0, 1 | `1` = Raklion (Selupan) boss battle on. |

## Quickly Event (Veloz - Premium)

The fastest player to type a word wins. Page: [Quickly](../events/quickly.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `QuicklyEvent` | 0 | 0, 1 | `1` = event on. |
| `QuicklyEventAutoReward1` | 0 | amount | WCoinC paid to the winner. |
| `QuicklyEventAutoReward2` | 0 | amount | WCoinP paid to the winner. |
| `QuicklyEventAutoReward3` | 0 | amount | Goblin Points paid to the winner. |

## Battle Ice3 Event

King of MU: guilds fight for a spot. Page: [King of MU](../events/king-of-mu.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `BattleIce3Event` | 0 | 0, 1 | `1` = event on. |
| `BattleIce3Map` | 0 | map number | Map of the battle. Shipped: 2 (Devias). |
| `BattleIce3CordX` / `BattleIce3CordY` | 0 | coordinates | Position of the spot. |
| `BattleIce3GuildMinMembers` | 0 | number | Minimum guild members to take part. |

## Finder Event (esconde esconde - Premium)

Find the Path: players find a hidden NPC. Page: [Find the Path](../events/find-the-path.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `FindPathEventToNpc` | 0 | 0, 1 | `1` = event on. |

## Event Start (Iniciar evento - Premium)

GM-started event with a countdown (`/abrirevento`, players join with `/participar`). Page: [Event Start](../events/event-start.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `EventStartSwitch` | 0 | 0, 1 | `1` = on. |
| `EventStartTime` | 0 | seconds | Join window after a GM opens the event at their position. The last 10 seconds are announced. Using the command again during the countdown cancels it. |

## Event Hide and Seek (Esconde-Esconde - Premium)

Page: [Hide and Seek](../events/hide-and-seek.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `EventHideAndSeekSwitch` | 0 | 0, 1 | `1` = on. |
| `EventHideAndSeekMaxTime` | 0 | minutes | Duration. |
| `EventHideAndSeekAutoReward1..3` | 0 | amount | WCoinC / WCoinP / Goblin Points paid to the winner. |

## Event Run And Catch (Pega-Pega - Premium)

Page: [Run and Catch](../events/run-and-catch.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `EventRunAndCatchSwitch` | 0 | 0, 1 | `1` = on. |
| `EventRunAndCatchTimeToEnter` | 30 | seconds | Registration window after the event opens. |
| `EventRunAndCatchMaxTime` | 0 | minutes | Duration. |
| `EventRunAndCatchAutoReward1..3` | 0 | amount | WCoinC / WCoinP / Goblin Points paid to the winner. |

## Event Russian Roulette (Roleta Russa)

Page: [Russian Roulette](../events/russian-roulette.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `EventRussianRouletteSwitch` | 0 | 0, 1 | `1` = on. |
| `EventRussianRouletteTimeToEnter` | 30 | seconds | Registration window. |
| `EventRussianRouletteMaxTime` | 0 | minutes | Duration. |
| `EventRussianRouletteMaxPlayer` | 0 | number | Maximum participants. |
| `EventRussianRouletteAutoReward1..3` | 0 | amount | WCoinC / WCoinP / Goblin Points paid to the winner. |

## Event PvP (1x1)

Page: [PvP 1x1](../events/pvp-1x1.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `EventPvPSwitch` | 0 | 0, 1 | `1` = on. |
| `EventPvPMaxScore` | 3 | number | Kills needed to win. |
| `EventPvPMaxTime` | 0 | minutes | Duration. |
| `EventPvPAutoReward1..3` | 0 | amount | WCoinC / WCoinP / Goblin Points paid to the winner. |

## Event Kill All (Mata-Mata Geral)

Free-for-all. Page: [Kill All](../events/kill-all.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `EventKillAllSwitch` | 0 | 0, 1 | `1` = on. |
| `EventKillAllTimeToEnter` | 0 | seconds | Registration window. |
| `EventKillAllDeadDecrease` | 0 | points | Points lost per death. |
| `EventKillAllKillIncrease` | 0 | points | Points won per kill. |
| `EventKillAllMaxScore` | 0 | points | Score that ends the event. |
| `EventKillAllMinPlayers` / `EventKillAllMaxPlayers` | 0 / 0 | number | Participants needed / allowed. |
| `EventKillAllMaxTime` | 0 | minutes | Duration. |
| `EventKillAllAutoReward1Rank1..3` | 0 | amount | WCoinC for 1st, 2nd and 3rd place. |
| `EventKillAllAutoReward2Rank1..3` | 0 | amount | WCoinP for 1st, 2nd and 3rd place. |
| `EventKillAllAutoReward3Rank1..3` | 0 | amount | Goblin Points for 1st, 2nd and 3rd place. |

## Event Team vs Team

Page: [TvT](../events/tvt-event.md). Schedule and rewards in `Data/Event/TvTEvent.dat`.

| Key | Default | Values | Description |
|---|---|---|---|
| `EventTvtSwitch` | 0 | 0, 1 | `1` = on. |
| `EventTvtNpc` | 0 | monster index | NPC players talk to in order to register. |
| `EventTvtNpcMap` | 0 | map number | Map of the NPC. |
| `EventTvtNpcX` / `EventTvtNpcY` | 0 | coordinates | Position of the NPC. |
| `EventTvtMinUsers` / `EventTvtMaxUsers` | 2 / 20 | number | Participants needed / allowed. |

## Event Guild vs Guild

Page: [GvG](../events/gvg-event.md). Schedule and rewards in `Data/Event/GvGEvent.dat`.

| Key | Default | Values | Description |
|---|---|---|---|
| `EventGvGSwitch` | 0 | 0, 1 | `1` = on. |
| `EventGvGNpc` | 0 | monster index | Registration NPC. |
| `EventGvGNpcMap` | 0 | map number | Map of the NPC. |
| `EventGvGNpcX` / `EventGvGNpcY` | 0 | coordinates | Position of the NPC. |
| `EventGvGMinUsers` / `EventGvGMaxUsers` | 2 / 20 | number | Participants needed / allowed. |

{% hint style="warning" %}
The shipped file has `EventGvGMinGuild` and `EventGvGMaxGuild`. Those names are **not read**. The server reads `EventGvGMinUsers` and `EventGvGMaxUsers` (default 2 and 20). Rename the keys if you want other limits.
{% endhint %}

## Morpheus Tournament System (Coliseum 1x1)

Only the switches are here. Arena, NPC, schedules, walkovers and prizes are in `Data/Event/MorpheusTournament.dat`. Page: [Morpheus Tournament](../events/morpheus-tournament.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `SwitchMorpheusTournamentSystem` | 0 | 0, 1 | Master switch of the system. |
| `EnableColiseumTournament` | 0 | 0, 1 | The 1x1 mode. Both this and the master switch must be 1 for the tournament to run. |
| `EnableMorpheusTournamentRegistration` | 1 | 0, 1 | `1` = players can register. |
| `EnableMorpheusTournamentSpectator` | 1 | 0, 1 | `1` = players can watch matches. |
| `EnableMorpheusTournamentRanking` | 0 | 0, 1 | `1` = save history, ranking and audit to the database. With 0 the tournament still runs, but nothing is recorded. |

---

## Example

Blood Castle and Devil Square on with 10 players each, Chaos Castle off, Kill All paying WCoinC to the top 3:

```ini
BloodCastleEvent = 1
BloodCastleMaxUser = 10
DevilSquareEvent = 1
DevilSquareMaxUser = 10
ChaosCastleEvent = 0

EventKillAllSwitch = 1
EventKillAllAutoReward1Rank1 = 100
EventKillAllAutoReward1Rank2 = 50
EventKillAllAutoReward1Rank3 = 25
```

## Rules and pitfalls

- **Switch on is not enough.** Each event also needs its `Data/Event/` file with a schedule. See [Schedule syntax](../events/schedule-syntax.md).
- **Castle Siege and Crywolf run on GameServerCS.** Turn them on in `GameServerCS/Data/GameServerInfo - Event.dat`, not in the normal GameServer.
- **Turn events off on the CS server** that should not run twice. The shipped CS copy already has Blood Castle, Chaos Castle, Devil Square, Kanturu, Imperial Guardian and invasions off.
- **Difficulty rates of 0** (`DoubleGoerDifficultRate`, `ImperialGuardianDifficultRate`) break the event. Use 100 for normal.
- `AutoReward = 0;` with a trailing `;` (as in the shipped file) is read correctly as 0.
