# RelayServer and VoiceServer

Two optional programs. You only need them if you offer **browser play** (RelayServer) or **party voice chat** (VoiceServer).

---

## RelayServer

### What it does

{% embed url="https://www.youtube.com/watch?v=Z2CNeffaRm4&t=13s" %}
This is what the RelayServer enables: the full game running inside a browser tab.
{% endembed %}

Web browsers cannot open the raw TCP connections the game uses. The browser client connects to the RelayServer over **WebSocket**, and the RelayServer forwards the bytes unchanged to the ConnectServer or a GameServer. It has no game logic: it is a pipe.

You need one mapping for the ConnectServer and one for each GameServer that browser players can enter.

### Where it lives

`RelayServer/Data/RelayServer.ini`

### Syntax and keys

INI format, section `[Relay]`.

| Key | Default | Values | Description |
|---|---|---|---|
| `TlsCert` | (empty) | text | Subject (name) of the TLS certificate installed in the Windows certificate store **Local Machine > Personal (My)**. When set, the relay serves secure `wss://`. When empty, it serves plain `ws://`. |
| `Map1` to `Map32` | (empty) | `wsPort:targetHost:targetPort` | One route each. `wsPort` = port the browser opens; `targetHost` = IP of the ConnectServer or GameServer; `targetPort` = that server's TCP port. |

`Map` numbers do not have to be consecutive: every key from `Map1` to `Map32` is checked, and missing ones are skipped.

### Example

```ini
[Relay]
TlsCert = <subject of your certificate>

; Browser -> wss 44490 -> ConnectServer
Map1 = 44490:127.0.0.1:44405

; Browser -> wss 44491 -> GameServer 0
Map2 = 44491:127.0.0.1:55901

; Browser -> wss 44492 -> GameServer 1
Map3 = 44492:127.0.0.1:55902
```

### Rules and pitfalls

{% hint style="danger" %}
If the web client page is served over **https** (it always is in production), the browser only accepts **wss://**. Without a valid `TlsCert`, or with a subject that does not exactly match an installed certificate, **no browser player can connect**, on desktop or mobile.
{% endhint %}

- At startup the RelayServer window must show a `[TLS] wss ON - cert subject '...'` line **before** the `listening` lines. If it does not, the certificate was not found.
- The certificate must be in **Local Machine**, not Current User, and must be valid for the address browsers use.
- A malformed mapping is logged in red (`bad mapping MapN=... (expected wsPort:host:port)`) and ignored. With no valid mapping the relay does not start.
- Open every `wsPort` in the firewall.
- `ConnectServer.ini` `MaxIpConnection` counts all browser players as **one IP** (the relay's). Keep that limit high.
- The web client's own settings must point to these WebSocket ports. See [Client](../client/README.md).

### Reload

Restart the RelayServer. Browser players connected through it are disconnected.

---

## VoiceServer

### What it does

Runs the voice rooms for **party voice chat**. When a player joins a party, the GameServer gives the client a signed ticket and the address of the VoiceServer; the client connects and talks with the other party members. Each party is one room.

Party voice needs three things to match:

| Where | Key | Must equal |
|---|---|---|
| `GameServer/Data/GameServerInfo - Common.dat` | `PartyVoiceEnabled = 1` | |
| `GameServer/Data/GameServerInfo - Common.dat` | `PartyVoiceTokenSecret` | `TokenSecret` in `VoiceServer.ini` |
| `GameServer/Data/GameServerInfo - Common.dat` | `PartyVoiceWsUrl` (port part) | `VoiceServerPort` in `VoiceServer.ini` |

See [GameServerInfo - Common](../gameserver/common.md#party-voice).

### Where it lives

`VoiceServer/Data/VoiceServer.ini`

### Syntax and keys

INI format, section `[VoiceServerInfo]`. The shipped file contains only the first four keys; the others use their defaults until you add them.

| Key | Default | Values | Description |
|---|---|---|---|
| `VoiceServerPort` | 55600 | 1 to 65535 | WebSocket port the clients connect to. |
| `TokenSecret` | (empty) | text, 32+ random characters | Secret used to verify the GameServer's tickets. Must be **identical** to `PartyVoiceTokenSecret`. |
| `MaxRooms` | 1000 | 1 to 100000 | Maximum simultaneous voice rooms (parties). |
| `StunServer` | (empty) | `stun:<host>:<port>` | STUN server used to find the public address of each side. Empty = no STUN (works on a LAN or when the server has a public IP directly on its network card). |
| `MaxMembersPerRoom` | 10 | 1 to 100 | Maximum people in one voice room. |
| `MaxPeersTotal` | 200 | 0 to 100000 | Maximum people in voice on the whole server. `0` = no limit. New joins over the limit are refused and logged as `server_full`. |
| `IdleTimeoutSec` | 60 | 0 to 86400 | Seconds before an idle connection is closed. `0` = never. |
| `OpusBitrate` | 0 | 0 to 512000 | Audio bitrate in bits per second. `0` = let the client and server agree on it (recommended). |
| `OpusDtx` | 0 | 0, 1 | `1` = stop sending audio during silence (saves bandwidth, may clip the first syllable). |
| `TokenSkewSec` | 300 | 0 to 3600 | How many seconds of clock difference between the GameServer and VoiceServer machines are tolerated when checking tickets. `0` = no tolerance. |

### Example

```ini
[VoiceServerInfo]
VoiceServerPort = 55600
TokenSecret = <a long random secret, same as PartyVoiceTokenSecret>
MaxRooms = 1000
MaxMembersPerRoom = 10
MaxPeersTotal = 200
IdleTimeoutSec = 60
```

And in `GameServerInfo - Common.dat`:

```ini
PartyVoiceEnabled = 1
PartyVoiceTokenSecret = <the same secret>
PartyVoiceWsUrl = ws://192.0.2.10:55600
```

### Rules and pitfalls

- **Generate your own secret.** Never keep an example value and never publish it: anyone with the secret can create voice tickets.
- If the two secrets differ, the GameServer hands out tickets the VoiceServer rejects, and nobody hears anybody.
- `PartyVoiceWsUrl` must use the address **players** can reach (public IP), not `127.0.0.1`.
- Voice audio travels over UDP between the client and the VoiceServer. Allow the VoiceServer program through the Windows firewall, not only its WebSocket port.
- Browsers on an https page refuse plain `ws://` connections. Browser players may need a secure (`wss://`) voice address.
- Keep the clocks of all server machines synchronized (Windows time service). A large clock difference makes tickets look expired.

### Reload

Restart the VoiceServer. Active voice rooms are dropped while it restarts. The GameServer side (`PartyVoice*` keys) is applied with `/reload common`.
