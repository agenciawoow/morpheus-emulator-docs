# Server dashboard

The **Dashboard** is the first page of every server: an overview of the server, its processes and its license. It refreshes on its own — you never need to reload the browser.

The page is organised top to bottom:

1. Status bar (server, license, last update).
2. Metric cards.
3. **MuServer services** table and the GameServer stats card.
4. **Morpheus GameServer Events** (event tracker).
5. **Morpheus GameServer Console** (live logs).

## Metric cards

| Card | What it shows |
|---|---|
| **CPU** | Processor usage of the server machine (5s average). |
| **Memory** | RAM used / total. |
| **Disk** | Disk used / free. |
| **Services online** | How many MuServer programs are running out of the total (for example `5 / 6`). |
| **Players online** | Players connected right now, split by platform: **PC** (Windows / macOS), **Mobile** (Android / iOS), **Web** and **Unidentified**. |

{% hint style="info" %}
**Unidentified** players are on a client or GameServer version that doesn't report the platform yet. They are still counted in the total.
{% endhint %}

## MuServer services

**Operational control of the main processes.** Each row is one MuServer program (DataServer, JoinServer, ConnectServer, GameServer, GameServerCS…).

| Column | Meaning |
|---|---|
| **Order** | The order used by **Start all** (drag by the ⋮⋮ handle or use the ↑ ↓ arrows to change it). **Stop all** uses the reverse order. |
| **Service** | Program name. |
| **Status** | **running** (green) or **stopped**. While restarting you may see **waiting for the port to be released…** |
| **PID** | Process ID on the machine while running. |
| **Path** | The executable that runs for this service. |
| **Actions** | Start, Stop, Restart and Reload buttons (hover for the tooltip). |

Starting, stopping, restarting, the **Start all** / **Stop all** sequences and module reload are explained in detail in [Services and reload](services-and-reload.md).

## GameServer stats card

The card on the right shows **Real-time stats.** for the selected GameServer. If you have several GameServers (rooms), use the **Switch GameServer** selector at the top of the card.

| Tile | Meaning |
|---|---|
| **Online** | Players connected to this GameServer. |
| **OffStore** | Offline personal stores. |
| **OffAttack** | Offline farming bots. |
| **Bots Buffer** | Buff bots. |
| **Monsters** | Runtime spawns (monsters currently alive). |
| **CPU (proc.)** | CPU used by the GameServer process (normalised per core). |
| **Memory (proc.)** | Memory (working set) of the GameServer process. |
| **Port** | Port the GameServer listens on. |
| **GS version** | Client build the GameServer expects. |
| **Release Date** | Build date/time of the GameServer program. |

{% hint style="info" %}
Some tiles may show **—**. Open **Why did some fields come back as "—"?** in the card for the explanation — usually the GameServer is still starting or is an older build that doesn't report that value.
{% endhint %}

### Viewing and disconnecting players

1. Click **View online players**. The list refreshes every 5 seconds.
2. Use **Filter by account or name…** to find someone, or click a platform chip (PC, Mobile, Web) to show only players on that platform; click again to see all.
3. The list shows **Account**, **Character**, **Platform** and an **Action** column.
4. Click **Disconnect** on a row and confirm **Disconnect player?** The session is closed immediately.

{% hint style="info" %}
When the list opens, the GameServer window on the machine briefly flashes a dialog. This is normal.
{% endhint %}

### GameServer actions

Click the **GameServer actions** button (the gear on the card) to open the same commands as the File menu of the GameServer window.

**Sessions**

| Button | Effect |
|---|---|
| **All User LogOut** | Ends the session of every logged-in player gracefully. They can reconnect right away. Confirm with **LogOut All**. |
| **All User Disconnect** | Force-disconnects everyone and blocks the GameServer for new logins. Use only for an emergency stop. Confirm with **Disconnect All**. |

**Server shutdown**

| Button | Effect |
|---|---|
| **1 min** / **3 min** / **5 min** | Broadcasts a countdown to all players, then closes the GameServer. |

{% hint style="warning" %}
Shutdown countdowns **cannot be cancelled**. If a countdown is already running, further clicks are ignored. After confirming you see **Action sent — The GameServer runs the action on the next cycle (up to 2s).**
{% endhint %}

## Morpheus GameServer Events

**Real-time event status.** The event tracker lists every scheduled event of the selected GameServer room (use the room selector, shown as `Sala | <name>`, if you have more than one).

| Column | Meaning |
|---|---|
| **Event** | Event name (Blood Castle, Devil Square, invasions, custom events…). |
| **Next run** | Countdown to the next start. |
| **Type** | Event category. |
| **Status** | **Active** (running), **Soon** (counting down) or **Inactive** (disabled). |
| **Action** | **Start** button. |

Use the filter chips to narrow the list: **Now / Soon**, **Next 30 min**, **Active**, **Disabled**, **All**. Click **Show less** to collapse a long list.

### Starting an event now

1. Find the event and click **Start**.
2. Confirm **Start "<event>"?**. The GameServer scheduler fires the event in about 5 seconds.
3. You will see **Event scheduled — "<event>" starts in ~5 seconds.** Follow the status in the tracker.

{% hint style="info" %}
Starting from the panel forces the scheduler's next cycle. It does not replace the in-game GM command, and it uses the event's normal configuration (rewards, maps, durations).
{% endhint %}

## Morpheus GameServer Console

**Real-time logs.** The console shows the GameServer log in tabs:

| Tab | Content |
|---|---|
| **System Log** | Main GameServer output (startup, loading, errors). |
| **Global Message** | Global notices and announcements. |
| **Command Log** | Commands typed by players and GMs. |
| **Hack Log** | Anti-hack detections. |
| **Connect Log** | Connections and disconnections. |

Toolbar options:

- **Show timestamp** – show or hide the time of each line.
- **Auto-scroll** – keep the view at the newest line.
- **Search the log…** – filter lines containing a word.
- **Clear** – clear the view (it doesn't delete the log file).
- **Reload** – reload the current tab.

If a tab says **No entries today — the GameServer has not written a single line to this log today.**, the log file for today is simply empty. For older days and other programs use [Server logs](server-logs.md).

## When the server is offline

If the connection status in the sidebar shows **offline**, the cards report that the server is offline and live data, service control and file editing are unavailable. Check that the server machine is powered on and connected to the internet. The panel reconnects automatically as soon as the machine is back.
