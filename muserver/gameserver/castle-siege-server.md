# Castle Siege server (GameServerCS)

## What it does

`GameServerCS` is a GameServer built for the Castle Siege side of the world. It hosts the maps where Castle Siege and Crywolf happen, and those events only run on it:

| Map | Number |
|---|---|
| Castle Siege (Valley of Loren) | 30 |
| Land of Trials | 31 |
| Crywolf Fortress | 34 |
| Barracks of Balgass | 41 |
| Balgass Refuge | 42 |
| Loren Market | 79 |

Players never pick it in the server list. When a player on a normal GameServer moves or warps to one of these maps, the server transfers them to GameServerCS automatically, and back again when they leave. This is set in `Data/MapServerInfo.dat` (see [Multiple GameServers](multiple-gameservers.md)).

## Where it lives

| Path | What |
|---|---|
| `GameServerCS/GameServer.exe` | The Castle Siege build of the GameServer. Do not replace it with the normal `GameServer.exe`: Castle Siege and Crywolf code is only in this build. |
| `GameServerCS/Data/GameServerInfo - *.dat` | Its own seven settings files |
| `Data/` (MuServer root) | Shared game content, including `Data/Event/MuCastleData.dat` (siege schedule and NPCs) and `Data/Event/Crywolf.dat` |

## How it differs from a normal GameServer

### Identity

| Setting | Normal GameServer | GameServerCS (shipped) |
|---|---|---|
| `ServerName` | `Geral` | `Siege` |
| `ServerCode` | 0 | 19 |
| `ServerPort` | 55901 | 55919 |
| `ServerList.dat` type | `"SHOW"` | `"HIDE"` |
| `MapServerInfo.dat` `InitSetVal` | 1 (hosts every map) | 0 (hosts only the maps listed for it) |

The code 19 is the last slot of group 0, so it does not collide with normal servers 0 to 18.

### Settings files

The CS server reads only its own `GameServerCS/Data/GameServerInfo - *.dat`. Changes to `GameServer/Data/` do not reach it. The shipped CS files differ from the normal ones:

| File | Main differences in the shipped CS copy |
|---|---|
| Common | No `HelperOffline*` block (offline helper off on CS maps), no `PartyVoice*` block (no party voice on CS maps), no `AccountLevelLock_AL*`, no `SpeedHackSkill*`, no `PkCanEnter*`, no `DuelBet`, no `SetLuckyReleaseLock`, no `DisableWingMix*`. Knockback removal (`Disable*Effect`) is 0, `EnableCsSkillsAllMaps = 0`, `PKTimePenaltyForKill = 0`, `DisablePotionCountDecrease = 0`. Adds `ALQLevelUpPoint` and `PkItemDropInventory`. |
| Character | Only the seven Season 6 classes. Every class-against-class rate is 100 (no class balance on CS maps). |
| ChaosMix | Same as the normal server. |
| Command | No reset and master reset coin rewards. |
| Custom | Same switches as the normal server. |
| Event | `CastleSiegeEvent = 1`. Blood Castle, Chaos Castle, Devil Square, Imperial Guardian, Kanturu, invasions and Bonus Manager are off. No premium events (Quickly, King of MU, Hide and Seek, Kill All, TvT, GvG, Morpheus Tournament...). Adds `CastleSiegeSeniorMixLimitType`, `CastleSiegeSeniorMixQuantityLimit`. |
| Skill | Only Season 6 classes in the class rates. No Party Heal, Bless, Blind, Earth Prison, Iron Defense or Blood Howling blocks. `LesserDefenseTimeConstB = 300`, `FireSlashTimeConstA = 20`. |

Every key missing from a CS file uses the built-in default (usually 0 = off). Each [GameServerInfo page](README.md#the-seven-files) documents the keys; the same tables apply to the CS copy.

{% hint style="warning" %}
If you want a feature to work on Castle Siege maps too (offline helper, party voice, a custom, a VIP rate), add or change the key in the **GameServerCS** copy as well. This is the most common reason for "it works everywhere except in Loren / Crywolf".
{% endhint %}

### Event settings that only matter here

Castle Siege and Crywolf keys are documented on [Event](event.md#castle-siege-event-settings). Castle Siege damage rates (`CastleSiegeDamageRate1..3`) are in [Character](character.md#character-damage-rate-settings) and must be set in the CS copy to take effect during the siege. The siege schedule, NPCs and rewards are in `Data/Event/MuCastleData.dat`; see [Castle Siege](../events/castle-siege.md).

## Example

`GameServerCS/Data/GameServerInfo - Common.dat`:

```ini
ServerName = Siege
ServerCode = 19
ServerPort = 55919
```

`ConnectServer/Data/ServerList.dat` (group 0):

```
19   "Castle Siege"   "GameServerCS"   "192.0.2.10"   55919   "HIDE"
```

`Data/MapServerInfo.dat`: see the full example in [Multiple GameServers](multiple-gameservers.md#example).

## Rules and pitfalls

- **Start GameServerCS after the normal GameServers.** Players cannot enter the siege maps while it is closed: the move fails and they stay where they are.
- **Same version and serial.** `ServerVersion` and `ServerSerial` in the CS Common.dat must match the client, like on the normal servers.
- **`CastleSiegeEvent` belongs in the CS copy.** Turning it on in the normal GameServer does nothing useful, because that server does not host map 30.
- **Forced dates** (`CastleSiegeCycleStart*`, `CastleSiegeStateStart*`) are for testing or fixing a cycle. Put them back to 0 afterwards, or the cycle is forced again at every restart.
- **The CS server needs its own `ServerCode` row** in `MapServerInfo.dat` and in `ServerList.dat`, or it refuses to start (`This GameServerCode (19) doesn't Exist`).

## Reload

Same modules as a normal GameServer (see [Reload modules](README.md#reload-modules)), run on the CS server: in game, the GM must be **on a CS map** for `/reload` to apply to GameServerCS. For Castle Siege schedule changes in `MuCastleData.dat`, prefer restarting GameServerCS outside the siege period: reloading events during a cycle can reset its state.
