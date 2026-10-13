# Getting started

Welcome to Morpheus Emulator. This page walks you through what you received, the first things to do in the control panel, and how this documentation is organized.

## What is in the package

| Part | What it is |
|---|---|
| **MuServer** | The server programs and their configuration files |
| **Game clients** | Windows (32 and 64 bit, with launcher), Android, iPhone and iPad, macOS, plus play in the web browser and mobile browser |
| **Control panel** | The website where you run, configure and update your server |
| **Documentation** | This site |

### Server programs

| Program | Role |
|---|---|
| ConnectServer | Shows the server list to players and sends them to the right GameServer |
| JoinServer | Validates account logins |
| DataServer | Reads and writes characters, items and accounts in the database |
| GameServer | The game world: maps, monsters, items, customs and events |
| GameServerCS | The Castle Siege server |
| RelayServer | Lets players play from the web browser |
| VoiceServer | Party voice chat |

### Main folders in MuServer

| Folder | Content |
|---|---|
| `GameServer/Data/` | The `GameServerInfo - *.dat` files: the main switches and settings (Common, Character, Custom, Event, Command, Skill, ChaosMix) |
| `Data/Custom/` | Configuration of each custom system (Morpheus Shop, Battle Pass, Quiz, Combo...) |
| `Data/Event/` | Schedules and rewards of each event |
| `Data/EventItemBag/` | Boxes, boss drops and event prizes |
| `Data/Lang/` | Server messages, one file per language |
| `Data/Item/`, `Data/Monster/`, `Data/Shop/` | Items, monsters and NPC shops |
| `ConnectServer/` | Server list and connection settings |

{% hint style="info" %}
You rarely need to open these folders by hand. The control panel has a file editor for all of them, plus visual editors for the most common tasks.
{% endhint %}

## Step 1: Sign in to the control panel

Open the control panel in your browser and sign in with the account you received. Two-factor verification is required on login.

See [Signing in](dashboard/signing-in.md).

## Step 2: Check that the services are running

On the dashboard you will see:

- CPU, memory and disk of the machine.
- How many services are online.
- The service table, with the status of ConnectServer, DataServer, JoinServer, GameServer and GameServerCS.

Every service should show as running. If one is stopped, use **Start** in its row. Start them in this order if you are starting everything by hand: DataServer, JoinServer, ConnectServer, GameServer, GameServerCS.

See the [Control panel guide](dashboard/README.md).

## Step 3: Edit your first setting

A good first change is in `GameServer/Data/GameServerInfo - Common.dat`, the file with the general server settings (experience, resets, MU Helper, online rewards, default language and more).

1. Open the **file editor** in the panel.
2. Browse to `GameServer/Data/GameServerInfo - Common.dat`.
3. Use **Ctrl+F** to find the setting you want.
4. Change the value and click **Save**.

The panel writes the file in the exact format MU expects and keeps a copy of the previous version.

See [GameServerInfo - Common](muserver/gameserver/common.md).

{% hint style="warning" %}
Change one thing at a time, and keep the structure of the file intact: do not delete section numbers or the `end` lines that close each section in `.txt` files.
{% endhint %}

## Step 4: Apply the change

Most changes need either a reload or a restart of the GameServer:

| Change | How to apply |
|---|---|
| Items, monsters, Cash Shop, server list, customs, languages, EventItemBag | **Reload** in the panel, choosing the matching module. Players stay online. |
| Settings read only at startup | **Restart** the GameServer from the panel |
| Anything the game client shows (server list screen, game texts, client items, game panels) | Publish it through the [Client Hub](dashboard/client-hub/README.md) |

The panel waits for the network port to be released before starting a service again, so a restart does not fail with "port already in use".

## Step 5: Start an event

To see the server come alive, start an event:

- From the panel, with one click, or
- In the game, with a GM command such as `/startbc` (Blood Castle), `/startds` (Devil Square) or `/startcc` (Chaos Castle).

Events also start on their own following the schedules in `Data/Event/`. See [Blood Castle](muserver/events/blood-castle.md) for a full example of how an event is configured.

## Step 6: Make the server yours

Some popular next steps:

- Set up the premium store: [Morpheus Shop](muserver/customs/morpheus-shop.md) and its [editor](dashboard/editors/morpheus-shop.md).
- Configure boxes and boss drops with the [EventItemBag editor](dashboard/editors/event-item-bag.md).
- Launch a season of the [Battle Pass](muserver/customs/battle-pass.md).
- Enable the [Morpheus Tournament](muserver/events/morpheus-tournament.md).
- Balance PvP with [Balance Class](muserver/customs/balance-class.md).
- Change the server list and game texts in the [Client Hub](dashboard/client-hub/README.md).

## How this documentation is organized

| Section | What you will find |
|---|---|
| [Feature overview](features.md) | What Morpheus Emulator offers |
| [Changelog](CHANGELOG.md) | All custom systems and events, and the release history |
| Getting started | This page |
| [Control panel](dashboard/README.md) | Signing in, services, file editor, backups, visual editors and the Client Hub |
| MuServer: GameServer | The `GameServerInfo - *.dat` files, setting by setting, starting with [Common](muserver/gameserver/common.md) |
| MuServer: Customs | One page per custom system, under `muserver/customs/` |
| MuServer: Events | One page per event, under `muserver/events/` |
| [Troubleshooting](troubleshooting.md) | Common problems and how to fix them |

{% hint style="success" %}
Most configuration pages follow the same pattern: what the system does, how to switch it on, each setting explained, and a worked example.
{% endhint %}
