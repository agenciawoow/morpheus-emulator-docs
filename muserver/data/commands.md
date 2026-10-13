# Chat commands (Command.txt)

## What it does

`Command.txt` lists every chat command: its name, whether it is enabled, and what it costs and requires **per account level** (VIP). It also says which commands are for Game Masters only.

Behaviour details that a table can't express (how `/reset` works, how many warehouses `/bau` has, reward texts...) are in `GameServerInfo - Command.dat` — see [GameServerInfo - Command](../gameserver/command.md). Help texts shown to players come from [Command Description](../customs/command-description.md).

## Where it lives

`MuServer/Data/Command.txt`

## Syntax

One line per command, then `end`. Every requirement column has **four** values: `AL0 AL1 AL2 AL3` = Free, VIP 1 (Bronze), VIP 2 (Silver), VIP 3 (Gold).

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0–87 | **Which function** the line controls. Fixed by the server: never renumber lines. |
| `Command` | `"/text"` | What players type. You **can** rename it (e.g. `"/bau"` → `"/ware"`). Case-insensitive. |
| `Enable` ×4 | 0 / 1 | `1` = enabled for that account level. |
| `Money` ×4 | Zen | Zen charged per use. `0` = free. |
| `MinLevel` ×4 | level | Minimum level. |
| `MaxLevel` ×4 | level | Maximum level. `0` = no limit. |
| `MinReset` ×4 | number | Minimum resets. |
| `MaxReset` ×4 | number | Maximum resets. `0` = no limit. |
| `Delay` | seconds | Cooldown between uses (per character). `0` = none. |
| `GameMaster` | 0 / 1 / 2 | `0` = everyone; `1` = Game Masters and Admins; `2` = Admins only (see [GameMaster.txt](util.md)). |
| `WC` | number | WCoinC charged per use. |
| `WP` | number | WCoinP charged per use. |
| `GP` | number | Goblin Points charged per use. |
| `Description` | `// text` | Comment for you. |

### Example (real lines)

```
//      #Index	#Command	   #Enable->	AL0	AL1	AL2	AL3	#Money->	AL0	AL1	AL2	AL3	... #Delay	#GameMaster	#WC	#WP	#GP	#Description
	11        "/reset"			1	1	1	1			0	0	0	0	...	0	0		0	0	0	//reset |auto|			- Reset character
	35        "/reload"			1	1	1	1			0	0	0	0	...	0	2		0	0	0	//reload <type>			- Reload config
```

### Example: paid `/pk` only for VIP, with a 60-second cooldown

```
	7        "/pk"				0	1	1	1			0	5000000	3000000	1000000			0	0	0	0			0	0	0	0			0	0	0	0			0	0	0	0		60	0		0	0	0	//pk - Clear PK
```

Free accounts can't use it; Bronze pays 5M Zen, Silver 3M, Gold 1M; one use per minute.

## Every command

"Shipped" shows the state of the shipped file: **on** = enabled for all account levels, **off** = disabled. "Who" is the `GameMaster` column.

| # | Command | Shipped | Who | Purpose |
|---|---|---|---|---|
| 0 | `/move <map>` | on | All | Warp to a map from [Move.txt](move.md). |
| 1 | `/post <msg>` | on | All | Global chat message (all players). |
| 2 | `/s <points>` | on | All | Add free points to Strength. |
| 3 | `/a <points>` | on | All | Add free points to Agility. |
| 4 | `/v <points>` | on | All | Add free points to Vitality. |
| 5 | `/e <points>` | on | All | Add free points to Energy. |
| 6 | `/c <points>` | on | All | Add free points to Command (Dark Lord). |
| 7 | `/pk` | on | All | Clear PK status. |
| 8 | `/zen <value>` | off | All | Add Zen to yourself. |
| 9 | `/evo` | off | All | Class change (evolve) by command. |
| 10 | `/bau <num>` | on | All | Switch to another warehouse page. |
| 11 | `/reset [auto]` | on | All | Reset the character (see [Reset tables](reset-and-experience.md)). |
| 12 | `/gmmove <name> <map> <x> <y>` | on | Admin | Move a player. |
| 13 | `/gmpost <msg>` | on | Admin | Global message with GM tag. |
| 14 | `/track <name>` | on | Admin | Go to a player. |
| 15 | `/trace <name>` | on | Admin | Bring a player to you. |
| 16 | `/disconnect <name>` | on | Admin | Disconnect a player. |
| 17 | `/fireworks` | on | Admin | Fireworks at your position. |
| 18 | `/make <group> <item> <level> <skill> <luck> <option> <exc> <set> <socket>` | on | Admin | Create an item in your inventory. |
| 19 | `/skin <name> <skin>` | on | Admin | Change a player's appearance (monster skin). |
| 20 | `/setmoney <name> <value>` | on | Admin | Set a player's Zen. |
| 21 | `/notice <msg>` | on | Admin | Notice to all servers. |
| 22 | `/mreset` | on | All | Master Reset (see [Reset tables](reset-and-experience.md)). |
| 23 | `/war <guild>` | on | All | Challenge another guild to a guild war. |
| 24 | `/soccer <guild>` | on | All | Challenge another guild to Battle Soccer. |
| 25 | `/re on\|off\|auto [password]` | on | All | Accept or refuse requests from other players; `auto` accepts party requests automatically (only from players who give the password, if one is set). |
| 26 | `/hide` | on | GM | Become invisible (GM). |
| 27 | `/rei` | on | All | Seize the throne in the [King of MU](../events/king-of-mu.md) event. |
| 28 | `/limparinv` | on | All | Clear your inventory. |
| 29 | `/setvip <account> <type> <days>` | off | Admin | Give VIP to an account. |
| 30 | `/marry to <player>` | on | All | Marriage system. |
| 31 | `/abrirbau` | on | All | Open the warehouse anywhere. |
| 32 | `/classe <type>` | off | All | Change character class. |
| 33 | `/premiar <name> <type> <value>` | on | Admin | Reward a player (coins). |
| 34 | `/premiarall <type> <value>` | on | Admin | Reward every online player. |
| 35 | `/reload <type>` | on | Admin | Reload server files (see [Reload types](#reload-types)). |
| 36 | `/banchar <player>` | on | Admin | Ban a character. |
| 37 | `/banacc <account>` | on | Admin | Ban an account. |
| 38 | `/mudarnome <newname>` | off | Admin | Rename character (checked against [FilterRename.txt](util.md)). |
| 39 | `/veloz` | on | Admin | Start the [Quickly](../events/quickly.md) event. |
| 40 | `/info <player>` | on | Admin | Show a character's status. |
| 41 | `/abrirevento` | on | Admin | Open an [Event Start](../events/event-start.md) gathering. |
| 42 | `/participar` | on | All | Join the Event Start gathering. |
| 43 | `/pegapega` | on | Admin | Start [Run and Catch](../events/run-and-catch.md). |
| 44 | `/queroir` | on | All | Join Run and Catch. |
| 45 | `/escesc` | on | Admin | Start [Hide and Seek](../events/hide-and-seek.md). |
| 46 | `/makeset <item> <level> <skill> <luck> <option> <exc> <set> <socket>` | on | Admin | Create a full armor set. |
| 47 | `/drop <group> <item> <level> <skill> <luck> <option> <exc> <set> <socket>` | on | Admin | Drop an item on the ground. |
| 48 | `/quest` | on | All | [Custom Quest](../customs/custom-quest.md) progress / turn-in. |
| 49 | `/presente` | off | Admin | Gift system command. |
| 50 | `/roletarussa` | on | Admin | Start [Russian Roulette](../events/russian-roulette.md). |
| 51 | `/top <type>` | off | Admin | Show a ranking (see [Custom Top](../customs/custom-top.md)). |
| 52 | `/readd` | on | All | Reset distributed stat points and get them back. |
| 53 | `/resp <answer>` | on | All | Answer the [Quiz](../customs/quiz.md). |
| 54 | `/pick <type>` | on | All | Auto-pick items (see [Pick](../customs/pick.md)). |
| 55 | `/ajuda` | off | Admin | Summon helper monsters that fight with you (again to dismiss). |
| 56 | `/remaster` | on | All | Reset the Master Skill Tree and get the points back. |
| 57 | `/startbc` | on | Admin | Start [Blood Castle](../events/blood-castle.md) now. |
| 58 | `/startds` | on | Admin | Start [Devil Square](../events/devil-square.md) now. |
| 59 | `/startcc` | on | Admin | Start [Chaos Castle](../events/chaos-castle.md) now. |
| 60 | `/startit` | on | Admin | Start [Illusion Temple](../events/illusion-temple.md) now. |
| 61 | `/offpvp` | off | Admin | Turn the character's PvP off/on. |
| 62 | `/lock <password>` | on | All | Lock item movement (anti-theft). |
| 63 | `/unlock <password>` | on | All | Unlock item movement. |
| 64 | `/store <type>` | on | All | Open a [Custom Store](../customs/server-features.md#custom-store-offline-store) (jewels/coins). |
| 65 | `/offstore` | on | All | Keep the custom store open offline. |
| 66 | `/attack [skill] [buff]` | on | All | Start/stop [Custom Attack](../customs/server-features.md#custom-attack-offline-attack). |
| 67 | `/offattack` | on | All | Keep attacking offline. |
| 68 | `/moveall <map> <x> <y>` | on | Admin | Move every online player. |
| 69 | `/moveguild <guild> <map> <x> <y>` | on | Admin | Move every member of a guild. |
| 70 | `/pvp` | on | Admin | Start the [PvP 1x1](../events/pvp-1x1.md) event. |
| 71 | `/matamata` | on | Admin | Start [Kill All](../events/kill-all.md). |
| 72 | `/starttvt` | on | Admin | Start [TvT](../events/tvt-event.md). |
| 73 | `/spot <monster> <qty>` | on | Admin | Summon monsters around you. |
| 74 | `/setcoin <char> <coin> <value>` | on | Admin | Add or remove coins (WCoinC/WCoinP/Goblin Points). |
| 75 | `/pack <type> <qty>` | on | All | Pack jewels into bundles. |
| 76 | `/unpack <type> <qty>` | on | All | Unpack bundles into jewels. |
| 77 | `/startquiz` | on | Admin | Start the [Quiz](../customs/quiz.md). |
| 78 | `/startdrop` | on | Admin | Start the [Event Drop](../customs/event-drop.md). |
| 79 | `/startking` | on | Admin | Start [King of MU](../events/king-of-mu.md). |
| 80 | `/startinvasion <index>` | on | Admin | Start an [Invasion](../events/invasions.md). |
| 81 | `/startarena <index>` | on | Admin | Start a [Custom Arena](../customs/custom-arena.md). |
| 82 | `/startgvg` | on | Admin | Start [GvG](../events/gvg-event.md). |
| 83 | `/startfindpath` | on | Admin | Start [Find the Path](../events/find-the-path.md). |
| 84 | `/setparty <password>` | on | All | Set a password for joining your party. |
| 85 | `/mapset` | on | Admin | Reserved: no function in this version. |
| 86 | `/mtstart` | on | Admin | Start the [Morpheus Tournament](../events/morpheus-tournament.md). |
| 87 | `/mtwin <1\|2>` | on | Admin | Decide a stopped tournament fight (when `BothAbsentAction = 2`). |

### Reload types

`/reload <type>` accepts:

| Type | Reloads |
|---|---|
| `cashshop` | Cash Shop settings and files |
| `chaosmix` | `GameServerInfo - ChaosMix.dat` |
| `character` | `GameServerInfo - Character.dat`, `DefaultClassInfo.txt` |
| `command` | `Command.txt`, `GameServerInfo - Command.dat` |
| `common` | `GameServerInfo - Common.dat`, `Effect.txt`, `MapManager.txt`, `MapServerInfo.dat`, languages, `Mining.txt` |
| `custom` | `GameServerInfo - Custom.dat` and `Custom/` files (also `ItemValueTrade.txt`) |
| `event` | `GameServerInfo - Event.dat` and `Event/` files |
| `eventitembag` | `EventItemBagManager.txt` and `EventItemBag/` |
| `hack` | Hack settings, `HackPacketCheck.txt`, `HackSkillCheck.txt` |
| `item` | `Item/` files |
| `lang` | `LangConfig.txt`, `Lang/` |
| `map` | `MapManager.txt`, `MapServerInfo.dat` (keeps old tables on error) |
| `monster` | `Monster/` files and all spawns |
| `move` | `Gate.txt`, `Move.txt`, `MoveSummon.txt`, `CustomMove.txt` |
| `quest` | `Quest/`, `QuestWorld/` |
| `shop` | `ItemValue.txt`, `ShopManager.txt`, `Shop/` |
| `skill` | `GameServerInfo - Skill.dat`, `Skill/` files |
| `terrain` (`force`) | `Terrain/*.att` |
| `util` | `Util/` files |
| `bots` | Bot buffer files |
| `all` | Everything above **except** `item` and `terrain` |

## Rules & pitfalls

- **Never change the `Index`** of a line: index 11 is always the reset function, whatever its name is.
- Disabled (`Enable 0`) for one account level means that level gets "command disabled", even GMs.
- `GameMaster 1` or `2` requires the character to be in `GameMaster.txt`; account level (VIP) does not matter.
- Each line must have all 31 values (index, name, 6 × 4 requirement values, delay, GM level, 3 coins). A missing value shifts every following command.
- Renaming a command to an existing name makes only the first one reachable.

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

```
/reload command
```
