# Client-side customs (Client Hub)

What the client-side data is, how it reaches the players through the **Client Hub**, and which server files it must stay in sync with.

## What it is

Some features need data **on the player's device**, not only on the server: custom item models, wing stats shown in the tooltip, jewel rules for drag-and-drop, the in-game shop catalogue, the event icons, the command window, the menu buttons, and so on. All of that is managed in the **Client Hub** of the Morpheus control panel, in [Client Hub → Client settings](../../dashboard/client-hub/client-settings.md), and reaches the players when you [publish an update from the Client Hub](../../dashboard/client-hub/publishing-updates.md).

| What | What it holds | Page |
|---|---|---|
| Client settings | On/off switches and options for the client (menus, interface, helper, Discord, login buttons...) | [Client settings reference](client-settings-reference.md) |
| Connection settings | Connection and window settings (server address, window title...) | [Client settings reference](client-settings-reference.md) |
| Client custom files (`*.txt`) | Custom items, wings, jewels, monsters, maps, commands, shop list, event icons, translations | [Client custom files](custom-files.md) |
| Client effect files (`*.txt`) | Glow/particle effects attached to items and pets (the copy that actually reaches the players) | [Effects and item manager files](effects-and-item-manager.md) |
| Client item manager files (`*.txt`) | Reference files for the item tooling. **Not used** by the client | [Effects and item manager files](effects-and-item-manager.md) |

{% hint style="info" %}
Everything above is edited in **Client Hub → Client settings**: the settings in their tabs, the client custom and effect files in the **Custom loaders** tab. Saving there and publishing the update is all you need to do.
{% endhint %}

## When you must publish a client update

Client-side data is **never** hot-reloaded. `/reload` only reloads server files. Any change to client settings, connection settings or client custom/effect files reaches the players only after you:

1. Save the change in **Client Hub → Client settings**.
2. Click **Publish update** there, then publish it from **Publish updates** (see [Publishing updates](../../dashboard/client-hub/publishing-updates.md)).
3. Players receive the update through the launcher/auto-update (or a new client download).
4. Players restart the game.

You must publish a client update whenever you:

- add or change a custom item, wing, pet, cape, jewel, monster model, map, image or effect;
- change any client setting or connection setting;
- add, remove or reorder commands, shop items, event icons or translations;
- turn on a feature that has a client switch (Battle Pass, Morpheus Shop, MU Helper Offline, Custom Full Map, shortcut buttons...).

{% hint style="warning" %}
Most features need **both sides**: the server switch in `GameServerInfo - *.dat` (applied with `/reload`) and the client switch or data in the Client Hub (applied by publishing a client update). Turning on only one side gives a button that does nothing, or a server feature with no way to reach it.
{% endhint %}

## Server custom <-> client custom that must match

| Server file / key | Client file / key | What must match | Server page |
|---|---|---|---|
| `Data/Custom/CustomWing.txt` | `CustomWing.txt` | Same rows: Type, Index and all option/damage columns. Identical column layout on both sides. The client uses it for the model and the tooltip, the server for the real stats. | [Custom Wing](../customs/custom-wing.md) |
| `Data/Custom/CustomJewel.txt` (section 0) | `CustomJewel.txt` | One client row per server row. Server `ItemIndex` = client `Type * 512 + Index` (e.g. `7412` = `14 244`). Level/option limits and slot flags should be identical, otherwise the client lets you drop a jewel the server refuses (or hides a valid one). | [Custom Jewel](../customs/custom-jewel.md) |
| `Data/Custom/CustomMonster.txt` + `Data/Monster/Monster.txt` / `MonsterSetBase` | `CustomMonster.txt` (+ `CustomMonsterRenderMesh.txt`) | A **new** monster/NPC index (579 or higher) spawned by the server needs a client row with its model, or it is invisible. The two `CustomMonster.txt` files have different contents: the server one holds stats/rewards, the client one holds model, folder, size and name. | [Custom Monster](../customs/custom-monster.md) |
| `Data/Custom/CustomMorpheusShop.txt` + `CustomMorpheusShopSwitch` (`GameServerInfo - Custom.dat`) | `CustomMorpheusShop.txt` + `[Custom] CustomMorpheusShopSwitch` / `EnableMorpheusShopCustomOptions` (client settings) | The client file decides **which** items appear (and in which tab); the server file holds prices, option prices, limits, discounts and texts. Every item listed on the client needs a price row on the server. Both switches = 1. | [Morpheus Shop](../customs/morpheus-shop.md) |
| `Data/Custom/CustomBuyVip.txt` | `CustomBuyVip.txt` | Same `Index` values with the same AccountLevel, days and coin prices. The client shows the list; the server charges and grants the VIP by `Index`. | [Buy VIP](../customs/buy-vip.md) |
| `Data/Event/EventName.dat` (sections 0 and 1) | `EventNameDisplay.txt` (sections 0 and 1) | Same section and same index for each event/invasion. A mismatch shows one event's name/icon at another event's time. | [Event names](../events/event-name.md) |
| `Data/Command.txt` | `CustomCommand.txt` | Same `Index` and the same command text. The client sends commands by this index (mobile vault switching uses index 10 `/bau` and 31 `/abrirbau`). | [Server features](../customs/server-features.md) |
| `Data/Command.txt`, `Data/Custom/CustomCommandDescription.txt`, `Data/Custom/CustomExchangeCoin.txt`, `Data/Custom/CustomMove.txt` | `CustomShortcutButton.txt` + `[Custom] EnableMenuCustomShortcutCommand` | The `Command` text of each button must exist, spelled exactly, in one of those server tables. Unknown commands fail silently. | [Server features](../customs/server-features.md) |
| `Data/Item/ItemStack.txt` | `CustomStack.txt` | Same item code, level, max stack and convert item. `-1` on the server = `*` on the client. | - |
| `CustomBattlePassSwitch` (`GameServerInfo - Custom.dat`) | `[Custom] CustomBattlePassSwitch` (+ `EnableMenuCustomBattlePass`) | Both = 1. With the client key at 0 the pass is hidden entirely. | [Battle Pass](../customs/battle-pass.md) |
| `HelperOfflineSwitch` (`GameServerInfo - Common.dat`) | `[HelperInfo] HelperActiveOfflineSystem` | Both = 1. The client key shows the "OFF HELPER" button; the server key makes it work. | [Server features](../customs/server-features.md) |
| `HelperSwitch` (`GameServerInfo - Common.dat`) | `[Interface] OficialMuHelper` | MU Helper window on the client, MU Helper allowed on the server. | [Server features](../customs/server-features.md) |
| `EnableCsSkillsAllMaps` (`GameServerInfo - Common.dat`) | `[Custom] EnableCsSkillsAllMaps` | Same value. | [Server features](../customs/server-features.md) |
| `CommandWareNumber_AL0..AL3` (`GameServerInfo - Command.dat`) | `[Interface] CommandWareNumber_AL0..AL3` | Same numbers. The client only draws vault buttons; the server accepts or refuses the switch. | [Server features](../customs/server-features.md) |
| `CustomRankUserSwitch` (`GameServerInfo - Custom.dat`, `Data/Custom/CustomRankUser.txt`) | `[Custom] RankUserSwitch` and `RankUser*` keys | Server sends the rank; client decides how to draw it (5 = off). | - |
| `CustomRankingSwitch` / `CustomEventTimeSwitch` (`GameServerInfo - Custom.dat`) | `[Custom] EnableRankingButton`, `EnableWindowsRankTop`, `EnableEventTimeButton`, `EnableWindowsEventTime` | Turn the window on in the client only if the server feature is on. | - |
| `SwitchBuyInventoryExt` (`GameServerInfo - Custom.dat`) | `[Interface] InventoryExtension`, `MaxInventoryExtension` | Inventory expansion UI on the client, purchase allowed on the server. | - |
| `CashShopSwitch` (`GameServerInfo - Common.dat`) | `[Interface] CashShopInGame` | Legacy cash shop window on both sides (ignored when the Morpheus Shop is on). | - |
| `Data/Custom/CustomWing.txt` / `Data/Item/Item.txt` items | `CustomItem.txt`, `CustomPet.txt`, `CustomCape.txt`, render/effect files | Any **new** item index created on the server needs its model on the client, or it shows as an invisible/unknown item. | - |

{% hint style="danger" %}
Shipped files that do **not** match today (fix them before going live):
- `Data/Custom/CustomBuyVip.txt` has every row commented out, while the client custom file `CustomBuyVip.txt` lists 9 VIP packages. Players see the packages, but buying does nothing until you uncomment/add the same rows on the server.
- `CommandWareNumber_AL0` is `1` in `GameServerInfo - Command.dat` but `5` in the client settings: free accounts see vault buttons the server refuses.
- The client custom file `CustomJewel.txt` allows level 0+ for most jewels while the server file requires 9+ (6+ for Luck). The server wins; the client just shows the jewel as usable.
{% endhint %}

## Pitfalls

- Editing a client file and running `/reload`: nothing changes. Publish a client update from the Client Hub.
- Saving in the Client Hub but not publishing: players do not see the change.
- Item codes: the client files use either two columns `Type Index` or a single item code `Type * 512 + Index` (e.g. `14 244` = `7412`). Check each file's column table.
- Keep the same file names. The client data is built from exact file names (`CustomWing.txt`, `CustomEffectStatic.txt`, ...). A missing file just publishes an empty list, silently.
- Some client custom files have a Portuguese or Vietnamese header; the column tables in [Client custom files](custom-files.md) follow the order the columns are actually read.

## Reload module

There is no hot reload for client-side data. Change it in **Client Hub → Client settings**, [publish the update to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md), and have players restart. The server-side half of each pair is applied with the usual `/reload` group (for example `/reload custom` for `CustomWing.txt`, `CustomJewel.txt`, `CustomMonster.txt`, `CustomMorpheusShop.txt`, `CustomBuyVip.txt`; `/reload event` for `EventName.dat`; `/reload common` for `GameServerInfo - Common.dat`).
