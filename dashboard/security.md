# Security

**Security** shows what the GameServer itself flagged while players were online: kicks for bad packets, packet floods, duplicate item serials, trades it refused, client integrity failures, plus every login/logout and every completed trade. The data comes from the GameServer's own evidence trail (`LOGS\SECURITY_LOG`), shipped to the panel by the agent.

{% hint style="info" %}
Nothing on this page punishes anyone by itself. It is the evidence; the decision is yours.
{% endhint %}

## Turning it on

1. In **Configuration → GameServer · Common settings**, set `SecurityLog = 1` and save. The GameServer picks it up on `/reload` or on the next restart.
2. Keep the agent online. It tails `GameServer\LOGS\SECURITY_LOG\YYYY-MM-DD.txt` every few seconds and sends new lines to the panel. A line is only marked as sent after the panel confirms it, so an outage never loses events (a resend of the same line is ignored).

The first events appear within about ten seconds. The header shows **Last event received** so you can tell the pipeline is alive.

## What you see

### Summary tiles

Counts for the chosen window (1 h to 7 days): events by severity, how many distinct accounts had a medium or high event, and how many logins happened.

| Severity | Meaning |
|---|---|
| **High** | The GameServer kicked the player or confirmed tampering: packet flood over the limit, wrong packet serial, duplicate item serial, speed/latency mismatch, client integrity failure. |
| **Medium** | Something was blocked without a kick: a packet dropped by the flood limiter, a trade cancelled because the zen was no longer there, a skill-speed penalty. |
| **Low** | Minor signals. |
| **Info** | Logins, logouts, completed trades and telemetry. Hidden by default in the feed. |

### Events tab

The chronological feed. Filter by type, minimum severity or account; informational rows are hidden unless you untick **Hide informational**. Each row shows where the player was (map and coordinates), the detail the server wrote, and the IP / HWID of the session. Click an account to open its card.

### Suspects tab

Accounts ranked by a score over the last 1–30 days: 10 points per high event, 3 per medium, 1 per low. The **main reason** column tells you which detection fired most. This is the list to look at every morning.

### Player tab

The card of one account:

* **Connections** – every IP / HWID / platform combination the account logged in from, with first and last time.
* **Other accounts sharing the same HWID or IP** – the fastest way to find alt accounts and shared devices.
* **Trades** – partner, zen each way and the items that changed hands.
* **Events** – everything the server recorded for that account.

Administrators also get **Disconnect from game** and **Block account** (1 to 365 days, with a reason). The block goes through the agent to the GameServer, which asks the DataServer to set `MEMB_INFO.bloc_code` and disconnects the player if online; it shows up in the **Banned** tab with the reason and who did it. Unblocking is still done in `MEMB_INFO`.

### Calibration tab

Shadow-mode checks (`GuardAttackRate`, `GuardPositionPacket` set to 1) only record; this tab shows what real players do: the peak number of attacks per window in each session, by class, and how many position packets arrived without a skill packet. Raise a check to 2 (drop) or 3 (disconnect) only when the threshold catches nobody honest here.

## Event types

| Type | Comes from |
|---|---|
| `session` | Login / logout, with the session length. |
| `trade` | Completed trade (both sides, zen and item serials). |
| `trade-money` | Trade cancelled at OK because a player no longer had the offered zen. |
| `packet-check` | Unknown packet, wrong encryption lane or wrong packet serial → kick. |
| `packet-flood` | Packet rate over the limit in `HackPacketCheck.txt` (drop or kick). |
| `skill-speed` | Skill cast faster than `HackSkillCheck.txt` allows. |
| `dup-serial` | The same item serial twice in the inventory or warehouse. |
| `speed-report` / `latency` | Heartbeat speed/clock checks (`CheckSpeedHack`, `CheckLatencyHack`). |
| `integrity` / `integrity-telemetry` | Client anti-tamper beacon: tampering confirmed, delayed kick, or telemetry-only bits. |
| `checksum` / `license-bind` / `client-breach` | Client build checks. |
| `attack-rate` | More physical attacks per window than the attack speed allows (`GuardAttackRate`). |
| `position-unpaired` / `position-far` | Position packet (0x15) with no skill packet in the window, or farther than the range (`GuardPositionPacket`). |
| `packet-flood-global` | More packets per second than `GuardPacketFloodMax`, any opcode (`GuardPacketFlood`). |
| `client-module` / `client-process` / `client-hook` / `client-thread` / `client-rwx` / `client-scan` | Windows client injection scanner (`GuardClientScan`): DLL outside the allowlist, external process with write access to the game, inline hook on send/recv/timers, thread starting outside any module, private executable memory summary, scan summary. |

Retention is 90 days.
