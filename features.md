# Feature overview

Morpheus Emulator is the complete MU Online base plus more than forty systems that the original game never had, a set of 32 events, clients for six platforms and a control panel that runs everything from a web browser.

This page explains what each part brings to your server. For the full list of systems and the release history, see the [Changelog](CHANGELOG.md).

## The product in numbers

| | |
|---|---|
| 6 platforms | Windows, Android, iPhone and iPad, macOS, web browser and mobile browser |
| 32 events | 14 MU classics and 18 built for Morpheus |
| 88 commands | Player, shop, event and moderation commands |
| 7 in-game languages | English, Portuguese, Spanish, Chinese, Korean, Vietnamese and Thai |
| 7 server programs | Connect, Data, Join, Game, GameServerCS, Relay and Voice |
| 40+ custom systems | Modules that do not exist in the original MU |
| 225 class matchups | Every pair among the 15 classes, simulated by the balance tool |

## One server, every platform

These are not six different games. It is the same server, the same account and the same character. A player on a phone joins the same party as a player on a PC.

| Platform | Highlights |
|---|---|
| Windows | Full client in 32 and 64 bit, with a launcher that updates the game before entry. Modern panels, with MU's classic cursor preserved. |
| Android | The real game on the phone: on-screen joystick, skill ring, touch inventory, every game window, chat, shop, map and warp. Updates itself when opened. |
| iPhone and iPad | Native client on Apple's graphics stack, running at 120 frames per second on the iPhone 17 Pro, even in crowded combat. |
| macOS | The same native client, with its own Mac launcher. |
| Web browser | The game opens in a tab with nothing to install. The cheapest front door there is: players try your server before downloading anything. |
| Mobile browser | The same tab, with Android's touch controls. |

On every platform:

- The client ships in seven languages, switchable inside the game.
- New content arrives automatically when the player opens the game, downloading only what changed.
- When the game crashes on a device, the report reaches your control panel without you having to ask the player for anything.

## Flagship systems

### Offline MU Helper

The player closes the game and the character keeps hunting. It is the feature that best retains adult players who cannot leave a PC running. You control the duration, minimum level, experience rate and auto potion and buff delays per account level. See [Server features](muserver/customs/server-features.md).

### Morpheus Shop

The premium in-game store. The player builds the item before buying: level, luck, skill, additional option, excellent, socket and harmony, within the limits you define. They buy exactly what they want, and you control exactly what they are allowed to build. See [Morpheus Shop](muserver/customs/morpheus-shop.md) and the [Morpheus Shop editor](dashboard/editors/morpheus-shop.md).

### Battle Pass

A seasonal pass with a free track and a premium track per level, missions, a 28-day calendar and paid plans. Season name, plans and missions are written per language. See [Battle Pass](muserver/customs/battle-pass.md) and the [Battle Pass editor](dashboard/editors/battle-pass.md).

### Jewel Bank

A jewel bank where players deposit and withdraw jewels, with the jewels rendered in 3D by the game engine. Duplication protection is validated on the server.

### Party voice

Players talk by voice inside the game, on PC, Android and browser, with no Discord and no separate program. The voice server is part of the package, with no third-party service and no external subscription. An indicator shows who is speaking. See [Server features](muserver/customs/server-features.md).

### Discord Rich Presence

The player's character shows up on their Discord profile with your server's name. Every connected player advertises your server to their friend list, at no cost.

## Morpheus Tournament: the Colosseum

A one-on-one single-elimination tournament that the server runs entirely on its own. It opens registration, draws the bracket, calls the fighters, judges every match, builds the ranking and hands out the prize. The GM does not have to referee anything.

Players follow everything in a dedicated panel opened from the Colosseum NPC. There is no HUD button, so the tournament stays a place in the world rather than one more menu.

**How the competition works**

- Up to 64 fighters per edition.
- Registration costs an item that you define. This gives the slot weight and keeps no-shows out of the bracket.
- Each match is a single sudden-death fight. The server records the winner and the reason: death, opponent no-show, disconnection, life timeout, damage timeout or GM decision.
- Players who are not fighting can watch: the panel teleports spectators to the arena.

**The six tabs**

| Tab | Content |
|---|---|
| Bracket | The tournament tree, from the first match to the final |
| Matches | The current round, who entered and who advanced |
| Betting | Odds, your bets and your balance |
| Ranking | The accumulated standings |
| Rules | The format and what applies to this specific edition |
| History | Previous editions: date, participants, format, the champion and the final rounds |

{% hint style="info" %}
Betting ships **turned off**. When you enable it, odds come from the pools themselves, players follow their own bets by state (all, open, won, lost) with totals and balance, and an opponent no-show still pays out.
{% endhint %}

See [Morpheus Tournament](muserver/events/morpheus-tournament.md).

## PvP balance

In the original MU each class calculates damage with a different constant. Two characters with exactly the same stats hit differently purely because of their class, and there is no fair way to compare them. This is the root of the eternal "that class is broken" complaint.

Morpheus solves it in two parts.

### A single PvP damage formula

When enabled, PvP damage is calculated by one formula, identical for every class, fed by the character's base stats. You control:

| Setting | What it controls |
|---|---|
| Weight of each stat | How much strength, agility, energy, vitality and command contribute. Weight zero means the stat does not count. |
| Rates | Minimum damage, maximum damage, normal hit, skill and pet multipliers |
| Defense | Defense base and how much agility contributes |
| Damage floor | The minimum that always lands, so no matchup becomes a permanent zero |
| Life and shield | Base value and how much vitality adds |
| Damage over time | Controlled separately, along with reflect |
| Per-class rate | A fine adjustment per class on top of the formula |

Per-item rules define how much a specific item adds in PvP and under which conditions: minimum level, excellent, ancient, +380, socket, harmony or luck. Gear matters without deciding the fight on its own.

{% hint style="success" %}
Nothing breaks when it is off. With the master switch off, or if the table has an error, the server falls back to MU's original calculation. The equipment bonus is recalculated only when the player changes an item, logs in or receives a buff, not on every hit.
{% endhint %}

### The balance analysis tool

The tool reads your GameServer configuration, builds the best gear set for each of the 15 classes and simulates all 225 matchups, a thousand fights each, using the server's real formulas. The result is a report that opens in a browser, with the full win-rate matrix, the worst imbalances highlighted and the proposed changes.

It then adjusts nine configuration levers (class-versus-class matrix, per-class multiplier, physical and magical damage divisors, per-class accuracy and mana shield absorption) to bring every matchup into a healthy win-rate band, 45% to 55% by default.

- It runs in analysis mode by default and waits for your confirmation.
- It makes an automatic backup before any change.
- Every value has a ceiling and a floor, and the total change per run is capped.

On a real run, matchups inside the healthy band went from 9% to 82%, and the average deviation from perfect balance dropped from 35.2 to 5.9 points. When a player says their class is weak, you have a report to show.

See [Balance Class](muserver/customs/balance-class.md).

## Automatic assistants

- Hunting helper with automatic potions and return to the spot.
- Automatic alchemist, buffer and warper.
- Automatic reset and PK clearing.
- Automatic reward and shop.
- Offline store and offline attack, which keep working while the player is disconnected.
- [Bot Buffer](muserver/customs/bot-buffer.md) characters that buff players.

## Minigames and extras

| Feature | Description |
|---|---|
| Mining | Five stages with jewel packs as rewards |
| MU Rummy | A card game inside MU |
| [Online Lottery](muserver/customs/online-lottery.md) | Drawn among connected players |
| [Quiz](muserver/customs/quiz.md) | Questions and answers you register |
| Event roulette | Luck-based rewards |

## Server customization

- [Custom Wing](muserver/customs/custom-wing.md), [Custom Jewel](muserver/customs/custom-jewel.md) and [Custom Mix](muserver/customs/custom-mix.md) recipes.
- [Balance Class](muserver/customs/balance-class.md), custom attack and [Combo](muserver/customs/combo.md) for every class.
- [Start Item](muserver/customs/start-item.md) per class.
- [Custom Quest](muserver/customs/custom-quest.md) and [NPC customs](muserver/customs/npc-customs.md): collector NPCs, command NPCs and NPCs that move.
- [Rankings](muserver/customs/ranking.md) and player Top lists.
- [Death Message](muserver/customs/death-message.md), [Custom Move](muserver/customs/custom-move.md), [Buy VIP](muserver/customs/buy-vip.md), [Exchange Coin](muserver/customs/exchange-coin.md) and [Pick](muserver/customs/pick.md).
- Custom description for every command.
- Experience and drop bonus by period with the [Bonus Manager](muserver/events/bonus-manager.md).

## Commands

There are 88 registered commands, each with its own permission and requirements.

| Group | Examples |
|---|---|
| Player | `/move` `/add` `/reset` `/remaster` `/pkclear` `/ware` `/post` `/marry` `/quest` `/make` `/pick` `/helper` `/top` |
| Shop | `/store` `/offstore` `/offattack` |
| Events | `/startbc` `/startds` `/startcc` `/startit` `/starttvt` `/startgvg` `/startquiz` `/startdrop` `/startking` `/startinvasion` `/startarena` |
| Moderation | `/notice` `/gift` `/reward` `/rewardall` `/rename` `/moveall` `/moveguild` `/disablepvp` `/lock` `/killall` `/reload` |

## Events

32 events, each running on a schedule, by GM command or from a single click in the control panel. Rewards and drops are configured event by event, and players see an on-screen announcement when an event starts. See the event list in the [Changelog](CHANGELOG.md#events).

## Rebuilt interface

- Inventory, guild, party, character, pet and quest rebuilt as new panels, with the same look on all platforms.
- Personal shop, command window and NPC dialogs redesigned.
- Minimap, name plates and status bars unified.
- Character selection screen rebuilt with its own scene.
- Mobile camera with larger controls and an option to hide the interface for screenshots.

## Multilingual server

Every player receives server notices and messages in their own language and can switch language inside the game without logging out. Translations ship inside the client package, so text and game versions always match.

## The emulator underneath

| Area | Included |
|---|---|
| Progression | Three evolutions per class, Master Level with the full skill tree, Reset and Master Reset, Gens, quests including Quest World |
| Items | Excellent, Ancient, Sockets, Jewel of Harmony, +380, full Chaos Machine, item bags, lucky items |
| Companions | Wings of every generation, pets, Muun, mercenaries, mounts, Pentagram, Dark Spirit and raven, earrings and nation flags |
| Monster AI | Group, patrol, conditional and state-based behaviour for real bosses, packs and ambushes |
| Social | Guilds with alliances and search, party with search and password, marriage, friends and mail |
| Combat | PK system, open PvP fields, duels with spectators, skill combos |
| Economy | NPC, personal and offline shops, Cash Shop, Morpheus Shop, tiered VIP, custom currencies, PC Point, vault, safe trade |
| Anti-cheat | Packet, skill and speed checks, chat and name filters, GM blocking, server-side duplication protection, hardened network protocol |

## Control panel

You run the entire server from a web browser, from anywhere:

- Start, stop and restart each server; watch CPU, memory, disk and online services.
- Hot reload items, monsters, shop, server list and customs without taking the server down.
- Edit any configuration file in the browser, saved in the exact format MU expects, with overwrite protection and an automatic copy of the previous version.
- Back up a service folder, compare it with the live files and restore only the files you choose.
- Visual editors for the [Morpheus Shop](dashboard/editors/morpheus-shop.md), [Cash Shop](dashboard/editors/cash-shop.md), [Battle Pass](dashboard/editors/battle-pass.md), [EventItemBag](dashboard/editors/event-item-bag.md), [NPC shops](dashboard/editors/npc-shop.md) and [spawn spots](dashboard/editors/spot.md).
- The [Client Hub](dashboard/client-hub/README.md) to change what players see (server list, game texts, items, client settings, game panels) and publish it with one button.
- Crash reports from players' devices, per-service logs and session history.
- Two-factor sign-in, separate owner and operator access, and a tamper-evident activity log.

See the [Control panel guide](dashboard/README.md).
