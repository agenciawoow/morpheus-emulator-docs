# GameServer overview

The GameServer runs the game world. Its behaviour is set by seven INI files named `GameServerInfo - <Name>.dat`, plus the game content in the shared root `Data/` folder.

## Where it lives

| Folder | What |
|---|---|
| `GameServer/GameServer.exe` | The program |
| `GameServer/Data/GameServerInfo - *.dat` | The seven settings files of this GameServer |
| `GameServerCS/Data/GameServerInfo - *.dat` | The seven settings files of the Castle Siege server (its own copy) |
| `Data/` (MuServer root) | Game content shared by all GameServers (items, monsters, events, customs, messages) |

Each GameServer instance (including GameServerCS) has its **own** seven files. If you run two normal GameServers, change the files of each one. See [Multiple GameServers](multiple-gameservers.md).

## The seven files

| File | What it controls | Page |
|---|---|---|
| `GameServerInfo - Common.dat` | Server identity and ports, connections, rates (experience, drop, zen), PK, trade, guild, party, potions, jewels, MU Helper, offline helper, party voice, anti-hack checks, cash shop, many premium switches | [Common](common.md) |
| `GameServerInfo - Character.dat` | PvP and PvM damage rates per class and class against class, damage and defense formulas, recovery rates, pets and guardians | [Character](character.md) |
| `GameServerInfo - ChaosMix.dat` | Chaos Machine success rates per mix and account level | [ChaosMix](chaosmix.md) |
| `GameServerInfo - Command.dat` | Extra settings of chat commands: reset, master reset, warehouse, marry, rewards, rename, help | [Command](command.md) |
| `GameServerInfo - Custom.dat` | On/off switches and texts of the custom systems | [Custom](custom.md) |
| `GameServerInfo - Event.dat` | On/off switches and basic limits of every event | [Event](event.md) |
| `GameServerInfo - Skill.dat` | Buff and skill formulas (Mana Shield, Greater Defense, Greater Damage, Swell Life...) | [Skill](skill.md) |

Related pages:

- [Castle Siege server (GameServerCS)](castle-siege-server.md)
- [Multiple GameServers](multiple-gameservers.md)
- [Languages](languages.md)

## How to read the key tables

All seven files are INI files with a single `[GameServerInfo]` section. The general rules (comments, spaces, case, duplicates, missing keys) are in [File format basics](../file-format-basics.md#ini-files).

In the key tables of these pages:

| Column | Meaning |
|---|---|
| **Key** | The exact key name. `_AL0..3` means four keys, one per account level (see below). |
| **Default** | The value the server uses **if the key is missing** from the file. This is often `0` (off), which is not always what the shipped file has. Where useful, the description also gives the shipped value. |
| **Values** | Accepted values and units. |
| **Description** | What it does. |

### Account levels: AL0 to AL3

Many keys come in four copies, one for each account level (VIP tier). They end in `_AL0`, `_AL1`, `_AL2` and `_AL3`:

| Suffix | Account level | Default name shown in game |
|---|---|---|
| `_AL0` | Free account | Free |
| `_AL1` | VIP 1 | Bronze |
| `_AL2` | VIP 2 | Silver |
| `_AL3` | VIP 3 | Gold |

The names shown in game come from the language files (`Data/Lang/`, texts 32 to 35). In the tables, `AddExperienceRate_AL0..3` stands for the four keys `AddExperienceRate_AL0`, `AddExperienceRate_AL1`, `AddExperienceRate_AL2` and `AddExperienceRate_AL3`.

```ini
AddExperienceRate_AL0 = 200   ; Free
AddExperienceRate_AL1 = 220   ; VIP 1
AddExperienceRate_AL2 = 230   ; VIP 2
AddExperienceRate_AL3 = 240   ; VIP 3
```

### Class abbreviations

Per-class keys start or end with a class code:

| Code | Class | Code | Class |
|---|---|---|---|
| `DW` | Dark Wizard | `GL` | Grow Lancer |
| `DK` | Dark Knight | `RW` | Rune Wizard |
| `FE` / `ELF` | Fairy Elf | `SL` | Slayer |
| `MG` | Magic Gladiator | `GC` | Gun Crusher |
| `DL` | Dark Lord | `KUN` | Kundun |
| `SU` | Summoner | `LEM` | Lemuria |
| `RF` | Rage Fighter | `KIN` | Illusion Knight |

The Season 6 client uses the first seven classes (DW to RF). Keys for the other classes only matter if your client supports those classes.

### Coins

Several settings pay or charge "coins". The numbering is the same everywhere:

| Coin | Currency |
|---|---|
| Coin1 | WCoinC (cash) |
| Coin2 | WCoinP |
| Coin3 | Goblin Points |

### Premium

Blocks marked **(Premium)** in the file are Morpheus additions that only exist in the Morpheus build. They are configured like any other key.

## Reload modules

Most settings can be applied without restarting. Use one of:

- the in-game GM command `/reload <module>`;
- the **Reload** menu of the GameServer window;
- the control panel (see [Services and reload](../../dashboard/services-and-reload.md)).

| Module | Re-reads |
|---|---|
| `common` | `GameServerInfo - Common.dat` (except the startup-only keys below), `Effect.txt`, `MapManager.txt`, `MapServerInfo.dat`, `Mining.txt`, the language files |
| `character` | `GameServerInfo - Character.dat`, `Data/Character/DefaultClassInfo.txt` |
| `chaosmix` | `GameServerInfo - ChaosMix.dat` |
| `command` | `Data/Command.txt`, `GameServerInfo - Command.dat` |
| `custom` | `GameServerInfo - Custom.dat` and every file in `Data/Custom/` |
| `event` | `GameServerInfo - Event.dat` and the files in `Data/Event/`. Event timers restart. |
| `skill` | `GameServerInfo - Skill.dat`, `Data/Skill/` |
| `hack` | The anti-hack keys of `GameServerInfo - Common.dat`, `Data/Hack/` |
| `cashshop` | The cash shop keys of `GameServerInfo - Common.dat`, `Data/CashShop/` |
| `eventitembag` | `Data/EventItemBagManager.txt`, `Data/EventItemBag/` |
| `item` | `Data/Item/` |
| `lang` | `Data/LangConfig.txt`, `Data/Lang/`, `DefaultLanguageSystem` |
| `map` | `Data/MapManager.txt`, `Data/MapServerInfo.dat` |
| `monster` | `Data/Monster/`, `Data/MonsterSetBase/` |
| `move` | `Data/Move/`, `Data/Custom/CustomMove.txt` |
| `quest` | `Data/Quest/`, `Data/QuestWorld/` |
| `shop` | `Data/ShopManager.txt`, `Data/Shop/`, `Data/Item/ItemValue.txt` |
| `util` | `Data/Util/` (experience table, filters, GM list, notices, reset tables) |
| `terrain` | `Data/Terrain/` (refused while an event is running unless you use `/reload terrain force`) |
| `bots` | `Data/Custom/BotSystem/` |
| `all` | Every module above except `item` and `terrain` |

### Startup-only keys

These keys of `GameServerInfo - Common.dat` are read only when the GameServer starts. Changing them requires a **restart**:

`ServerName`, `ServerCode`, `ServerPort`, `ServerVersion`, `ServerSerial`, `DataServerAddress`, `DataServerPort`, `JoinServerAddress`, `JoinServerPort`, `ConnectServerAddress`, `ConnectServerPort`, `WriteChatLog`, `WriteCommandLog`, `WriteTradeLog`, `WriteConnectLog`, `WriteHackLog`, `WriteCashShopLog`, `WriteChaosMixLog`, `WriteBattlePassLog`.

{% hint style="warning" %}
A reload replaces the settings for everyone at once. Avoid `/reload event` while an event is running, and avoid `/reload monster` in the middle of an invasion: those modules restart their timers and spawns.
{% endhint %}
