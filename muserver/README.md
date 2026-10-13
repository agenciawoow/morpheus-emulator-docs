# MuServer overview

The MuServer is the set of programs and configuration files that run your Morpheus world. This section explains what each program does, how they talk to each other, where their files are, and how to change them safely.

## The programs

| Program | Folder | What it does | Players connect to it? |
|---|---|---|---|
| **DataServer** | `DataServer/` | Reads and writes characters, inventories, guilds and rankings in the SQL database. Only GameServers talk to it. | No |
| **JoinServer** | `JoinServer/` | Validates account logins and tracks which accounts are online. Only GameServers talk to it. | No |
| **ConnectServer** | `ConnectServer/` | Shows the server list and sends each player to the GameServer they pick. | Yes (first connection) |
| **GameServer** | `GameServer/` | The game world: maps, monsters, items, events, customs and commands. You can run several. | Yes |
| **GameServerCS** | `GameServerCS/` | A GameServer built for the Castle Siege maps (Castle Siege, Land of Trials, Crywolf, Barracks, Refuge, Loren Market). | Yes (by map move) |
| **RelayServer** | `RelayServer/` | Bridges browser players (WebSocket) to the ConnectServer and GameServer (TCP). Only needed for browser play. | Yes (browser only) |
| **VoiceServer** | `VoiceServer/` | Party voice chat. Only needed if party voice is on. | Yes (voice only) |

Detailed pages:

- [ConnectServer](servers/connectserver.md)
- [DataServer](servers/dataserver.md)
- [JoinServer](servers/joinserver.md)
- [RelayServer and VoiceServer](servers/relay-and-voice.md)
- [GameServer](gameserver/README.md) and [GameServerCS](gameserver/castle-siege-server.md)

## How they connect

```
Player ──► ConnectServer ──(server list)──► Player picks a server
Player ──► GameServer ──► JoinServer   (login check)
                     └──► DataServer   (characters, items, guilds)
GameServer ──UDP──► ConnectServer      (online count, "I am alive")
JoinServer ──UDP──► ConnectServer      ("I am alive")
Browser player ──► RelayServer ──► ConnectServer / GameServer
```

If the JoinServer stops reporting to the ConnectServer, the server list comes back empty. If a GameServer stops reporting, it disappears from the list after about 10 seconds.

## Start order

Start the programs in this order and wait until each window shows it is listening before starting the next:

1. **DataServer**
2. **JoinServer**
3. **ConnectServer**
4. **GameServer** (all of them)
5. **GameServerCS**
6. **RelayServer** and **VoiceServer** (if you use browser play or party voice)

Stop them in the reverse order. Always close GameServers first so characters are saved through the DataServer.

{% hint style="info" %}
The control panel starts, stops and restarts these services for you, in the right order. See [Services and reload](../dashboard/services-and-reload.md).
{% endhint %}

## Folder layout

```
MuServer/
├── ConnectServer/   ConnectServer.exe + Data/ (ConnectServer.ini, ServerList.dat, Notice.dat)
├── DataServer/      DataServer.exe    + Data/ (DataServer.ini, AllowableIpList.txt, BadSyntax.txt)
├── JoinServer/      JoinServer.exe    + Data/ (JoinServer.ini, AllowableIpList.txt)
├── GameServer/      GameServer.exe    + Data/ (the 7 "GameServerInfo - *.dat" files)
├── GameServerCS/    GameServer.exe    + Data/ (its own 7 "GameServerInfo - *.dat" files)
├── RelayServer/     RelayServer.exe   + Data/RelayServer.ini
├── VoiceServer/     VoiceServer.exe   + Data/VoiceServer.ini
└── Data/            game data shared by every GameServer
    ├── Custom/  Event/  EventItemBag/  Item/  Monster/  MonsterSetBase/
    ├── Move/  Quest/  Shop/  Skill/  Util/  Lang/  Hack/  Terrain/ ...
    ├── Command.txt  MapManager.txt  MapServerInfo.dat  LangConfig.txt ...
```

There are two kinds of `Data` folder:

| Folder | Contains | Shared? |
|---|---|---|
| `<Program>/Data/` | The settings of that one program (ports, switches, rates) | No, one per program |
| `MuServer/Data/` (root) | The game content: items, monsters, spawns, drops, shops, events, customs, messages | Yes, every GameServer and GameServerCS reads the same root `Data/` |

So if you run two GameServers, each has its own `GameServerInfo - *.dat` (different rates, ports, names), but both read the same `Data/Item/Item.txt`, the same `Data/Custom/` files, and so on.

## How to edit the files

Use the control panel's **File editor**. It opens every file in the right encoding, checks the syntax before saving, keeps a backup of the previous version and can reload the GameServer for you. See [File editor](../dashboard/file-editor.md).

If you edit by hand (for example with Notepad++ on the server):

1. Make a copy of the file first.
2. Keep the file's encoding (most are Windows-1252, see [File format basics](file-format-basics.md)).
3. Save, then apply the change with the right reload or a restart.

Before editing any file for the first time, read [File format basics](file-format-basics.md). It explains sections, `end`, `*`, comments, quotes and item indexes, which are the same in almost every file.

## Applying changes

| You changed | How to apply |
|---|---|
| A `GameServerInfo - *.dat` file or a file in root `Data/` | A GameServer reload (`/reload <module>` in game, the **Reload** menu in the GameServer window, or the control panel). Each page in this guide says which module. |
| A setting that is read only at startup (ports, server code, connections) | Restart that GameServer |
| `ConnectServer/Data/ServerList.dat` | ConnectServer menu **Reload > Server List**, or restart the ConnectServer |
| Any other program's `.ini` | Restart that program |

The reload modules are listed in [GameServer overview](gameserver/README.md#reload-modules).

## Map of this documentation

| Section | What you find there |
|---|---|
| [File format basics](file-format-basics.md) | The syntax rules shared by all files |
| [Servers](servers/connectserver.md) | ConnectServer, DataServer, JoinServer, Relay, Voice |
| [GameServer](gameserver/README.md) | The 7 `GameServerInfo - *.dat` files key by key, Castle Siege server, multiple GameServers, languages |
| [Game data](data/README.md) | Items, monsters, spawns, drops, shops, moves, quests in root `Data/` |
| [Events](events/README.md) | Every event: switches, schedules and rewards |
| [Customs](customs/README.md) | Every Morpheus custom system |
| [Client](client/README.md) | Client-side files that must match the server |
