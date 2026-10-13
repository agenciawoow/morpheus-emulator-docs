# Customs

Morpheus has more than forty systems that the original MU never had. This section shows how to configure each one.

## Where customs live

| Layer | Location | Notes |
|---|---|---|
| On/off switches and texts | `GameServer/Data/GameServerInfo - Custom.dat` | A few live in `- Common.dat` |
| Data tables | `Data/Custom/*.txt` (and `CustomBalanceClass.dat`) | One file per system |
| Client side | `Tools/MainInfo/Local/*.txt` | Some customs need a matching client file. See [Client customs](../client/README.md) |

{% hint style="warning" %}
Some customs have **two halves**: a server file and a client file. When you add a wing, jewel, monster, shop item or VIP plan, change both and redistribute the client. Otherwise players see the wrong item or nothing at all. The [Client customs](../client/README.md) page lists every matching pair.
{% endhint %}

## Customs with their own file

| Custom | File | Enable key (default) | Command | Client file to match | Page |
|---|---|---|---|---|---|
| Custom Arena | `CustomArena.txt` | `CustomArenaSwitch = 0` | `/startarena <index>` | none | [Custom Arena](custom-arena.md) |
| PvP balance (single damage formula) | `CustomBalanceClass.dat` | `CustomBalanceClassSwitch = 1` | none | none | [Balance Class](balance-class.md) |
| Battle Pass | `CustomBattlePass.txt` + `BattlePass/NNN - <season>.txt` | `CustomBattlePassSwitch = 1` | none (button) | `MainInfo.ini` `CustomBattlePassSwitch` | [Battle Pass](battle-pass.md) |
| Buy VIP | `CustomBuyVip.txt` | none | none (window) | `Local/CustomBuyVip.txt` | [Buy VIP](buy-vip.md) |
| Skill combos | `CustomCombo.txt` | `EnableComboToAllSwitch = 1` (Common) | none | none | [Combo](combo.md) |
| Command descriptions | `CustomCommandDescription.txt` | none | none | see page | [Command Description](command-description.md) |
| Death messages | `CustomDeathMessage.txt` | `CustomDeathMessageSwitch = 1` | none | none | [Death Message](death-message.md) |
| Event Drop (item rain) | `CustomEventDrop.txt` | `CustomEventDropSwitch = 1` | `/startdrop` | none | [Event Drop](event-drop.md) |
| Coin exchange | `CustomExchangeCoin.txt` | none | none | none | [Exchange Coin](exchange-coin.md) |
| Custom jewels | `CustomJewel.txt` | none | none | `Local/CustomJewel.txt` | [Custom Jewel](custom-jewel.md) |
| Custom Chaos Machine mixes | `CustomMix.txt` | none | none | none | [Custom Mix](custom-mix.md) |
| Mix expansion (Goblin NPC) | `MixExpansion.txt` | Section 0 of `MixExpansion.txt` (the `GoblinMixNpc*` keys are not read) | none | none | [Mix Expansion](mix-expansion.md) |
| Custom monsters | `CustomMonster.txt` | none | none | `Local/CustomMonster.txt`, `CustomMonsterRenderMesh.txt` | [Custom Monster](custom-monster.md) |
| Morpheus Shop | `CustomMorpheusShop.txt` | `CustomMorpheusShopSwitch = 1` | none (window) | `Local/CustomMorpheusShop.txt` | [Morpheus Shop](morpheus-shop.md) |
| Custom /move list | `CustomMove.txt` | none | `/move` | none | [Custom Move](custom-move.md) |
| Collector, command, move and quest NPCs | `CustomNpcCollector.txt`, `CustomNpcCommand.txt`, `CustomNpcMove.txt`, `CustomNpcQuest.txt` | `CustomNpcCollectorSwitch = 0`, `CustomNpcQuestSwitch = 1` | none | none | [NPC Customs](npc-customs.md) |
| Online Lottery | `CustomOnlineLottery.txt` | `CustomOnlineLotterySwitch = 1` | none | none | [Online Lottery](online-lottery.md) |
| Auto pick | `CustomPick.txt` | `/pick` row in `Command.txt` + `CustomPick` column in `MapManager.txt` | `/pick <type>` | none | [Pick](pick.md) |
| Custom quests | `CustomQuest.txt` | `CustomQuestSwitch = 1` | `/quest` | none | [Custom Quest](custom-quest.md) |
| Quiz | `CustomQuiz.txt` | `CustomQuizSwitch = 1` | `/startquiz`, `/resp` | none | [Quiz](quiz.md) |
| Rank titles by reset/level | `CustomRankUser.txt` | `CustomRankUserSwitch = 1` | none | none | [Rank User](rank-user.md) |
| Ranking window (F8) | `CustomRanking.txt` | `CustomRankingSwitch = 1` | none | none | [Ranking](ranking.md) |
| Starting items per class | `CustomStartItem.txt` | none | none | none | [Start Item](start-item.md) |
| Top lists | `CustomTop.txt` | none | `/top <type>` | none | [Custom Top](custom-top.md) |
| Custom wings and wing mix | `CustomWing.txt`, `CustomWingMix.txt` | none | none | `Local/CustomWing.txt` | [Custom Wing](custom-wing.md) |
| Buffer bot | `BotSystem/BotBuffer.txt` | none | `/reload bots` | none | [Bot Buffer](bot-buffer.md) |

All files above are in `Data/Custom/`. Enable keys are in `GameServerInfo - Custom.dat` unless marked (Common).

## Customs with no data file

These are set entirely with keys in `GameServerInfo - Custom.dat` or `- Common.dat`. All of them are on one page: [Server features](server-features.md).

| Feature | File | Main key (default) |
|---|---|---|
| Announce Chaos Mix | Custom.dat | `AnnounceChaosMix = 1` |
| Custom Store / offline store (`/store`, `/offstore`) | Custom.dat | `CustomStore*` |
| Custom Attack / offline attack (`/attack`, `/offattack`) | Custom.dat | `CustomAttack*` |
| Event Time window (H key) | Custom.dat | `CustomEventTimeSwitch = 1` |
| Guild Warehouse | Custom.dat | `GuildWarehouseEnable = 1` |
| Shop MessageBox | Custom.dat | `CustomShopMessageBox = 0` |
| Buy Inventory Extension | Custom.dat | `SwitchBuyInventoryExt = 1` |
| Chat Anti-Spam | Custom.dat | `EnableAntiSpam = 1` |
| MU Helper Offline | Common.dat | `HelperOfflineSwitch = 1` |
| Party Voice | Common.dat | `PartyVoiceEnabled = 1` |
| Reward Online Users | Common.dat | `OnlineReward*` |
| Combo for all classes | Common.dat | `EnableComboToAllSwitch = 1` |
| HP Bonus by master reset | Common.dat | `HpBonus_AL*` |
| Remove skill knockback | Common.dat | `Disable*Effect` |
| Transformation Ring | Common.dat | `TransformationRing1..6` |
| Infinity Arrow | Common.dat | `InfinityArrow_AL*` |

## Enable it

1. Set the switch in `GameServerInfo - Custom.dat` (or `- Common.dat`).
2. Edit the data file in `Data/Custom/`.
3. If the custom has a client half, update the matching file in `Tools/MainInfo/Local/`, repack the client and redistribute it.
4. Apply the change with `/reload custom` (or `/reload common`), or restart the GameServer.

## Pitfalls

* **Some files load even when their switch is off.** That includes Morpheus Shop, Balance Class and Battle Pass. Their errors show in the log either way. A malformed Battle Pass file can stop the GameServer even with `CustomBattlePassSwitch = 0`.
* **Customs that run as events** (Custom Arena, Event Drop, Online Lottery, Quiz) read their file on `/reload custom`. Their schedule and switch are only re-applied on `/reload event`. See [Schedule syntax](../events/schedule-syntax.md#reload-module).
* Some messages, such as the Announce Chaos Mix texts and the Battle Pass texts, come from `Data/Lang` in each player's language. For those, the matching text in `GameServerInfo - Custom.dat` is only used when the Lang entry is missing.

## Reload module

| Command | Reloads |
|---|---|
| `/reload custom` | `GameServerInfo - Custom.dat` and every `Data/Custom/*.txt` / `.dat` listed above, including the active Battle Pass season |
| `/reload common` | `GameServerInfo - Common.dat` (MU Helper Offline, HP Bonus, Reward Online, and more) |
| `/reload move` | `Data/Custom/CustomMove.txt` together with `Move/Gate.txt`, `Move/Move.txt`, `Move/MoveSummon.txt` |
| `/reload bots` | `Data/Custom/BotSystem/BotBuffer.txt` and re-creates the bots |
| `/reload event` | Recalculates the schedules of Custom Arena, Event Drop, Online Lottery and Quiz |

`/reload` is an admin command (`#GameMaster = 2`, index 35 in `Data/Command.txt`).
