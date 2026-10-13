# MHPServer

## What it does

The MHPServer is the server side of the client anti-hack. The game client keeps a connection to it while playing. The MHPServer:

- checks that the client is the official, unmodified version for your server;
- receives detection reports from the client (forbidden programs, memory changes, macros, tampered files) and disconnects the offender;
- keeps a blacklist of IP addresses and devices that are never allowed to play.

The detection rules themselves are delivered with the package. As a server owner you normally only manage the **blacklist** and a few operational settings.

## Where it lives

| File | Purpose |
|---|---|
| `MHPServer/MHPServer.ini` | Port, connection limit, lock time and the client identification |
| `MHPServer/BlackList.txt` | Banned IP addresses and hardware IDs |
| `MHPServer/Data/*.db` | Detection databases (checksums, process and window lists). Shipped with the package, do not edit. |

## MHPServer.ini

INI format, section `[MHPServerInfo]`. Only the keys below are meant to be changed by server owners. Leave every other key (memory guard, macro protection, addresses, encryption values) exactly as shipped unless support tells you otherwise.

| Key | Values | Description |
|---|---|---|
| `MHPServerPort` | 1 to 65535 | TCP port the client's anti-hack connects to. Must match the client configuration and be open in the firewall. |
| `MaxIpConnection` | number | Maximum anti-hack connections from the same IP. Players behind the same router or mobile carrier share an IP, so keep it at or above the GameServer's `MaxIpConnection`. |
| `DetectionLockTime` | seconds | How long a player who was caught is blocked from reconnecting. `300` = 5 minutes. |
| `ClientVersion` | `X.XX.XX` | Client version. Must equal `ServerVersion` in `GameServerInfo - Common.dat` and the version built into your client. |
| `ClientSerial` | text | Client serial. Must equal `ServerSerial` in `GameServerInfo - Common.dat` and the serial built into your client. |

{% hint style="warning" %}
When you release a new client with a new version or serial, update `ClientVersion` / `ClientSerial` here **and** `ServerVersion` / `ServerSerial` in every GameServer at the same time. A mismatch disconnects every player.
{% endhint %}

### Example

```ini
[MHPServerInfo]
ClientVersion = 1.04.05
ClientSerial = <your client serial>
MHPServerPort = 55999
MaxIpConnection = 10
DetectionLockTime = 300
```

## BlackList.txt

### What it does

Players matching an entry are refused by the anti-hack and cannot play.

### Syntax

Two sections, each closed with `end`:

| Section | Content | Format |
|---|---|---|
| `0` | IP addresses | `"192.0.2.55"` |
| `1` | Hardware IDs (device) | `"XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX"` |

### Example

```
0
"192.0.2.55"
"192.0.2.77"
end

1
"0A1B2C3D-4E5F6071-8293A4B5-C6D7E8F9"
end
```

### Rules and pitfalls

- Prefer banning by **hardware ID**. IP bans hit everyone on the same router, school network or mobile carrier, and are easy to bypass.
- Keep an empty section with just `end` if you have no entries of that type. Do not delete the section number.
- Account and character bans are done in game with the GM commands (`/banacc`, `/banchar`) and the database, not here.

## Reload

Restart the MHPServer after changing `MHPServer.ini` or `BlackList.txt`. Players connected at that moment may be disconnected and have to log in again.
