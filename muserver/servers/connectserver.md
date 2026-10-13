# ConnectServer

## What it does

The ConnectServer is the first program a player's client talks to. It:

- shows the **server list** (the names the player picks from on the login screen);
- tells the client the address and port of the GameServer the player picked;
- limits how many connections come from the same IP address and the same device;
- optionally sends client patch files to out-of-date clients.

GameServers and the JoinServer report to the ConnectServer every few seconds over UDP. A GameServer that stops reporting disappears from the list after about 10 seconds. If the JoinServer stops reporting, the list is sent **empty** (players see no servers).

## Where it lives

| File | Purpose |
|---|---|
| `ConnectServer/Data/ConnectServer.ini` | Ports and connection limits |
| `ConnectServer/Data/ServerList.dat` | The server list |
| `ConnectServer/Data/Notice.dat` | Login notices (title, date, text) |
| `ConnectServer/Data/Update/` | Optional client patch files (see `ConnectServerVersion`) |

---

## ConnectServer.ini

### Syntax and keys

INI format, section `[ConnectServerInfo]`.

| Key | Default | Values | Description |
|---|---|---|---|
| `ConnectServerPortTCP` | 44405 | 1 to 65535 | TCP port the game client connects to. Must match the port configured in the client (and the RelayServer target for browser play). |
| `ConnectServerPortUDP` | 55557 | 1 to 65535 | UDP port where GameServers and the JoinServer send their "alive" reports. Must match `ConnectServerPort` in every `GameServerInfo - Common.dat` and in `JoinServer.ini`. |
| `ConnectServerVersion` | 0 | 0, or a version number | Client patch version. `0` = patch system off. When greater than 0, a client with a lower version receives every file in `ConnectServer/Data/Update/` from its version up to this one. |
| `MaxIpConnection` | 0 | 1 to 1000 | Maximum simultaneous connections from the same IP address. **0 refuses every connection.** |
| `MaxHWIDConnection` | 1 | 1 to 1000 | Maximum simultaneous connections from the same device (hardware ID). **0 refuses every connection.** |
| `RequireHardwareId` | 1 | 0, 1 | `1` = refuse clients that send an empty, malformed or legacy hardware ID. `0` = accept them (use only if some device cannot generate an ID). |

### Example

```ini
[ConnectServerInfo]
ConnectServerPortTCP = 44405
ConnectServerPortUDP = 55557
ConnectServerVersion = 0
MaxIpConnection = 100
MaxHWIDConnection = 5
RequireHardwareId = 1
```

### Rules and pitfalls

{% hint style="danger" %}
`MaxIpConnection = 0` or `MaxHWIDConnection = 0` does **not** mean "unlimited". It blocks everybody. A missing `MaxIpConnection` key also defaults to 0.
{% endhint %}

- **`MaxIpConnection` is anti-flood, not a per-player limit.** Ten phones on the same Wi-Fi share one IP. Mobile carriers put thousands of users behind a few IPs (CGNAT). And every browser player arrives from the RelayServer's IP. Keep it generous (the shipped value is 100).
- **`MaxHWIDConnection` is the per-device limit.** Use it to limit multi-clienting. The shipped value is 5.
- When a client is refused, the ConnectServer window logs `[HWID] recusado` with the reason (invalid ID or limit reached).
- Patch files in `Data/Update/` must be named `NNN - name` (three digits, space, dash, space). The number is the version that file belongs to.

### Reload

Restart the ConnectServer. The **Reload > Server Version** menu in the ConnectServer window re-reads only `ConnectServerVersion` and the `Update/` folder.

---

## ServerList.dat

### What it does

Defines the servers shown on the login screen and where each one is.

### Syntax

Table script with sections `0` to `9`. Each section is a **server group** (the client shows each group as a separate tab or block). Each row is one GameServer:

```
<group>
//ServerCode   ServerClient          ServerName      ServerAddress     ServerPort   ServerType
<code>         "<name shown>"        "<log name>"    "<ip>"            <port>       "SHOW|HIDE"
end
```

| Column | Values | Description |
|---|---|---|
| `ServerCode` | 0 to 19 | Number of the server inside its group. The real server code is **group × 20 + ServerCode**. |
| `ServerClient` | text, quoted | The name the player sees in the list. |
| `ServerName` | text, quoted | Internal name used in the ConnectServer log ("GameServer online/offline"). |
| `ServerAddress` | IP, quoted | The address the client will connect to. Use the **public** IP players can reach. |
| `ServerPort` | 1 to 65535 | The GameServer's TCP port (`ServerPort` in its `GameServerInfo - Common.dat`). |
| `ServerType` | `"SHOW"`, `"HIDE"` | `SHOW` = listed. Anything else (normally `"HIDE"`) = not listed but still usable for map moves, which is how the Castle Siege server is set up. |

The real server code (group × 20 + ServerCode) must be equal to `ServerCode` in that GameServer's `GameServerInfo - Common.dat`. Group 0 holds codes 0 to 19, group 1 holds codes 20 to 39, and so on.

### Example

```
0
//ServerCode   ServerClient          ServerName          ServerAddress      ServerPort   ServerType
0              "Server (PvP)"        "GameServer"        "192.0.2.10"       55901        "SHOW"
1              "Server (Premium)"    "GameServer2"       "192.0.2.10"       55902        "SHOW"
19             "Castle Siege"        "GameServerCS"      "192.0.2.10"       55919        "HIDE"
end

1
//ServerCode   ServerClient          ServerName          ServerAddress      ServerPort   ServerType
0              "Test Server"         "GameServer20"      "192.0.2.10"       55921        "SHOW"
end
```

Here the second group's server has the real code `1 × 20 + 0 = 20`, so its `GameServerInfo - Common.dat` must say `ServerCode = 20`.

### Rules and pitfalls

- A server only appears when it is `SHOW` **and** its GameServer is running and reporting to the ConnectServer UDP port.
- If the list is empty for everybody, check that the **JoinServer** is running and that its `ConnectServerPort` matches `ConnectServerPortUDP`.
- `127.0.0.1` in `ServerAddress` only works for a client running on the server machine itself.
- Each GameServer also needs a row in `Data/MapServerInfo.dat`. See [Multiple GameServers](../gameserver/multiple-gameservers.md).

### Reload

ConnectServer window menu **Reload > Server List**, or restart the ConnectServer.

---

## Notice.dat

### What it does

Holds login-screen notices: a title, a date and a message per row. The current ConnectServer and game client do not send it in-game; it is kept for launchers and tools that display news. In-game automatic notices are configured in `Data/Util/Notice.txt` (see [Util files](../data/util.md)).

### Syntax

One table, closed by `end`. Columns:

| Column | Values | Description |
|---|---|---|
| `Index` | 0 to 999 | Notice number |
| `Title` | text, quoted (up to 64 chars) | Title |
| `Date` | text, quoted (up to 32 chars) | Date shown, free format |
| `Message` | text, quoted (up to 256 chars) | Notice text |

### Example

```
0	"WARNING"	"26/07/2025"	"Beware of duplicated items when trading."
1	"EVENT"		"01/08/2025"	"Double experience all weekend!"
end
```

### Reload

Re-read by the tool that uses it. No server restart needed.
