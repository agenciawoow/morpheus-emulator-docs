# GameServerInfo - Custom

## What it does

The on/off switches, limits and texts of the Morpheus custom systems. Most customs also have a data file in `Data/Custom/`; this file only turns them on and sets the few values that are not in their data file. Each block below links to the custom's own page, where the data file is explained.

## Where it lives

- `GameServer/Data/GameServerInfo - Custom.dat`
- `GameServerCS/Data/GameServerInfo - Custom.dat` (separate copy)

INI format, section `[GameServerInfo]`. `_AL0..3` = Free, VIP 1, VIP 2, VIP 3. See [Customs](../customs/README.md) for the list of every custom and its files.

## Reload

`/reload custom` re-reads this file and every file in `Data/Custom/`.

{% hint style="warning" %}
Text keys in this file (announcements, shop names) are sent exactly as written, in one language. Several systems now take their texts from `Data/Lang/` instead, translated per player (see each block). Do not put `;` comments after a text value: they become part of the text.
{% endhint %}

---

## Announce Chaos Mix Settings

Announces to the whole server when a player succeeds or fails a +level Chaos Machine mix. Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `AnnounceChaosMix` | 0 | 0, 1 | `1` = announcements on. |
| `AnnounceChaosMixLevel` | 5 | 0 to 6 | How many mix levels are announced, counting from +10: mixes to +10 up to +(9 + value). `5` = +10 to +14 (shipped), `6` = +10 to +15, `0` = none. |
| `AnnounceChaosMixSuccess` | built-in English text | text with `%s %s %d` | Fallback success text (player, item, level). Used only if text 946 is missing from the player's language file. |
| `AnnounceChaosMixFailure` | built-in English text | text with `%s %s %d` | Fallback failure text. Used only if text 947 is missing from the language file. |

## Custom Arena Settings

PvP arenas with scoring, defined in `Data/Custom/CustomArena.txt`. Page: [Custom Arena](../customs/custom-arena.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomArenaSwitch` | 0 | 0, 1 | `1` = arenas on. |
| `CustomArenaVictimScoreDecrease` | 0 | points | Points lost when killed. |
| `CustomArenaKillerScoreIncrease` | 0 | points | Points won per kill. |
| `CustomArenaText1` to `CustomArenaText19` | (empty) | text | Announcements and error messages: closing countdown, closed, opening countdown, started, finished, winner, your position, cannot enter, not open, full, level/master level/reset/master reset limits, class not allowed. Keep the `%s` and `%d` of each text in the same order. |
| `CustomArenaText20` | built-in text | text | Shown when the reward item cannot be delivered because the inventory is full. |

## Custom Mix Expansion

Page: [Mix Expansion](../customs/mix-expansion.md).

The keys of this block (`GoblinMixNpc`, `GoblinMixNpcX`, `GoblinMixNpcY`, `GoblinMixNpcMap`) are **not read** by the server. The mix expansion is configured entirely in `Data/Custom/MixExpansion.txt`.

## Custom Event Drop Settings

Item-rain event, defined in `Data/Custom/CustomEventDrop.txt`. Page: [Event Drop](../customs/event-drop.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomEventDropSwitch` | 0 | 0, 1 | `1` = event on. |
| `CustomEventDropText1` | (empty) | text `%s %d` | "will start in N minutes" announcement (event name, minutes). |
| `CustomEventDropText2` | (empty) | text `%s` | "started" announcement. |
| `CustomEventDropText3` | (empty) | text `%s` | "finished" announcement. |

## Custom Store Settings

The custom personal store, where players sell for jewels or coins instead of zen (`/store`), and the offline store (`/offstore`). Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomStoreTime_AL0..3` | 0 | minutes, 0 = unlimited | How long an offline store stays open. |
| `CustomStoreCommandJoBSyntax` | (empty) | word | Argument of `/store` for a shop priced in Jewel of Bless. Shipped: `bless`. |
| `CustomStoreCommandJoSSyntax` | (empty) | word | Jewel of Soul. Shipped: `soul`. |
| `CustomStoreCommandJoCSyntax` | (empty) | word | Jewel of Chaos. Shipped: `chaos`. |
| `CustomStoreCommandWCCSyntax` | (empty) | word | WCoinC. Shipped: `wcc`. |
| `CustomStoreCommandWCPSyntax` | (empty) | word | WCoinP. Shipped: `wcp`. |
| `CustomStoreCommandWCGSyntax` | (empty) | word | Goblin Points. Shipped: `wcg`. |
| `CustomStoreJoBName` ... `CustomStoreWCGName` (6 keys) | (empty) | text | Title shown on the shop for each currency. |

Example: `/store bless` opens a shop priced in Jewels of Bless with the title `.::BLESS STORE::.`.

## Custom Attack Settings

Automatic attack (`/attack`) and offline attack (`/offattack`). Which maps allow it is set in the `CustomAttack` column of `MapManager.txt`. Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomAttackTime_AL0..3` | 0 | minutes, 0 = unlimited | Maximum offline attack time. Shipped: 720 / 1440 / 2160 / 2880 (12 to 48 hours). |
| `CustomAttackDelay` | 0 | ms | Minimum time between two attacks. |
| `CustomAttackAutoPotionDelay` | 0 | ms | Minimum time between two potions (used below 50 % HP or mana). |
| `CustomAttackAutoBuff_AL0..3` | 0 | 0, 1 | `1` = recast the character's buffs automatically. |
| `CustomAttackAutoBuffDelay` | 0 | ms | Interval between buff recasts. |
| `CustomAttackAutoResume_AL0..3` | 0 | 0, 1 | `1` = the server remembers that the character was in offline attack and resumes it automatically when the character is loaded again (for example after a server restart). |

Experience in offline attack uses `OffAttackExperienceRate_AL*` in [Common](common.md#experience-settings).

## Custom Online Lottery Settings

A prize drawn among connected players, defined in `Data/Custom/CustomOnlineLottery.txt`. Page: [Online Lottery](../customs/online-lottery.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomOnlineLotterySwitch` | 0 | 0, 1 | `1` = lottery on. |
| `CustomOnlineLotteryText1` | (empty) | text `%s %d` | Countdown announcement. |
| `CustomOnlineLotteryText2` | (empty) | text `%s` | Winner announcement. |
| `CustomOnlineLotteryText3` | (empty) | text `%s` | "No winner" announcement. |

`CustomOnlineLotteryNoOff` in the shipped file is not read.

## Custom Quest Settings

Quests from `Data/Custom/CustomQuest.txt` (`/quest`). Page: [Custom Quest](../customs/custom-quest.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomQuestSwitch` | 0 | 0, 1 | `1` = custom quests on. |

## Custom RankUser Settings (CustomRankUser.txt)

Rank titles given by reset, master reset or level. Page: [Rank User](../customs/rank-user.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomRankUserSwitch` | 0 | 0, 1 | `1` = rank titles on. |
| `CustomRankUserType` | 0 | 0, 1, 2 | What the ranks are based on: `0` = resets, `1` = master resets, `2` = level. |
| `CustomRankUserRewardSwitch` | 0 | 0, 1 | `1` = give the reward of a rank when it is reached. |
| `CustomRankUserNoticeToAll` | 0 | 0, 1 | `1` = announce a new rank to the whole server. |
| `CustomRankUserNoticeToUser` | 0 | 0, 1 | `1` = tell the player their new rank. |

## Custom Death Message

Page: [Death Message](../customs/death-message.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomDeathMessageSwitch` | 0 | 0, 1 | `1` = messages from `Data/Custom/CustomDeathMessage.txt` on. |

## Custom Quiz

Page: [Quiz](../customs/quiz.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomQuizSwitch` | 0 | 0, 1 | `1` = quiz from `Data/Custom/CustomQuiz.txt` on. |

## Custom NpcCollector Settings (CustomNpcCollector.txt)

Page: [NPC customs](../customs/npc-customs.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomNpcCollectorSwitch` | 0 | 0, 1 | `1` = collector NPCs on. |

## Custom Pick Command (CustomPick.txt)

Page: [Pick](../customs/pick.md).

The keys of this block (`CustomPickSwitch`, `CustomPickEnable_AL0..3`, `CustomPickRequireLevel_AL0..3`, `CustomPickRequireReset_AL0..3`, `CustomPickRequireMoney_AL0..3`) are **not read** by the server. `/pick` is enabled, priced and limited per account level in its row of `Data/Command.txt`; the items it can collect are in `Data/Custom/CustomPick.txt`; the maps where it works are set in the `CustomPick` column of `MapManager.txt`.

## Custom Ranking Settings (CustomRanking.txt) F8 in game

Page: [Ranking](../customs/ranking.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomRankingSwitch` | 0 | 0, 1 | `1` = ranking window (F8) on. |

## Custom EventTime Settings H in game

Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomEventTimeSwitch` | 0 | 0, 1 | `1` = the event schedule window (H key) on. |

## Custom Guild Warehouse

A shared warehouse for the guild. Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `GuildWarehouseEnable` | 0 | 0, 1 | `1` = guild warehouse on. |
| `GuildWarehouseStatus` | 0 | 0, 32, 64, 128 | Minimum guild rank to use it: `0` = all members, `32` = Battle Master and above, `64` = Assistant Master and above, `128` = Guild Master only. |

## Custom Shop MessageBox

Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomShopMessageBox` | 0 | 0, 1, 2 | Confirmation box when buying from NPC shops: `0` = off, `1` = only for items priced in coins, `2` = for every purchase. |

## Custom Quest Settings (CustomNpcQuest.txt)

Quests given by NPCs. Page: [NPC customs](../customs/npc-customs.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomNpcQuestSwitch` | 0 | 0, 1 | `1` = NPC quests on. |

## Buy Inventory Extension Slot

Players buy extra inventory pages. Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `SwitchBuyInventoryExt` | 0 | 0, 1 | `1` = purchase on. |
| `PriceInventoryExt1..4` | 100 | WCoinC | Price of the 1st, 2nd, 3rd and 4th extension, paid in WCoinC (Coin1). |
| `message_success` | 0 | text index, 0 = none | Index of the `Data/Lang` text sent on success. |
| `message_failed` | 0 | text index, 0 = none | Text sent when the purchase is not possible (dead, trading, maximum reached). |
| `message_error` | 0 | text index, 0 = none | Text sent when the player lacks WCoinC. |

{% hint style="warning" %}
The three `message_*` keys are **text numbers** from `Data/Lang/NNN - Xxx_Message.txt`, not on/off switches. The shipped value `1` shows text 1 ("Blood Castle has closed"). Set them to the numbers of suitable texts, or 0 for no message.
{% endhint %}

## Chat System Anti Spam Settings

Page: [Server features](../customs/server-features.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `EnableAntiSpam` | 0 | 0, 1 | `1` = anti-spam on. |
| `LimitMaxMessage` | 0 | number | How many times the **same** message may be repeated. |
| `LimitMaxTimeMessageCheck` | 0 | seconds | Time window in which repeats are counted. |
| `TimeBlockedChat` | 0 | minutes | How long the spammer's chat is blocked. |

Shipped: 3 identical messages within 60 seconds block the chat for 1 minute.

## Custom Morpheus Shop Settings

Page: [Morpheus Shop](../customs/morpheus-shop.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomMorpheusShopSwitch` | 0 | 0, 1 | `1` = the Morpheus Shop window is on. `Data/Custom/CustomMorpheusShop.txt` is always loaded (so errors appear in the log), but purchases are only accepted when this is 1. |

## Custom Balance Class

Single PvP damage formula for every class, using the character's base stats. Does not change damage against monsters. Page: [Balance Class](../customs/balance-class.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomBalanceClassSwitch` | 0 | 0, 1 | `1` = PvP damage is calculated by Balance Class (`Data/Custom/CustomBalanceClass.dat`) instead of the class rates of [Character](character.md). |
| `CustomBalanceClassLifeSwitch` | 0 | 0, 1 | `1` = HP and SD are calculated from Vitality by the balance rules. Also affects PvM. |
| `CustomBalanceClassItemBonus` | 1 | 0, 1 | `1` = apply the item bonuses defined in the balance table. |
| `CustomBalanceClassNoMiss` | 1 | 0, 1 | `1` = PvP attacks never miss. |
| `CustomBalanceClassKeepEventRate` | 1 | 0, 1 | `1` = still apply the duel, Gens, arena, Chaos Castle, Illusion Temple and Castle Siege damage rates of Character.dat on top of the balanced damage. |

## Passe de Batalha (Battle Pass)

Seasonal pass with free and premium tracks. Page: [Battle Pass](../customs/battle-pass.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CustomBattlePassSwitch` | 0 | 0, 1 | `1` = Battle Pass on. **Half of the switch**: the client must also have `CustomBattlePassSwitch = 1` in `MainInfo.ini` `[Custom]`, or the button does not appear. |
| `CustomBattlePassPaymentType` | 1 | 1, 2 | How the premium pass is paid. `1` = in-game coins (see `CustomBattlePassCoinID`). `2` = the web wallet set by `CustomBattlePassWalletID`. |
| `CustomBattlePassCoinID` | 1 | 1, 2, 3 | With payment type 1: `1` = WCoinC, `2` = WCoinP, `3` = Goblin Points. Zen is not accepted. |
| `CustomBattlePassWalletID` | 0 | number | With payment type 2: the wallet to charge. |

{% hint style="danger" %}
`Data/Custom/CustomBattlePass.txt` and the season file `Data/Custom/BattlePass/NNN - <name>.txt` are loaded **even when the switch is 0**, so a malformed file stops the GameServer at startup. Validate the files before restarting. The Battle Pass texts are in `Data/Lang/` (texts 950 to 999, and 1000 and up per season).
{% endhint %}

---

## Example

Turn on the anti-spam and the guild warehouse for Battle Masters and above:

```ini
EnableAntiSpam = 1
TimeBlockedChat = 5
LimitMaxMessage = 3
LimitMaxTimeMessageCheck = 60

GuildWarehouseEnable = 1
GuildWarehouseStatus = 32
```

## Rules and pitfalls

- **Switch on, but nothing happens?** Many customs also need their data file filled, a column in `MapManager.txt`, a row in `Command.txt`, or a client file. Check the custom's page.
- **Two halves:** Battle Pass, Morpheus Shop, custom wings, jewels and VIP plans need matching client files. See [Client](../client/README.md).
- **Unread keys:** `GoblinMixNpc*`, `CustomOnlineLotteryNoOff` and the whole `CustomPick*` block do nothing; configure those features where described above.
- **GameServerCS has its own copy** of this file. Turn a custom on in both if players use it on Castle Siege maps.
