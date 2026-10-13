# Game data (root Data folder)

The root `MuServer/Data/` folder holds the **game content** shared by every GameServer: items, monsters, spawns, drops, shops, warps, quests, skills, commands and more. This section documents every file in it, except `Custom/` (see [Customs](../customs/README.md)) and `Event/` (see [Events](../events/README.md)).

Before editing for the first time, read [File format basics](../file-format-basics.md): sections, `end`, `*`, comments and item indexes work the same way in almost every file.

{% hint style="info" %}
**Item index.** Many files write an item as one number: `Section × 512 + Type`. Jewel of Bless = section 14, type 13 → `7181`. Box of Kundun = 14/11 → `7179`.
{% endhint %}

## Pages in this section

| Page | Covers |
|---|---|
| [Items](items.md) | Item definitions, prices, trade fee, move/stack rules, VIP tickets, lucky items, jewel bank |
| [Item options](item-options.md) | Option odds (ItemOptionRate), option effects, 380, ancient sets, sockets, harmony, pet/earring/flag options |
| [Item drop](item-drop.md) | ItemDrop.txt and how a monster's drop is decided |
| [EventItemBag](event-item-bag.md) | Boxes, boss drops and event rewards (Normal and Advanced formats) |
| [Monsters and spawns](monsters.md) | Monster stats, MonsterSetBase (incl. Event/ and Invasion/), Kanturu spawns |
| [Monster AI and skills](monster-ai.md) | AI automata, elements, groups, rules, units; monster skills |
| [Maps](maps.md) | MapManager, MapServerInfo, terrain files |
| [Gates and warps](move.md) | Gate, Move, MoveSummon |
| [NPC shops](shops.md) | ShopManager and the shop files |
| [Cash Shop](cash-shop.md) | Cash Shop packages and products |
| [Quests](quests.md) | Classic quests and Quest World |
| [Skills](skills.md) | Skills, skill damage, Master Skill Tree |
| [Character base stats](character.md) | DefaultClassInfo |
| [Reset and experience](reset-and-experience.md) | ResetTable, MasterResetTable, ExperienceTable |
| [GMs, notices, filters, messages](util.md) | GameMaster, Notice, Filter, FilterRename, languages |
| [Chat commands](commands.md) | Command.txt, every command, reload types |
| [Effects and Mining](effects-and-mining.md) | Effect.txt, Mining.txt |
| [Anti-hack tables](anti-hack.md) | HackPacketCheck, HackSkillCheck, key files |

## Every file → page

### Root files

| File | Page | Reload |
|---|---|---|
| `Command.txt` | [Chat commands](commands.md) | `/reload command` |
| `Effect.txt` | [Effects and Mining](effects-and-mining.md) | `/reload common` |
| `EventItemBagManager.txt` | [EventItemBag](event-item-bag.md) | `/reload eventitembag` |
| `LangConfig.txt` | [Languages](../gameserver/languages.md) (summary in [util](util.md)) | `/reload lang` |
| `MapManager.txt` | [Maps](maps.md) | `/reload map` |
| `MapServerInfo.dat` | [Maps](maps.md) | `/reload map` |
| `Mining.txt` | [Effects and Mining](effects-and-mining.md) | `/reload common` |
| `ShopManager.txt` | [NPC shops](shops.md) | `/reload shop` |

### Folders

| Folder / file | Page | Reload |
|---|---|---|
| `CashShop/CashShopPackage.txt`, `CashShopProduct.txt` | [Cash Shop](cash-shop.md) | `/reload cashshop` |
| `Character/DefaultClassInfo.txt` | [Character base stats](character.md) | `/reload character` |
| `EventItemBag/NNN - Name.txt` | [EventItemBag](event-item-bag.md) | `/reload eventitembag` |
| `EventItemBag/_COMO-FUNCIONA.txt` | Reference manual (not loaded); translated in [EventItemBag](event-item-bag.md) | — |
| `Hack/HackPacketCheck.txt`, `HackSkillCheck.txt` | [Anti-hack tables](anti-hack.md) | `/reload hack` |
| `Hack/Dec1.dat`, `Enc2.dat` | [Anti-hack tables](anti-hack.md) — binary keys, do not edit | `/reload hack` |
| `Item/Item.txt` | [Items](items.md) | `/reload item` (restart for new items) |
| `Item/ItemValue.txt` | [Items](items.md) | `/reload shop` |
| `Item/ItemValueTrade.txt` | [Items](items.md) | `/reload custom` |
| `Item/ItemMove.txt`, `ItemStack.txt`, `ItemTicketVip.txt`, `LuckyItem.txt`, `JewelOfStore.txt` | [Items](items.md) | `/reload item` |
| `Item/ItemMakeColor.txt` | [Items](items.md) — legacy, not read | — |
| `Item/ItemOptionRate.txt`, `ItemOption.txt` | [Item options](item-options.md) | `/reload item` |
| `Item/ExcellentOptionRate.txt` | [Item options](item-options.md) — legacy, not read | — |
| `Item/380ItemOption.txt`, `380ItemType.txt` | [Item options](item-options.md) | `/reload item` |
| `Item/SetItemOption.txt`, `SetItemType.txt` | [Item options](item-options.md) | `/reload item` |
| `Item/SocketItemOption.txt`, `SocketItemType.txt` | [Item options](item-options.md) | `/reload item` |
| `Item/JewelOfHarmonyOption.txt`, `JewelOfHarmonyType.txt` | [Item options](item-options.md) | `/reload item` |
| `Item/PetItemOption.txt`, `EarringItemOption.txt`, `FlagItemOption.txt` | [Item options](item-options.md) | `/reload item` |
| `Item/ItemDrop.txt` | [Item drop](item-drop.md) | `/reload item` |
| `Lang/NNN - Xxx_Message.txt` | [Languages](../gameserver/languages.md) (summary in [util](util.md)) | `/reload lang` |
| `Monster/Monster.txt` | [Monsters and spawns](monsters.md) | `/reload monster` |
| `Monster/KanturuMonsterSetBase.txt` | [Monsters and spawns](monsters.md) | `/reload monster` |
| `Monster/MovePath/MovePath_35.dat` | [Monsters and spawns](monsters.md) | `/reload monster` |
| `Monster/MonsterAIAutomata.txt`, `MonsterAIElement.txt`, `MonsterAIGroup.txt`, `MonsterAIRule.txt`, `MonsterAIUnit.txt` | [Monster AI and skills](monster-ai.md) | `/reload monster` |
| `Monster/MonsterSkill.txt`, `MonsterSkillElement.txt`, `MonsterSkillUnit.txt` | [Monster AI and skills](monster-ai.md) | `/reload monster` |
| `MonsterSetBase/NNN - Map.txt` | [Monsters and spawns](monsters.md) | `/reload monster` |
| `MonsterSetBase/Event/NNN - Map.txt` | [Monsters and spawns](monsters.md) | `/reload monster` |
| `MonsterSetBase/Invasion/NNN - Map.txt` | [Monsters and spawns](monsters.md) | `/reload monster` |
| `Move/Gate.txt`, `Move.txt`, `MoveSummon.txt` | [Gates and warps](move.md) | `/reload move` |
| `Quest/Quest.txt`, `QuestObjective.txt`, `QuestReward.txt` | [Quests](quests.md) | `/reload quest` |
| `QuestWorld/QuestWorld.txt`, `QuestWorldObjective.txt`, `QuestWorldReward.txt` | [Quests](quests.md) | `/reload quest` |
| `Shop/NNN - Name.txt` | [NPC shops](shops.md) | `/reload shop` |
| `Skill/Skill.txt`, `SkillDamage.txt`, `MasterSkillTree.txt` | [Skills](skills.md) | `/reload skill` |
| `Skill/SkillElect.hit` | [Skills](skills.md) — binary, do not edit | `/reload skill` |
| `Skill/Skill_eng.txt`, `Skill_chs.txt` | [Skills](skills.md) — translations, not read | — |
| `Terrain/*.att` | [Maps](maps.md) — binary, do not edit | `/reload terrain` |
| `Util/ExperienceTable.txt`, `ResetTable.txt`, `MasterResetTable.txt` | [Reset and experience](reset-and-experience.md) | `/reload util` |
| `Util/GameMaster.txt`, `Notice.txt`, `Filter.txt`, `FilterRename.txt` | [GMs, notices, filters](util.md) | `/reload util` |

### Files the GameServer does not read

| File / folder | Note |
|---|---|
| `Skin/skinlist.txt`, `Skin/skinshop.txt` | Not loaded by this GameServer version. |
| `BMD/` | Output folder of the GameServer's export tools (empty by default). |
| `*.bak-*` files and `Lang.bak-*` folders | Backups. Never loaded. |
| Files not named `NNN - Name.txt` inside `EventItemBag/`, `MonsterSetBase/` or `Shop/` (e.g. `_131 - ...`, `teste000 - ...`) | Ignored by the loader. |

## Applying changes

Every page ends with its **Reload module**. Type `/reload <module>` in game with an Admin character, use the GameServer window's Reload menu, or the panel's hot reload — see [Services and reload](../../dashboard/services-and-reload.md). The full list of modules is in [Chat commands](commands.md#reload-types).

{% hint style="warning" %}
`/reload all` reloads everything **except** the item tables and the terrain. After adding items to `Item.txt`, restart the GameServer.
{% endhint %}

## Visual editors in the panel

| Editor | Files |
|---|---|
| [EventItemBag editor](../../dashboard/editors/event-item-bag.md) | `EventItemBagManager.txt`, `EventItemBag/` |
| [NPC Shop editor](../../dashboard/editors/npc-shop.md) | `ShopManager.txt`, `Shop/` |
| [Spot editor](../../dashboard/editors/spot.md) | `MonsterSetBase/` (uses `Terrain/` to show walkable tiles) |
| [Cash Shop editor](../../dashboard/editors/cash-shop.md) | `CashShop/` and the client shop scripts |

Every other file opens in the [File editor](../../dashboard/file-editor.md).
