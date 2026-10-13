# Services and reload

The **MuServer services** table on the [Dashboard](server-dashboard.md) lets you control every MuServer program without logging in to the server machine: start, stop, restart, start/stop everything in the right order, and **hot reload** — make a program re-read its files *without* closing it and without disconnecting anyone.

{% hint style="info" %}
{% endhint %}

## Per-service buttons

Each row has icon buttons in the **Actions** column. Hover over a button to see its tooltip.

| Button | Tooltip | What happens |
|---|---|---|
| ▶ | **Start** | Opens the program. Status turns **running** and a PID appears. |
| ■ | **Stop** | Asks the program to close gracefully; if it doesn't respond, it is force-closed. |
| ↻ | **Restart (may take up to 60s to release the port)** | Stop, short pause, start. The panel waits until the network port is free before starting again, so you don't get "port already in use" errors. While it waits you see **waiting for the port to be released…** |
| ⟳ | **Reload (pick which module to reload — without taking the .exe down)** | Opens the **Module reload** window. See [Hot reload](#hot-reload). |

{% hint style="warning" %}
Stopping or restarting a **GameServer** disconnects the players on it. For planned maintenance, prefer **Stop all** with a warning to players, or the shutdown countdown in the [GameServer actions](server-dashboard.md#gameserver-actions).
{% endhint %}

A row marked **not registered** means the panel doesn't know which program to run for that service yet. Contact support to have it registered.

## Start all / Stop all

Above the table you find **Start all** and **Stop all**.

### Start all

Confirm **Start all services?**. Services open **one at a time**, in the order of the **Order** column — by default DataServer, JoinServer, ConnectServer, Relay/Voice and then the GameServers. The next one only opens when the previous one has finished loading and is listening on its port. If one fails, the sequence stops.

Progress is shown live, for example `in progress — 2 of 6`, and each row shows its state:

| State | Meaning |
|---|---|
| **queued** | Waiting for its turn. |
| **opening…** | Being started. |
| **waiting to be ready (Ns)** | Started; waiting for its port to open. |
| **ready** | Up and listening. |
| **skipped** | It **was already running**. |
| **failed** | It **was not ready within the time limit** or **the process closed right after opening**. |
| **not run** | Not started because an earlier service failed. |

Final results: **done**, **finished with problems — see the rows in red**, **stopped: a service did not come up, the following ones were not started**, or **canceled**. You can stop a running sequence with **Cancel sequence**.

### Stop all

Confirm **Stop all services?** and choose how to warn players:

- **Disconnect and save now (recommended)** – GameServers save and disconnect every player, then close.
- **Warn players and close in N minutes** – players receive a countdown notice first (`players warned: closing in 120s (35 online)`).

GameServers close first (**saving characters… (N online)**), then the other services in **reverse order**.

{% hint style="danger" %}
If a service shows **force-killed** (**did not close on its own and was force-killed — check that nothing was left unsaved.**), check your database and logs before starting again.
{% endhint %}

### Changing the start order

Drag a row by its **⋮⋮** handle (or use the ↑ ↓ arrows) in the **Order** column. The order is saved immediately (**Saving order…**). "Start all" uses this order; "Stop all" goes in reverse.

## Hot reload

Hot reload tells a running program to re-read part of its configuration **without restarting the process**. Nobody is disconnected.

### How to reload

1. Save your changes (in the [File editor](file-editor.md), [Configuration](configuration-forms.md) or a [visual editor](editors/README.md)).
2. On the Dashboard, click the **Reload** button on the GameServer (or ConnectServer) row.
3. In **Module reload — Reloads without killing the process**, click the module that matches the files you changed.
4. Check the GameServer **System Log** in the [console](server-dashboard.md#morpheus-gameserver-console) for a line like `Shop loaded successfully`.

The Reload button is disabled when the service is stopped (**Service offline or without the reload capability**) or when the program doesn't support reload (**Reload is not supported by this service**).

{% hint style="info" %}
If you have more than one GameServer (for example a normal server and a Castle Siege server), reload **each one** that uses the files you changed.
{% endhint %}

### GameServer modules

| Module | Reloads | Use it after changing… |
|---|---|---|
| **Reload CashShop** | Cash shop settings and the cash shop catalog (packages and products). | Cash Shop (X) prices, packages, coins. |
| **Reload ChaosMix** | Chaos Machine mix settings (`GameServerInfo - ChaosMix.dat`). | Mix success rates and costs. |
| **Reload Character** | Character settings (`GameServerInfo - Character.dat`) and default class info. | Class stats, damage/defense formulas, starting values. |
| **Reload Command** | Commands list and command settings (`Command.txt`, `GameServerInfo - Command.dat`). | Command costs, levels, enabled commands. |
| **Reload Common** | General settings (`GameServerInfo - Common.dat`), effects, map data, languages, mini-map and mining. | Experience/drop rates, general rules, Offline Helper settings. |
| **Reload Custom** | All custom systems (`GameServerInfo - Custom.dat` and the `Custom` folder): arena, event drop, online lottery, custom quests, ranking, wings, jewels, Morpheus Shop, Battle Pass, NPC commands, start items, custom mixes and more. | Any custom system, Morpheus Shop prices, Battle Pass. |
| **Reload Event** | Event settings (`GameServerInfo - Event.dat`) and every event file (Blood Castle, Chaos Castle, Devil Square, invasions, Crywolf, Kanturu, Raklion, tournaments…). | Event schedules, rewards, maps. |
| **Reload EventItemBag** | The bag manager and every bag file. | Box/boss drops (see [EventItemBag editor](editors/event-item-bag.md)). |
| **Reload Hack** | Anti-hack settings and packet/skill checks. | Anti-hack rules. |
| **Reload Item** | The item tables (`Item` folder: items, drops, options, sets, sockets, values…). | Item attributes and drop tables. |
| **Reload Lang** | Server messages in every language (`LangConfig.txt` and the `Lang` folder). | In-game server messages and translations. |
| **Reload Map** | Map manager and map/server routing (`MapManager.txt`, `MapServerInfo.dat`). | Map rules, which server hosts each map. |
| **Reload Monster** | Monster list, monster AI/skills and monster spots (`Monster` folder and spot files). Existing monsters are removed and re-spawned. | Monster stats, spots (see [Spot editor](editors/spot.md)). |
| **Reload Move** | Gates and the `/move` list. | Move costs/levels, gates. |
| **Reload Quest** | Quests and world quests. | Quest objectives and rewards. |
| **Reload Shop** | NPC shops (`ShopManager.txt` and the shop files), item values and PC Points. | NPC shop contents (see [NPC Shop editor](editors/npc-shop.md)). |
| **Reload Skill** | Skill settings, skill list, master skill tree and skill damage. | Skill damage and requirements. |
| **Reload Terrain** | Map terrain files (walls, safe zones). | Terrain/attribute files. |
| **Reload Util** | Experience table, chat filters, rename filter, GM list, automatic notices and reset tables. | Notices, GM list, reset/master reset tables. |
| **Reload Bots** | Buff bot settings. | Buff bots. |
| **Reload All** | Every module above **except Reload Item and Reload Terrain** (map and language data are included through Common). | Several areas at once. |

{% hint style="warning" %}
**Reload Terrain while an event is running:** the panel asks **Reload terrain? — If an event is running, reloading the terrain reverts that event's doors and walls.** Prefer to reload terrain when no event is in progress.
{% endhint %}

{% hint style="warning" %}
**Reload All** does **not** re-read the item tables. After changing files in the `Item` folder, use **Reload Item** — or restart the GameServer for large `Item.txt` changes. Remember that item changes usually also need a client update (see [Client Hub](client-hub/README.md)).
{% endhint %}

### ConnectServer modules

| Module | Reloads | Use it after changing… |
|---|---|---|
| **Reload ServerList** | The ConnectServer server list (`ServerList.dat`). | Server names/IPs/ports shown at the server selection. |
| **Reload Server Launcher** | The launcher/client version setting (`ConnectServerVersion` in `ConnectServer.ini`) and the list of update files. | The required client version. |

## What reload cannot do

- **Client-side files** (anything players have on their PC/phone: item names, interface, `ServerList.bmd`, client settings) are not affected by a reload. Publish them with the [Client Hub](client-hub/publishing-updates.md).
- **Ports, database connections and startup-only settings** are read only when the program starts. Use **Restart**.
- If a reload fails because a file is invalid, the GameServer usually keeps the previous values and writes a red line in the **System Log**. Fix the file and reload again — see [Validation and conflicts](validation-and-conflicts.md).

## Reload from the visual editors

The visual editors have a bar with **Save** followed by the reload (and, when needed, publish) step, so you don't have to come back to the dashboard. See [The Save → Reload → Publish bar](editors/README.md#the-save-reload-and-publish-bar).
