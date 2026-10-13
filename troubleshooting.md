# Troubleshooting

Common problems and how to fix them. Before anything else, check the **GameServer log** in the control panel: most configuration errors are reported there with the file name and the line.

{% hint style="info" %}
If a change made things worse, you can always go back. Every save in the panel keeps the previous version of the file, and folder backups let you restore only the files you choose. See the [Control panel guide](dashboard/README.md).
{% endhint %}

## Configuration files

### A file stops loading after I edited it

**Symptom:** after an edit, the system stops working, the GameServer log shows an error for that file, or the GameServer closes at startup.

**Cause:** most `.txt` files in `Data/` are split into numbered sections, and every section must be closed with a line containing only `end`. A missing `end`, a deleted section number or an extra quote makes the GameServer read everything after it out of place.

**Fix:**

1. Compare the file with the previous version (the panel keeps one) or with a backup.
2. Make sure each section starts with its number and finishes with `end`:

```
0
//Index  Value
1        100
end

1
//Index  Value
1        200
end
```

3. Make sure text values are inside double quotes, for example `"Box of Luck"`.

### Two files with the same number prefix

**Symptom:** a box, a season or a language file seems to be ignored, or shows the content of another file.

**Cause:** in folders such as `Data/EventItemBag/`, `Data/Custom/BattlePass/` and `Data/Lang/`, the `NNN` at the start of the file name (for example `003 - Star of Christmas.txt`) is the index the server uses. Two files with the same `NNN` collide, and only one of them is used.

**Fix:** give each file a unique prefix. When you duplicate a file to create a new one, rename it to the next free number. The [EventItemBag editor](dashboard/editors/event-item-bag.md) and the [Battle Pass editor](dashboard/editors/battle-pass.md) allocate the numbers for you.

### Accents and special characters show as strange symbols

**Symptom:** names and messages show `Ã©`, `?` or boxes instead of accented letters.

**Cause:** MU configuration files use the Windows-1252 encoding with Windows line endings. Saving them as UTF-8 in an external editor breaks every accented character.

**Fix:** edit the files in the control panel's file editor, which reads and writes them in the correct format. If you must use an external editor, save with "Western (Windows 1252)" encoding. Restore the previous version if the file was already damaged.

### The panel refuses to save: the file was changed by someone else

**Cause:** another person (or another tab) saved the same file after you opened it. The panel blocks your save so their work is not erased.

**Fix:** copy your changes, reload the file, and apply them again on top of the newest version.

### I cannot edit some files

**Cause:** executables and libraries can never be edited, and DataServer and JoinServer are read-only by default. Operator accounts cannot edit files at all.

**Fix:** sign in with the server owner's account. See [Signing in](dashboard/signing-in.md).

## Applying changes

### I reloaded but nothing changed

**Causes and fixes:**

| Cause | Fix |
|---|---|
| The wrong module was reloaded | Reload the module that matches the file: Item, Monster, Cash Shop, Server List, custom, language or EventItemBag |
| The setting is read only at startup | Restart the GameServer from the panel |
| The file has an error, so the server kept the old values | Check the GameServer log and fix the file |
| The change is on the game client side | Publish it in the [Client Hub](dashboard/client-hub/README.md); players receive it when they open the game |
| The text was changed but not the language files | After editing messages, also reload the language module |

### A restart fails with "port already in use"

**Cause:** the previous process had not yet released its network port.

**Fix:** restart from the panel, which waits for the port to be released before starting again. If you start a program by hand, wait about a minute after stopping it.

### Players do not see my client changes

**Cause:** changes to the server list screen, game texts, client items, client settings or game panels sit in the Client Hub's pending queue until you publish them.

**Fix:** open the [Client Hub](dashboard/client-hub/README.md), review the pending list and click **Publish**. Players get the update the next time they open the game.

## Connection

### Players cannot connect to the server

Check, in this order:

1. **All services are running** on the panel dashboard: ConnectServer, DataServer, JoinServer and GameServer.
2. **The server list.** `ConnectServer/Data/ServerList.dat` must contain the GameServer with the correct code, address and port, and `ConnectServer/Data/ConnectServer.ini` must have the right settings.
3. **The firewall.** The ConnectServer and GameServer ports must be open for incoming connections on the machine and on any router or hosting firewall.
4. **The client's server list**, edited in the [Client Hub](dashboard/client-hub/README.md), must point to your ConnectServer. Publish it after any change.

### The server appears in the list but entering fails

**Cause:** the GameServer entry in `ServerList.dat` does not match the GameServer itself (server code, address or port), or the GameServer is still starting.

**Fix:** compare the GameServer's code and port with the line in `ServerList.dat`, fix it, reload Server List in the panel and try again after the GameServer finishes starting.

### Players are disconnected right after logging in

**Cause:** the DataServer or JoinServer is down, or the account is already marked as online.

**Fix:** check that DataServer and JoinServer are running. If they restarted while players were online, wait a moment and try again.

### Party voice does not work

**Fix:** make sure party voice is enabled in [GameServerInfo - Common](muserver/gameserver/common.md) and that the VoiceServer is running. Players must be in the same party to talk.

## Shops

### An item disappeared from the Morpheus Shop

**Possible causes:**

- The item has no price for any duration and currency.
- Another item's range now covers it. Use **separate this item** in the [Morpheus Shop editor](dashboard/editors/morpheus-shop.md) to give it its own entry.
- The list of items changed on the server but was not published to the client.

**Fix:** open the [Morpheus Shop editor](dashboard/editors/morpheus-shop.md). Its status indicator lists items without a price. If you changed only prices, reload the custom module; if you changed the item list, publish to the client as well. See [Morpheus Shop](muserver/customs/morpheus-shop.md).

### The Morpheus Shop or Battle Pass button does not appear in the game

**Cause:** these systems have two switches: one on the server (`GameServer/Data/GameServerInfo - Custom.dat`) and one on the client. With only one of them on, the button does not appear.

**Fix:** turn on both switches, then reload or restart the GameServer and publish the client change. The editors show both switches in their status indicator. See [Morpheus Shop](muserver/customs/morpheus-shop.md) and [Battle Pass](muserver/customs/battle-pass.md).

### The GameServer closes at startup after editing the Battle Pass, even with it switched off

**Cause:** the Battle Pass files are always read at startup so that errors appear in the log, even when the system is off. A malformed season file stops the GameServer.

**Fix:** check the GameServer log for the file and line, fix it or restore the previous version. Using the [Battle Pass editor](dashboard/editors/battle-pass.md) avoids this, because it validates the files with the same rules the GameServer uses.

### Cash Shop items look different in the game than on the server

**Cause:** the Cash Shop has a server part and a client part, and they are out of sync.

**Fix:** use the [Cash Shop editor](dashboard/editors/cash-shop.md), which saves both together and shows what was out of sync. Reload Cash Shop and publish to the client.

## Drops and boxes

### Zen never drops from a box (EventItemBag)

**Cause:** each drop is one independent draw. If the draw fails `ItemDropRate`, the box gives Zen; if it passes, it gives an item. With `ItemDropRate` set to `100`, the draw never fails, so Zen can never drop.

**Fix:** lower `ItemDropRate`. For example, `80` gives an item 80% of the time and Zen 20% of the time. See the [EventItemBag editor](dashboard/editors/event-item-bag.md).

### Ancient items never drop from a box

**Cause:** `SetItemDropRate` is `0`.

**Fix:** set `SetItemDropRate` to the chance you want for an ancient item instead of an item from the list.

### A box or boss gives nothing, or gives the prizes of another box

**Cause:** the trigger line in `Data/EventItemBagManager.txt` points to the wrong bag, or another line with the same item, monster or event is matched first and shadows it.

**Fix:** open the **Trigger** tab of the [EventItemBag editor](dashboard/editors/event-item-bag.md), which warns about shadowed triggers. Use the **Chances** tab to see the real chance of each item per class.

## Events

### An event does not start

Check:

1. **The switch** for that event in `GameServer/Data/GameServerInfo - Event.dat` is `1`.
2. **The schedule** in `Data/Event/<Event>.dat`: lines starting with `//` are comments and are ignored. Remove the `//` from the time you want to use.
3. **The server clock.** Times use the clock of the machine where the GameServer runs.
4. **Minimum players.** Some events, such as Kill All, cancel when fewer than the minimum number of players enter.

You can also start the event right away from the panel, or with its GM command, to confirm it works. See the page of each event under [Events](CHANGELOG.md#events).

### The Morpheus Tournament does not open

**Cause:** the tournament needs two switches in `GameServerInfo - Event.dat`: `SwitchMorpheusTournamentSystem` (the system) and `EnableColiseumTournament` (the one-on-one mode). Both must be `1`.

**Fix:** set both to `1` and complete the arena, NPC, schedule and prizes in `Data/Event/MorpheusTournament.dat`. See [Morpheus Tournament](muserver/events/morpheus-tournament.md).

## Players and gameplay

### A command does not work

**Possible causes:** the system behind it is switched off, the player does not meet the requirements (level, reset, Zen, VIP level), or the command needs GM permission.

**Fix:** check the command's switch and requirements. For example, `/pick` is controlled by its row in `Command.txt` and the `CustomPick` column of `MapManager.txt` (the `CustomPick*` keys in Custom.dat are not read). See [Pick](muserver/customs/pick.md) and [Server features](muserver/customs/server-features.md).

### Offline helper or offline attack stops after a while

**Cause:** offline time is limited per account level (normal and each VIP tier).

**Fix:** adjust the offline time for each account level. Offline MU Helper is in [GameServerInfo - Common](muserver/gameserver/common.md); offline attack and offline store are on [Server features](muserver/customs/server-features.md).

### Server messages appear in the wrong language

**Cause:** each player receives messages in the language they chose in the game. The default language applies to players who never chose one.

**Fix:** set the default language in [GameServerInfo - Common](muserver/gameserver/common.md). If one specific message is wrong or missing in a language, that text is missing from that language's file in `Data/Lang/` (for example `001 - Por_Message.txt`). Add it with the same index used in the other language files and reload the language module.

### Monster damage changed after enabling Balance Class

**Cause:** Balance Class only changes PvP damage, but the life option (`CustomBalanceClassLifeSwitch`) calculates life and shield from vitality, which also affects fights against monsters.

**Fix:** turn the life option off if you want PvM untouched. See [Balance Class](muserver/customs/balance-class.md).

### Players cannot buy inventory extension

**Fix:** set `SwitchBuyInventoryExt = 1` and the price of each extension in `GameServerInfo - Custom.dat`. See [Server features](muserver/customs/server-features.md).

### The game crashes on a player's phone

**Fix:** open the crash reports in the control panel. Each report shows the device model, the game version and the technical detail of the failure, so you do not need the player's phone. Update failures are listed separately, to tell whether a specific file is blocking players from getting in.
