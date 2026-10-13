# Changelog

This page lists the main custom systems and features of Morpheus Emulator, followed by the release history. For a longer description of what each area does, see the [Feature overview](features.md).

{% hint style="info" %}
Morpheus Emulator is developed by Hi Networks. Each entry links to the page that explains how to configure it.
{% endhint %}

## Highlights

| | |
|---|---|
| **Platforms** | Windows (32 and 64 bit), Android, iPhone and iPad, macOS, web browser and mobile browser, all on the same server, account and character |
| **Events** | 32 in total: 14 MU classics and 18 built for Morpheus |
| **Commands** | 88 registered commands for players, shops, events and moderation, each with its own permission and requirements |
| **In-game languages** | 7: English, Portuguese, Spanish, Chinese, Korean, Vietnamese and Thai, switchable inside the game |
| **Custom systems** | More than 40 modules that do not exist in the original MU |
| **Classes** | 15 classes, with all 225 matchups simulated by the balance tool |
| **Server programs** | Connect, Data, Join, Game, GameServerCS, Relay (browser play) and Voice (party voice chat) |
| **Control panel** | Run the whole server from a web browser, in 5 languages |

## Custom systems

Most custom systems are switched on in `GameServer/Data/GameServerInfo - Custom.dat` and configured in a file under `Data/Custom/`.

### Flagships

| System | What it does | Docs |
|---|---|---|
| Morpheus Shop | Premium in-game store where the player builds the item (level, luck, skill, option, excellent, socket, harmony) before buying, within the limits you set | [Morpheus Shop](muserver/customs/morpheus-shop.md) |
| Battle Pass | Seasonal pass with free and premium reward tracks, missions, a 28-day calendar and paid plans | [Battle Pass](muserver/customs/battle-pass.md) |
| PvP balance (Balance Class) | A single PvP damage formula for every class, per-stat weights and per-item bonus rules, plus the balance analysis tool | [Balance Class](muserver/customs/balance-class.md) |
| Morpheus Tournament | One-on-one Colosseum tournament run end to end by the server, with bracket, ranking, history and optional betting | [Morpheus Tournament](muserver/events/morpheus-tournament.md) |
| Offline MU Helper | The player closes the game and the character keeps hunting | [Server features](muserver/customs/server-features.md) |
| Jewel Bank | Deposit and withdraw jewels, with duplication protection validated on the server | [Feature overview](features.md#jewel-bank) |
| Party voice | Voice chat inside the game on PC, Android and browser, with no external program | [Server features](muserver/customs/server-features.md) |
| Discord Rich Presence | The player's character and your server name show on their Discord profile | [Feature overview](features.md#discord-rich-presence) |

### Gameplay and economy customs

| System | What it does | Docs |
|---|---|---|
| Custom Arena | Arenas you design yourself, with kill and death scoring | [Custom Arena](muserver/customs/custom-arena.md) |
| Online Lottery | Prize drawn among the players who are connected | [Online Lottery](muserver/customs/online-lottery.md) |
| Quiz | Questions and answers you register, with rewards for the fastest correct answer | [Quiz](muserver/customs/quiz.md) |
| Combo | Skill combos for every class, not only the Blade Knight, with the combo sequences you define | [Combo](muserver/customs/combo.md) |
| Custom Wing | New wings with their own options, plus the wing mix | [Custom Wing](muserver/customs/custom-wing.md) |
| Custom Jewel | New jewels with their own effects and success rates | [Custom Jewel](muserver/customs/custom-jewel.md) |
| Custom Mix | Chaos Machine recipes of your own, including the mix expansion | [Custom Mix](muserver/customs/custom-mix.md) |
| Custom Monster | Per-monster overrides: life, damage, defense, experience, kill messages, coin rewards and summons | [Custom Monster](muserver/customs/custom-monster.md) |
| Buy VIP | Players buy VIP tiers with in-game currency | [Buy VIP](muserver/customs/buy-vip.md) |
| Exchange Coin | Exchange between the server's currencies | [Exchange Coin](muserver/customs/exchange-coin.md) |
| Start Item | Starting items delivered per class to new characters | [Start Item](muserver/customs/start-item.md) |
| Pick | `/pick` command that collects chosen items automatically, with requirements per account level | [Pick](muserver/customs/pick.md) |
| Rankings and Top | Custom rankings (F8 window), player Top lists and rank rewards by reset, master reset or level | [Rankings](muserver/customs/ranking.md) |
| Death Message | Personalized death messages | [Death Message](muserver/customs/death-message.md) |
| Event Drop | Item rain event for a limited time | [Event Drop](muserver/customs/event-drop.md) |
| Custom Quest | Quests created by the GM, including NPC quests with objectives and rewards | [Custom Quest](muserver/customs/custom-quest.md) |
| NPC customs | Collector NPCs, command NPCs and NPCs that move | [NPC customs](muserver/customs/npc-customs.md) |
| Custom Move | `/move` destinations of your own, with level, reset and VIP requirements | [Custom Move](muserver/customs/custom-move.md) |
| Bot Buffer | Buffer bots that buff players, optionally reserved for VIP accounts | [Bot Buffer](muserver/customs/bot-buffer.md) |
| Command descriptions | Custom description text for every command | [Server features](muserver/customs/server-features.md) |

### Server features

These are switched on in the GameServer configuration and documented together on the [Server features](muserver/customs/server-features.md) page.

| Feature | What it does |
|---|---|
| Offline store | The personal shop keeps selling while the player is disconnected |
| Offline attack | The character keeps attacking while the player is disconnected, with auto potion, auto buff and auto resume per account level |
| Offline MU Helper | Offline hunting with its own experience rate, duration and minimum level |
| Guild warehouse | Shared warehouse for the guild, restricted by member rank |
| Inventory extension | Players buy extra inventory slots |
| Chat anti-spam | Blocks players who flood the chat for a configurable time |
| Party voice | In-game voice chat for the party |
| Online rewards | Coins paid periodically to connected players, per account level |
| Chaos Mix announcement | Global notice when a player succeeds or fails a high-level mix, translated per player language |
| Shop confirmation box | Asks the player to confirm coin purchases (or all purchases) |
| Event timetable | In-game window (H key) with the time of the next events |

### Minigames and extras

| Feature | What it does |
|---|---|
| Mining | Five mining stages with jewel packs as rewards |
| MU Rummy | Card game inside MU |
| Event roulette | Luck-based event rewards |
| Automatic assistants | Hunting helper with auto potion and return to spot, automatic alchemist, buffer and warper, automatic reset and PK clearing |

## Events

Every event can run on a schedule, by GM command, or from one click in the control panel. Switches live in `GameServer/Data/GameServerInfo - Event.dat` and schedules in `Data/Event/`.

### MU classics (14)

| Event | Description | Docs |
|---|---|---|
| Blood Castle | Race against the clock to recover the archangel weapon | [Blood Castle](muserver/events/blood-castle.md) |
| Devil Square | Round-based arena: kill monster waves for points | [Devil Square](muserver/events/devil-square.md) |
| Chaos Castle | Everyone against everyone in a shrinking arena | [Chaos Castle](muserver/events/chaos-castle.md) |
| Illusion Temple | Team contest inside the temple | [Illusion Temple](muserver/events/illusion-temple.md) |
| Kanturu | Three phases: Maya battle, Nightmare and the Tower | [Kanturu](muserver/events/kanturu.md) |
| Crywolf | Altar defense against the invasion | [Crywolf](muserver/events/crywolf.md) |
| Castle Siege | Guilds fight for control of the castle | [Castle Siege](muserver/events/castle-siege.md) |
| Raklion | Group boss fight against Selupan | [Raklion](muserver/events/raklion.md) |
| Imperial Guardian | Timed boss encounter | [Imperial Guardian](muserver/events/imperial-guardian.md) |
| Doppelganger | The clone chamber (Double Goer) | [Double Goer](muserver/events/doppelganger.md) |
| Castle Deep | The castle's deep dungeon | [Castle Deep](muserver/events/castle-deep.md) |
| Invasions | Monster invasions per map, announced on screen | [Invasions](muserver/events/invasions.md) |
| Battle Ground | Guild battlefield | - |
| Arca Battle | Arca war | - |

### Built for Morpheus (18)

| Event | Description | Docs |
|---|---|---|
| Colosseum | One-on-one tournament run by the server | [Morpheus Tournament](muserver/events/morpheus-tournament.md) |
| GvG | Guild against guild | [GvG](muserver/events/gvg-event.md) |
| TvT | Team against team | [TvT](muserver/events/tvt-event.md) |
| PvP | Open one-on-one duel by registration | [PvP](muserver/events/pvp-1x1.md) |
| Kill All | Free-for-all elimination with kill scoring | [Kill All](muserver/events/kill-all.md) |
| Hide and Seek | Search minigame | [Hide and Seek](muserver/events/hide-and-seek.md) |
| Run and Catch | Chase minigame | [Run and Catch](muserver/events/run-and-catch.md) |
| Russian Roulette | Luck minigame | [Russian Roulette](muserver/events/russian-roulette.md) |
| Find the Path | Find the NPC hidden on the map | [Find the Path](muserver/events/find-the-path.md) |
| Quickly | Speed minigame | [Quickly](muserver/events/quickly.md) |
| Moss Merchant | Travelling merchant | [Moss Merchant](muserver/events/moss-merchant.md) |
| Custom Arena | Arena you design yourself | [Custom Arena](muserver/customs/custom-arena.md) |
| Quiz | Questions and answers | [Quiz](muserver/customs/quiz.md) |
| Drop Event | Item rain for a limited time | [Event Drop](muserver/customs/event-drop.md) |
| Online Lottery | Draw among connected players | [Online Lottery](muserver/customs/online-lottery.md) |
| Party vs Party | Party against party | - |
| Battle Soccer | MU's football match | - |
| King Event (King of MU) | Guilds and players compete for the crown; top scorers are rewarded | [King of MU](muserver/events/king-of-mu.md) |

### Event tools

| Tool | What it does | Docs |
|---|---|---|
| Bonus Manager | Experience and drop bonus windows by period, on all maps or selected ones | [Bonus Manager](muserver/events/bonus-manager.md) |
| On-screen announcements | Players are notified when an event starts | - |
| Per-event rewards and drops | Each event has its own reward and drop table | - |

## Emulator

| Area | Included |
|---|---|
| Character progression | First, second and third evolution; Master Level with the full skill tree; Reset and Master Reset (manual or automatic); Gens factions and ranking; quests including Quest World; configurable experience table and per-class balance |
| Items and crafting | Excellent, Ancient, Sockets, Jewel of Harmony and +380 options; full Chaos Machine; jewel and wing mix; stackable items, item bags and lucky items; drop rates per event, map and monster |
| Companions and gear | Wings of every generation (including custom), pets, Muun, mercenaries, mounts, Pentagram with errtels, Dark Spirit and raven, earrings and nation flags |
| Monster AI | Group, patrol route, conditional and state-based behaviour for real bosses, packs and ambushes; monster skills with element and option; invasions by map and time |
| Social and combat | Guilds with alliances, guild search and mobile logo editor; party with search and password; marriage with couple bonuses; friends and mail; PK system; open PvP fields; duels with arena and spectators; skill combos |
| Economy | NPC shops, personal shops, offline shops, Cash Shop and Morpheus Shop, tiered VIP, custom currencies with exchange, PC Point, password-protected warehouse, safe trade |
| Anti-cheat | Packet, skill and speed checks; per-account connection control; name and chat filters; GM account and character blocking; item duplication protection validated on the server |
| Security | Hardened network protocol between client and server |
| Operations | Server messages in each player's own language; event scheduler; hot reload of tables and settings; detailed logs and crash reports |

## Control panel

The control panel runs the server from a web browser. See the [Control panel guide](dashboard/README.md).

| Feature | What it does | Docs |
|---|---|---|
| Server control | Start, stop and restart each server with one click; CPU, memory and disk in real time; several servers on one account | [Control panel](dashboard/README.md) |
| Hot reload | Reload Cash Shop, Monster, Item, Server List, customs, languages and EventItemBag without taking the server down | [Control panel](dashboard/README.md) |
| File editor | Edit any configuration file in the browser, with Ctrl+F, line numbers, correct MU file encoding, overwrite protection and automatic copy of the previous version | [Control panel](dashboard/README.md) |
| Backups | Full folder backups, comparison with the live files and selective restore | [Control panel](dashboard/README.md) |
| Morpheus Shop editor | Item registration on the client and price catalogue on the server, saved together | [Morpheus Shop editor](dashboard/editors/morpheus-shop.md) |
| Cash Shop editor | Packages, categories, durations and prices, server and client kept in sync, bulk price adjustment | [Cash Shop editor](dashboard/editors/cash-shop.md) |
| Battle Pass editor | Season, rewards, free and premium tracks, missions, calendar and plans, with texts per language | [Battle Pass editor](dashboard/editors/battle-pass.md) |
| EventItemBag editor | Boxes, boss drops and event prizes, with trigger editing, real drop chances per class and a 1,000-opening simulation | [EventItemBag editor](dashboard/editors/event-item-bag.md) |
| NPC Shop editor | Build each NPC shop seeing the grid as the player sees it | [NPC Shop editor](dashboard/editors/npc-shop.md) |
| Spot editor | Place monsters and NPCs on the real map | [Spot editor](dashboard/editors/spot.md) |
| Client Hub | Visual editors for the server list, game texts, items, client settings, move requirements, mixes and tooltips; UI Editor with live preview of the game's web panels; review queue and one-click publish | [Client Hub](dashboard/client-hub/README.md) |
| Player updates | Players receive new content automatically when they open the game, downloading only what changed | [Client Hub](dashboard/client-hub/README.md) |
| Diagnostics | Crash reports from players' devices, update failures, per-service logs, session history and online players by platform | [Control panel](dashboard/README.md) |
| Accounts and roles | Owner and operator access levels, two-factor login and a tamper-evident activity log | [Signing in](dashboard/signing-in.md) |
| Languages | Panel available in English, Portuguese, Spanish, Vietnamese and Chinese | [Control panel](dashboard/README.md) |

## Version history

All notable changes, newest first.

## [Unreleased]

### Added

- **Battle Pass.** Seasonal pass with free and premium tracks per level, missions with explained conditions, a 28-day calendar, paid plans and experience sources. Season name, plans and mission texts are translated per language.
- **Battle Pass editor** in the control panel. It saves the engine file, the season file and the language texts together, all or nothing, with a backup.
- **Cash Shop editor.** Edit packages with the real item icon and name, categories, duration variants with price and options. One save writes the server and the client files together. Includes bulk price adjustment with a preview.
- **Morpheus Shop editor.** Client registration (tab, position, scale) and server catalogue (price per duration and currency, option costs, limits, discounts, texts per language) in one screen.
- **EventItemBag editor** for both the normal and advanced formats: drop rules explained field by field, item lists with options, trigger editing, real chance per class and evolution, and a simulation of 1,000 openings.
- **UI Editor** in the Client Hub, with a live preview of the game's web panels, including videos.
- **Client Hub editors** for move requirements, mixes, item tooltips, and game texts and items in every language, with a text mode using aligned columns.
- Server owners can review and publish their own pending Client Hub changes.
- Online players list shows each player's platform.
- Pagination in the logs and sent notifications.

### Changed

- Morpheus Shop: item code 0 (Kris) now has its own price range, limits and discounts like any other item.
- Morpheus Shop tabs 5, 12, 13 and 14 renamed to Staffs, Pendants/Rings, Wings/Pets and Others.
- Chaos Mix announcements are now translated into each player's language.
- The panel's language selector is now its own button on the top bar.

### Fixed

- Full-screen mode in the editors no longer gets stuck in its original position.

## [6.0] - 2026-08

Presence, fairness and convenience.

### Added

- **Unified PvP damage system.** In the original MU every class calculates damage with a different constant, so two characters with identical stats hit differently because of their class. There is now a single formula, identical for every class, with a configurable weight per stat and bonus rules per item. A master switch turns it off, and the server then falls back to the original calculation with no side effects.
- **Morpheus Tournament completed.** The Colosseum runs end to end: registration paid with an item, bracket draw, sudden-death matches, accumulated ranking, paginated history of previous editions and automatic prize delivery. The six-tab panel opens from the Colosseum NPC on all six platforms. Ranking and history read the real database. Betting ships turned off so you decide whether to enable it.
- **Offline MU Helper.** The player closes the game and the character keeps hunting.
- **Discord Rich Presence.** The player's character appears on their Discord profile with the server name.
- **Personal shop in a new panel**, with the same modern interface as the other windows.

### Changed

- **NPC windows on mobile.** You can choose by configuration which NPC windows use the native version and which use the new one.
- **Browser loading screen.** The logo is positioned like in the launcher and follows the window size.

## [5.0] - 2026-07

Languages and Apple performance.

### Added

- **Multilingual server.** Each player receives server notices and messages in the language they chose, and can switch language inside the game with immediate effect, without logging out.
- **Translations packaged with the client**, removing mismatches between the text version and the game version.

### Changed

- **iPhone at 120 frames per second.** The Apple client went from 55-60 frames with stutter in combat to a locked 120 frames on the iPhone 17 Pro, including crowded scenes with skill effects. The freeze when changing zones was removed.

## [4.0] - 2026-06

The game in a browser.

### Added

- **Playable client in the browser.** MU opens in a tab with nothing to install: login, character selection and the world, with audio and the same protection as the installed client. Google login shipped alongside it.
- **Jewel Bank**, with the jewels rendered in 3D by the engine and duplication protection validated on the server.
- **Rebuilt mobile interface.** New inventory, party, guild, commands, character, pet and quest panels on mobile: move, equip, drop, repair and auto-equip by touch. Guild creation on mobile completed, including the logo editor.

### Changed

- **Visual standardization.** Login, character selection and creation, system menu, name plates and the guild window follow one visual standard across platforms.
- **Mobile camera.** Larger camera controls and an option to hide the interface for clean screenshots.

## [3.0] - 2026-06

Security and voice.

### Added

- **Hardened network protocol.** Traffic between client and server is protected in both directions.
- **Party voice.** Voice chat inside the game on PC, Android and browser, with no separate program and no third-party service. The connection stays alive with the panel closed, and an indicator shows who is speaking.
- **Automatic crash reporting.** Game crashes on the player's device and update download failures arrive in the control panel with device model and version.
- **Packaged updates.** Updates download as a single package plus the changed files instead of thousands of separate downloads: faster, with far fewer failures halfway through.

### Changed

- **Android performance.** The Android app is now shipped as an optimized build, with a considerable performance gain.
- **Panel in five languages.** Panel, notifications and the presentation page translated into English, Portuguese, Spanish, Vietnamese and Chinese.

## [2.0] - 2026-06

The game on mobile (released across May and June 2026).

### Added

- **Complete Android client** on Android's modern graphics stack: joystick, skill ring, health and mana bars, minimap, quick potions and the game windows. The full game, not a reduced version.
- **Morpheus Shop.** The premium in-game store where the player builds the item before buying.
- **Loading and character selection screens** rebuilt with their own scene and clean transitions.

### Changed

- **Mobile performance.** Optimizations on terrain, on-screen characters and texture loading. The initial load became fast and the frame rate rose consistently.
- **Bandwidth-efficient launcher.** Files are verified before downloading, so content the player already has is not downloaded again.

## [1.0] - 2026-05

The foundation.

### Added

- **Complete emulator**: the seven server programs, character progression, items, companions, economy, social systems, combat, monster AI and anti-cheat.
- **More than 40 custom systems**: automatic assistants, minigames, customization, rankings and the 88 commands.
- **32 events**: the 14 MU classics and the 18 built for Morpheus, with time scheduling.
- **Control panel**: service control, file editor, backups and access roles.
- **Balance analysis tool.** Simulates all 225 matchups among the 15 classes with the server's real formulas and produces a report with the win-rate matrix and proposed corrections. Runs in analysis mode by default and only writes after your confirmation, always with a backup.
- **Windows client and launcher**, in 32 and 64 bit, with the launcher updating the game before entry.
