# Client-side customs (MainInfo)

What lives in `Tools/MainInfo`, how it reaches the players, and which server files it must stay in sync with.

## What it is

Some features need data **on the player's device**, not only on the server: custom item models, wing stats shown in the tooltip, jewel rules for drag-and-drop, the in-game shop catalogue, the event icons, the command window, the menu buttons, and so on. That data lives in `Tools/MainInfo/` and is packed into the client data files (`Data/morph.pak` and `Data/Local/morpheus.emu`) by the **GetMainInfo** screen of the Morpheus control panel (or by `Tools/MainInfo/Encoder.exe`).

| Path (relative to `MuServer/Tools/MainInfo/`) | What it holds | Page |
|---|---|---|
| `MainInfo.ini` | On/off switches and options for the client (menus, interface, helper, Discord, login buttons...) | [MainInfo.ini & MainConnect.ini](maininfo.md) |
| `MainConnect.ini` | Connection and window settings (server address, window title...) | [MainInfo.ini & MainConnect.ini](maininfo.md) |
| `Local/*.txt` | Custom items, wings, jewels, monsters, maps, commands, shop list, event icons, translations | [Custom files (Local)](custom-files.md) |
| `Common/Effect/*.txt` | Glow/particle effects attached to items and pets (the copy that is actually packed) | [Effects & ItemManager](effects-and-item-manager.md) |
| `ItemManager/*.txt` | Reference files for the item tooling. **Not packed** into the client | [Effects & ItemManager](effects-and-item-manager.md) |

{% hint style="info" %}
The control panel has an editor for `MainInfo.ini`, `MainConnect.ini` and every `Local/*.txt` file (GetMainInfo screen, ".txt files" tab). Editing there and publishing does the repack for you.
{% endhint %}

## When you must repack and redistribute the client

Client-side files are **never** hot-reloaded. `/reload` only reloads server files. Any change under `Tools/MainInfo/` reaches the players only after you:

1. Save the file(s).
2. Regenerate the client data (GetMainInfo screen: publish/encode, or run `Encoder.exe` in `Tools/MainInfo/`). This rewrites `Data/morph.pak` and `Data/Local/morpheus.emu` in the client folder set by `PathExport`.
3. Distribute the new client files to the players (launcher/auto-update, or a new client download).
4. Players restart the game.

You must repack whenever you:

- add or change a custom item, wing, pet, cape, jewel, monster model, map, image or effect;
- change any key in `MainInfo.ini` or `MainConnect.ini`;
- add, remove or reorder commands, shop items, event icons or translations;
- turn on a feature that has a client switch (Battle Pass, Morpheus Shop, MU Helper Offline, Custom Full Map, shortcut buttons...).

{% hint style="warning" %}
Most features need **both sides**: the server switch in `GameServerInfo - *.dat` (applied with `/reload`) and the client switch or data in MainInfo (applied with a repack). Turning on only one side gives a button that does nothing, or a server feature with no way to reach it.
{% endhint %}

## Server custom <-> client custom that must match

| Server file / key | Client file / key | What must match | Server page |
|---|---|---|---|
| `Data/Custom/CustomWing.txt` | `Local/CustomWing.txt` | Same rows: Type, Index and all option/damage columns. Identical column layout on both sides. The client uses it for the model and the tooltip, the server for the real stats. | [Custom Wing](../customs/custom-wing.md) |
| `Data/Custom/CustomJewel.txt` (section 0) | `Local/CustomJewel.txt` | One client row per server row. Server `ItemIndex` = client `Type * 512 + Index` (e.g. `7412` = `14 244`). Level/option limits and slot flags should be identical, otherwise the client lets you drop a jewel the server refuses (or hides a valid one). | [Custom Jewel](../customs/custom-jewel.md) |
| `Data/Custom/CustomMonster.txt` + `Data/Monster/Monster.txt` / `MonsterSetBase` | `Local/CustomMonster.txt` (+ `Local/CustomMonsterRenderMesh.txt`) | A **new** monster/NPC index (579 or higher) spawned by the server needs a client row with its model, or it is invisible. The two `CustomMonster.txt` files have different contents: the server one holds stats/rewards, the client one holds model, folder, size and name. | [Custom Monster](../customs/custom-monster.md) |
| `Data/Custom/CustomMorpheusShop.txt` + `CustomMorpheusShopSwitch` (`GameServerInfo - Custom.dat`) | `Local/CustomMorpheusShop.txt` + `[Custom] CustomMorpheusShopSwitch` / `EnableMorpheusShopCustomOptions` (`MainInfo.ini`) | The client file decides **which** items appear (and in which tab); the server file holds prices, option prices, limits, discounts and texts. Every item listed on the client needs a price row on the server. Both switches = 1. | [Morpheus Shop](../customs/morpheus-shop.md) |
| `Data/Custom/CustomBuyVip.txt` | `Local/CustomBuyVip.txt` | Same `Index` values with the same AccountLevel, days and coin prices. The client shows the list; the server charges and grants the VIP by `Index`. | [Buy VIP](../customs/buy-vip.md) |
| `Data/Event/EventName.dat` (sections 0 and 1) | `Local/EventNameDisplay.txt` (sections 0 and 1) | Same section and same index for each event/invasion. A mismatch shows one event's name/icon at another event's time. | [Event names](../events/event-name.md) |
| `Data/Command.txt` | `Local/CustomCommand.txt` | Same `Index` and the same command text. The client sends commands by this index (mobile vault switching uses index 10 `/bau` and 31 `/abrirbau`). | [Server features](../customs/server-features.md) |
| `Data/Command.txt`, `Data/Custom/CustomCommandDescription.txt`, `Data/Custom/CustomExchangeCoin.txt`, `Data/Custom/CustomMove.txt` | `Local/CustomShortcutButton.txt` + `[Custom] EnableMenuCustomShortcutCommand` | The `Command` text of each button must exist, spelled exactly, in one of those server tables. Unknown commands fail silently. | [Server features](../customs/server-features.md) |
| `Data/Item/ItemStack.txt` | `Local/CustomStack.txt` | Same item code, level, max stack and convert item. `-1` on the server = `*` on the client. | - |
| `CustomBattlePassSwitch` (`GameServerInfo - Custom.dat`) | `[Custom] CustomBattlePassSwitch` (+ `EnableMenuCustomBattlePass`) | Both = 1. With the client key at 0 the pass is hidden entirely. | [Battle Pass](../customs/battle-pass.md) |
| `HelperOfflineSwitch` (`GameServerInfo - Common.dat`) | `[HelperInfo] HelperActiveOfflineSystem` | Both = 1. The client key shows the "OFF HELPER" button; the server key makes it work. | [Server features](../customs/server-features.md) |
| `HelperSwitch` (`GameServerInfo - Common.dat`) | `[Interface] OficialMuHelper` | MU Helper window on the client, MU Helper allowed on the server. | [Server features](../customs/server-features.md) |
| `EnableCsSkillsAllMaps` (`GameServerInfo - Common.dat`) | `[Custom] EnableCsSkillsAllMaps` | Same value. | [Server features](../customs/server-features.md) |
| `CommandWareNumber_AL0..AL3` (`GameServerInfo - Command.dat`) | `[Interface] CommandWareNumber_AL0..AL3` | Same numbers. The client only draws vault buttons; the server accepts or refuses the switch. | [Server features](../customs/server-features.md) |
| `CustomRankUserSwitch` (`GameServerInfo - Custom.dat`, `Data/Custom/CustomRankUser.txt`) | `[Custom] RankUserSwitch` and `RankUser*` keys | Server sends the rank; client decides how to draw it (5 = off). | - |
| `CustomRankingSwitch` / `CustomEventTimeSwitch` (`GameServerInfo - Custom.dat`) | `[Custom] EnableRankingButton`, `EnableWindowsRankTop`, `EnableEventTimeButton`, `EnableWindowsEventTime` | Turn the window on in the client only if the server feature is on. | - |
| `SwitchBuyInventoryExt` (`GameServerInfo - Custom.dat`) | `[Interface] InventoryExtension`, `MaxInventoryExtension` | Inventory expansion UI on the client, purchase allowed on the server. | - |
| `CashShopSwitch` (`GameServerInfo - Common.dat`) | `[Interface] CashShopInGame` | Legacy cash shop window on both sides (ignored when the Morpheus Shop is on). | - |
| `Data/Custom/CustomWing.txt` / `Data/Item/Item.txt` items | `Local/CustomItem.txt`, `Local/CustomPet.txt`, `Local/CustomCape.txt`, render/effect files | Any **new** item index created on the server needs its model on the client, or it shows as an invisible/unknown item. | - |

{% hint style="danger" %}
Shipped files that do **not** match today (fix them before going live):
- `Data/Custom/CustomBuyVip.txt` has every row commented out, while `Local/CustomBuyVip.txt` lists 9 VIP packages. Players see the packages, but buying does nothing until you uncomment/add the same rows on the server.
- `CommandWareNumber_AL0` is `1` in `GameServerInfo - Command.dat` but `5` in `MainInfo.ini`: free accounts see vault buttons the server refuses.
- `Local/CustomJewel.txt` allows level 0+ for most jewels while the server file requires 9+ (6+ for Luck). The server wins; the client just shows the jewel as usable.
{% endhint %}

## Pitfalls

- Editing a client file and running `/reload`: nothing changes. Repack and redistribute.
- Repacking but not distributing: only your own client sees the change.
- Item codes: the client files use either two columns `Type Index` or a single item code `Type * 512 + Index` (e.g. `14 244` = `7412`). Check each file's column table.
- Keep the same file names and folders. The packer looks for exact paths (`Local/CustomWing.txt`, `Common/Effect/CustomEffectStatic.txt`, ...). A missing file just packs an empty list, silently.
- Some `Local/*.txt` files have a Portuguese or Vietnamese header; the column tables in [Custom files](custom-files.md) follow the order the packer actually reads.

## Reload module

There is no hot reload for client-side files. Change the file, repack with the GetMainInfo screen (or `Encoder.exe`), redistribute the client data, and have players restart. The server-side half of each pair is applied with the usual `/reload` group (for example `/reload custom` for `CustomWing.txt`, `CustomJewel.txt`, `CustomMonster.txt`, `CustomMorpheusShop.txt`, `CustomBuyVip.txt`; `/reload event` for `EventName.dat`; `/reload common` for `GameServerInfo - Common.dat`).
