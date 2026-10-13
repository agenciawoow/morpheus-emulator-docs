# Multiple GameServers

## What it does

You can run several GameServers on the same database: for example a normal server and a VIP-only server, a PvP and a non-PvP server, or separate servers per region. They share accounts, characters and the root `Data/` folder, but each has its own rates, switches and port. The Castle Siege server is one more GameServer of the same group.

Three files must agree for this to work:

| File | What it says about each GameServer |
|---|---|
| `GameServer*/Data/GameServerInfo - Common.dat` | Its own `ServerCode` and `ServerPort` |
| `ConnectServer/Data/ServerList.dat` | How it appears in the server list, and the address/port players connect to |
| `Data/MapServerInfo.dat` | Which maps it hosts and where to send players for the other maps |

## Where it lives

```
MuServer/
├── GameServer/       GameServer.exe + Data/GameServerInfo - *.dat   (ServerCode 0, port 55901)
├── GameServer2/      GameServer.exe + Data/GameServerInfo - *.dat   (ServerCode 1, port 55902)
├── GameServerCS/     GameServer.exe + Data/GameServerInfo - *.dat   (ServerCode 19, port 55919)
├── ConnectServer/Data/ServerList.dat
└── Data/MapServerInfo.dat
```

Every GameServer folder must sit directly inside the MuServer root, next to `Data/`: the program finds the shared game data at `..\Data\`.

## Server codes

The server code identifies a GameServer everywhere. In `ServerList.dat` it is written as a group number plus a position:

```
server code = group × 20 + position
```

| Group (section in ServerList.dat) | Codes |
|---|---|
| 0 | 0 to 19 |
| 1 | 20 to 39 |
| 2 | 40 to 59 |
| ... | ... up to group 9 |

`ServerCode` in `GameServerInfo - Common.dat` and the first column of `MapServerInfo.dat` use the **full** code (for example 20, not "group 1, position 0").

## MapServerInfo.dat

Table script with two sections. The map side of this file is also covered in [Maps](../data/maps.md#mapserverinfodat).

### Section 0: the GameServers

```
0
//ServerCode   MapServerGroup   InitSetVal   IpAddress        Port
0              0                1            S192.0.2.10      55901
end
```

| Column | Values | Description |
|---|---|---|
| `ServerCode` | 0 and up | Full server code. |
| `MapServerGroup` | 0 to 19 | Servers in the same group share map moves and global messages (`/post` with `CommandPostType = 4`). Use the same number as the ServerList group. |
| `InitSetVal` | -1, 0, 1 | Default for maps not listed in section 1. `1` = this server hosts every map. `0` = it hosts none; players going to those maps are sent to a server of the group that hosts them (random if several). `-1` = it hosts none; players are sent to a fixed server of the group that hosts them. |
| `IpAddress` | one letter + IP | The address clients are sent to when they move to this server. **The first character is skipped**, so write a prefix letter followed by the IP: `S192.0.2.10`. Use the public IP. |
| `Port` | port | The GameServer's `ServerPort`. |

### Section 1: map exceptions

```
1
//ServerCode   NotMoveOption   NextMap   NextServerCode
0              0               30        19
19             1               30        -1
end
```

| Column | Values | Description |
|---|---|---|
| `ServerCode` | code | The server this rule belongs to (must be in section 0). |
| `NotMoveOption` | 0, 1 | `1` = this server **hosts** the map (players stay). `0` = players going to this map are **sent** to `NextServerCode`. |
| `NextMap` | map number | The map the rule is about. |
| `NextServerCode` | code, -1, -2 | With option 0: the server to send players to. `-1` = any server of the group that hosts the map (random). `-2` = a fixed server of the group that hosts it. Ignored with option 1 (write -1). |

## Example

A normal server (code 0), a VIP server (code 1) and the Castle Siege server (code 19), all in group 0. Both normal servers send the Castle Siege maps to server 19.

`GameServer/Data/GameServerInfo - Common.dat`:

```ini
ServerName = Main
ServerCode = 0
ServerPort = 55901
```

`GameServer2/Data/GameServerInfo - Common.dat`:

```ini
ServerName = VIP
ServerCode = 1
ServerPort = 55902
AccountLevelLock_AL0 = 0
```

`ConnectServer/Data/ServerList.dat`:

```
0
//ServerCode   ServerClient      ServerName       ServerAddress    ServerPort   ServerType
0              "Main"            "GameServer"     "192.0.2.10"     55901        "SHOW"
1              "VIP"             "GameServer2"    "192.0.2.10"     55902        "SHOW"
19             "Castle Siege"    "GameServerCS"   "192.0.2.10"     55919        "HIDE"
end
```

`Data/MapServerInfo.dat`:

```
0
//ServerCode   MapServerGroup   InitSetVal   IpAddress       Port
0              0                1            S192.0.2.10     55901
1              0                1            S192.0.2.10     55902
19             0                0            S192.0.2.10     55919
end

1
//ServerCode   NotMoveOption   NextMap   NextServerCode
0              0               30        19
0              0               31        19
0              0               34        19
0              0               41        19
0              0               42        19
0              0               79        19
1              0               30        19
1              0               31        19
1              0               34        19
1              0               41        19
1              0               42        19
1              0               79        19
19             1               30        -1
19             1               31        -1
19             1               34        -1
19             1               41        -1
19             1               42        -1
19             1               79        -1
end
```

Maps 30, 31, 34, 41, 42 and 79 are Castle Siege, Land of Trials, Crywolf, Barracks, Refuge and Loren Market.

## Rules and pitfalls

- **Every running GameServer needs a row in section 0** of `MapServerInfo.dat`. Otherwise it stops at startup with `This GameServerCode (N) doesn't Exist`.
- **Different `ServerCode` and `ServerPort` for each GameServer.** Two servers with the same code fight over the same server-list slot; two with the same port cannot both start.
- **Forgetting the prefix letter** in `IpAddress` cuts the first digit of the IP, and players moving between servers get "connection failed".
- **Each GameServer has its own seven settings files.** A new GameServer folder copied from the first one starts with the same rates and switches; change what should differ.
- **Same version everywhere.** All GameServers must accept the same client (`ServerVersion`, `ServerSerial`).
- **One account, one server.** The JoinServer keeps an account on only one GameServer at a time.
- **Open every `ServerPort`** in the firewall.
- **Group limits:** up to 20 groups and 20 servers per group.

## Reload

| File | How to apply |
|---|---|
| `GameServerInfo - Common.dat` `ServerCode` / `ServerPort` | Restart that GameServer |
| `ServerList.dat` | ConnectServer **Reload > Server List** |
| `MapServerInfo.dat` | `/reload map` on **each** GameServer (or `/reload common`). With `/reload map`, a file with errors is rejected and the previous table stays in use; the error is logged in red. |
