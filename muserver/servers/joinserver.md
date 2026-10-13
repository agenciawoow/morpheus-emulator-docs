# JoinServer

## What it does

The JoinServer checks account logins (account and password against the `MEMB_INFO` table) and keeps track of which accounts are online, so the same account cannot be in two GameServers at once. GameServers connect to it; players never do.

It also reports to the ConnectServer every second. While the JoinServer is down or not reporting, the ConnectServer sends an **empty server list**.

## Where it lives

| File | Purpose |
|---|---|
| `JoinServer/Data/JoinServer.ini` | Database connection, ports, login options |
| `JoinServer/Data/AllowableIpList.txt` | Which machines may connect (your GameServers) |

---

## JoinServer.ini

INI format, sections `[JoinServerInfo]` and `[GoogleAuth]`.

### [JoinServerInfo]

| Key | Default | Values | Description |
|---|---|---|---|
| `JoinServerPort` | 55970 | 1 to 65535 | TCP port GameServers connect to. Must equal `JoinServerPort` in every `GameServerInfo - Common.dat`. |
| `ConnectServerAddress` | 127.0.0.1 | IP | Address of the ConnectServer, for the "alive" report. |
| `ConnectServerPort` | 55557 | 1 to 65535 | The ConnectServer **UDP** port (`ConnectServerPortUDP` in `ConnectServer.ini`). |
| `CaseSensitive` | 0 | 0, 1 | `0` = account names are not case sensitive (`Player1` and `player1` are the same account). `1` = they are different accounts. |
| `MD5Encryption` | 0 | 0, 1 | `0` = passwords are stored as plain text in the database. `1` = passwords are stored as MD5 hashes. Must match how your website registers accounts. |
| `GlobalPassword` | built-in value | text, up to 10 characters | A support password that logs into **any** account. Every use is logged in red in the JoinServer window. |
| `JoinServerODBC` | (empty) | text | Name of the ODBC data source used for accounts. Set up at installation. |

The file also holds the database login. It is created at installation and you should not need to change it.

{% hint style="danger" %}
**Always set `GlobalPassword` yourself.** If the key is missing, the JoinServer uses a built-in value. Leave it present but empty (`GlobalPassword =`) to disable it, or use a long random value (10 characters maximum) known only to your staff. Change it if a staff member leaves.
{% endhint %}

### [GoogleAuth]

Enables "Sign in with Google" in clients that support it.

| Key | Default | Values | Description |
|---|---|---|---|
| `Enabled` | 0 | 0, 1 | `1` = accept Google sign-in. |
| `ClientId` | (empty) | text | The OAuth Client ID of your project in the Google Cloud Console. Required when `Enabled = 1`. |

The account tables must have the Google link column created by the database update scripts shipped with the server.

### Example

```ini
[JoinServerInfo]
JoinServerODBC = MuOnline
JoinServerPort = 55970
ConnectServerAddress = 127.0.0.1
ConnectServerPort = 55557
CaseSensitive = 0
MD5Encryption = 0
GlobalPassword =

[GoogleAuth]
Enabled = 0
ClientId =
```

### Rules and pitfalls

- **Empty server list for everyone:** the JoinServer is closed, failed to connect to the database, or `ConnectServerAddress`/`ConnectServerPort` do not point to the ConnectServer's UDP port.
- **Changing `MD5Encryption` on a live server** breaks every existing password: passwords stored in the other format no longer match. Change it only on a fresh database, or convert all passwords at the same time.
- **Changing `CaseSensitive` on a live server** can make two existing accounts that differ only by case collide. Decide it before opening.

### Reload

Restart the JoinServer. Players already in game are not disconnected, but logins fail while it is down.

---

## AllowableIpList.txt

Same format and rules as the DataServer's: section `0`, one quoted IP per line, `end`. Only listed machines can connect to the JoinServer.

```
0
"127.0.0.1"
end
```

Add the IP of every machine that runs a GameServer. See [DataServer](dataserver.md#allowableiplisttxt) for the details.

### Reload

Restart the JoinServer.
