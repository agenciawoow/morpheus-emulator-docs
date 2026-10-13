# Events

Every event the GameServer can run: where its configuration lives, which switch turns it on, and the command that starts it by hand.

Each event can start on a schedule, from a GM command, or from the Morpheus control panel. Events with a schedule all use the same start-time format, described in [Schedule syntax](schedule-syntax.md).

## How events are configured

An event is controlled from three places:

| Layer | File | What it holds |
|---|---|---|
| On/off switch and global limits | `GameServer/Data/GameServerInfo - Event.dat` (some in `- Custom.dat`) | `XxxEvent = 1`, user limits, rates, quick rewards |
| Event data | `Data/Event/<Event>.dat` (or `Data/Custom/<Custom>.txt`) | Schedule, timings, levels, monsters, reward tables |
| Command permissions | `Data/Command.txt` | Who may use `/startbc`, `/startds`, and so on, plus cost and delay |

Item rewards come from the event item bags (`Data/EventItemBagManager.txt` and `Data/EventItemBag/`). Each event's page names the bag it uses.

## MU classic events

| Event | Config file | Enable key (default) | Start by hand | Page |
|---|---|---|---|---|
| Blood Castle | `Data/Event/BloodCastle.dat` | `BloodCastleEvent = 1` | `/startbc` | [Blood Castle](blood-castle.md) |
| Devil Square | `Data/Event/DevilSquare.dat` | `DevilSquareEvent = 1` | `/startds` | [Devil Square](devil-square.md) |
| Chaos Castle | `Data/Event/ChaosCastle.dat` | `ChaosCastleEvent = 1` | `/startcc` | [Chaos Castle](chaos-castle.md) |
| Illusion Temple | `Data/Event/IllusionTemple.dat` | `IllusionTempleEvent = 0` | `/startit` | [Illusion Temple](illusion-temple.md) |
| Kanturu | `Data/Event/Kanturu.dat` | `KanturuEvent = 1` | n/a | [Kanturu](kanturu.md) |
| Crywolf (GameServerCS) | `Data/Event/Crywolf.dat` | `CrywolfEvent = 0` | n/a | [Crywolf](crywolf.md) |
| Castle Siege (GameServerCS) | `Data/Event/MuCastleData.dat` | `CastleSiegeEvent = 0` | n/a | [Castle Siege](castle-siege.md) |
| Raklion | `Data/Event/Raklion.dat` | `RaklionEvent = 0` | n/a | [Raklion](raklion.md) |
| Imperial Guardian | `Data/Event/ImperialGuardian.dat` | `ImperialGuardianEvent = 1` | n/a | [Imperial Guardian](imperial-guardian.md) |
| Doppelganger | `Data/Event/DoubleGoer.dat` | `DoubleGoerEvent = 0` | n/a | [Doppelganger](doppelganger.md) |
| Castle Deep | `Data/Event/CastleDeepEvent.dat` | `CastleDeepEvent = 0` | n/a | [Castle Deep](castle-deep.md) |
| Invasions | `Data/Event/InvasionManager.dat` | `InvasionManagerSwitch = 1` | `/startinvasion <index>` | [Invasions](invasions.md) |
| Moss Merchant | `Data/Event/MossMerchant.dat` | `MossMerchantEvent = 0` | n/a | [Moss Merchant](moss-merchant.md) |
| Bonus Manager (EXP/drop bonus windows) | `Data/Event/BonusManager.dat` | `BonusManagerSwitch = 1` | n/a | [Bonus Manager](bonus-manager.md) |

All enable keys above are in `GameServerInfo - Event.dat`.

## Morpheus custom events

| Event | Config file | Enable key (default) | Start by hand | Page |
|---|---|---|---|---|
| Morpheus Tournament (Colosseum 1x1) | `Data/Event/MorpheusTournament.dat` | `SwitchMorpheusTournamentSystem = 1` + `EnableColiseumTournament = 1` | `/mtstart` | [Morpheus Tournament](morpheus-tournament.md) |
| King of MU | `Data/Event/BattleIce3.dat` | `BattleIce3Event = 1` | `/startking` | [King of MU](king-of-mu.md) |
| Guild vs Guild | `Data/Event/GvGEvent.dat` | `EventGvGSwitch = 1` | `/startgvg` | [GvG Event](gvg-event.md) |
| Team vs Team | `Data/Event/TvTEvent.dat` | `EventTvtSwitch = 0` | `/starttvt` | [TvT Event](tvt-event.md) |
| Find the Path | `Data/Event/FindPathNpc.dat` | `FindPathEventToNpc = 1` | `/startfindpath` | [Find the Path](find-the-path.md) |
| Quickly | `GameServerInfo - Event.dat` only | `QuicklyEvent = 1` | `/veloz` | [Quickly](quickly.md) |
| Hide and Seek | `GameServerInfo - Event.dat` only | `EventHideAndSeekSwitch = 1` | `/escesc` | [Hide and Seek](hide-and-seek.md) |
| Run and Catch | `GameServerInfo - Event.dat` only | `EventRunAndCatchSwitch = 1` | `/pegapega` | [Run and Catch](run-and-catch.md) |
| Russian Roulette | `GameServerInfo - Event.dat` only | `EventRussianRouletteSwitch = 1` | `/roletarussa` | [Russian Roulette](russian-roulette.md) |
| PvP 1x1 | `GameServerInfo - Event.dat` only | `EventPvPSwitch = 1` | `/pvp` | [PvP 1x1](pvp-1x1.md) |
| Kill All | `GameServerInfo - Event.dat` only | `EventKillAllSwitch = 1` | `/matamata` | [Kill All](kill-all.md) |
| Event Start (open event + join) | `GameServerInfo - Event.dat` only | `EventStartSwitch = 1` | `/abrirevento` | [Event Start](event-start.md) |

## Custom events configured in `Data/Custom`

These four run on a schedule like any event, but their files are in `Data/Custom` and their switches are in `GameServerInfo - Custom.dat`. They are documented with the customs.

| Event | Config file | Enable key (default) | Start by hand | Page |
|---|---|---|---|---|
| Custom Arena | `Data/Custom/CustomArena.txt` | `CustomArenaSwitch = 0` | `/startarena <index>` | [Custom Arena](../customs/custom-arena.md) |
| Event Drop (item rain) | `Data/Custom/CustomEventDrop.txt` | `CustomEventDropSwitch = 1` | `/startdrop` | [Event Drop](../customs/event-drop.md) |
| Online Lottery | `Data/Custom/CustomOnlineLottery.txt` | `CustomOnlineLotterySwitch = 1` | n/a | [Online Lottery](../customs/online-lottery.md) |
| Quiz | `Data/Custom/CustomQuiz.txt` | `CustomQuizSwitch = 1` | `/startquiz` | [Quiz](../customs/quiz.md) |

## Shared reference

| Topic | Page |
|---|---|
| Start-time rows (`Year Month Day DoW Hour Minute Second`) | [Schedule syntax](schedule-syntax.md) |
| Event names in the H-key event window and in announcements | [Event names](event-name.md) |
| Client-side event name list (`EventNameDisplay.txt`) | [Client custom files](../client/custom-files.md) |

## Enable it

1. Set the event's switch to `1` in `GameServerInfo - Event.dat` (or `- Custom.dat`).
2. Make sure its file has at least one uncommented schedule row if it should start by itself.
3. Run `/reload event` in game, or restart the GameServer.

PK players are kept out of Blood Castle, Chaos Castle and Devil Square unless you allow them in `GameServerInfo - Common.dat`:

| Key | Default | Meaning |
|---|---|---|
| `PkCanEnterBC` | `0` | `1` lets PK characters enter Blood Castle |
| `PkCanEnterCC` | `0` | `1` lets PK characters enter Chaos Castle |
| `PkCanEnterDS` | `0` | `1` lets PK characters enter Devil Square |

## Commands related

The start commands are admin commands (`#GameMaster = 2` in `Data/Command.txt`). Some events also have a player command to join or play:

| Command | Who | What |
|---|---|---|
| `/participar` | Player | Join the event opened with `/abrirevento` |
| `/queroir` | Player | Join Run and Catch |
| `/rei` | Player | Claim the throne in King of MU |
| `/resp <answer>` | Player | Answer the Quiz |
| `/mtwin <1\|2>` | Admin | Decide a stalled Morpheus Tournament fight |

## Pitfalls

* Crywolf and Castle Siege run only on the **GameServerCS** (Castle Siege server), so set their keys in that server's `GameServerInfo - Event.dat`.
* `/startbc`, `/startds` and `/startcc` don't bypass the switch. They add a one-off start time about two minutes ahead, which only fires when the event's switch is `1`. That one-off start lasts until the next reload or restart.
* Event names in `EventName.dat` and the client's `EventNameDisplay.txt` must use the same indexes, or the H-key window shows the wrong names.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and every `Data/Event/*.dat`, then resets every event's state and schedule. Use it between events: a running event is cut short. For the four `Data/Custom` events, run `/reload custom` first. See [Schedule syntax](schedule-syntax.md#reload-module).
