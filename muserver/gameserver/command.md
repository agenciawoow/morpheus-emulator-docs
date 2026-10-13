# GameServerInfo - Command

## What it does

Extra settings for chat commands that need more than "on/off and price". Which commands exist, who can use them, their zen/coin cost, level and reset limits and their delay are set in `Data/Command.txt`; this file holds what `Command.txt` cannot express: how `/reset` and `/mreset` behave, how many warehouses `/bau` opens, reward texts, and so on.

## Where it lives

- `GameServer/Data/GameServerInfo - Command.dat`
- `Data/Command.txt` in the MuServer root (shared by all GameServers)
- `GameServerCS/Data/GameServerInfo - Command.dat` (separate copy)

INI format, section `[GameServerInfo]`. `_AL0..3` = Free, VIP 1, VIP 2, VIP 3.

### Command.txt in short

Each row of `Data/Command.txt` is a command: index, name (`"/reset"`), and for each account level: enabled, zen cost, min/max level, min/max reset; then delay, required GM level, and WCoinC/WCoinP/Goblin Point costs. Command names can be changed there (for example `/bau` to `/ware`). See [Commands](../data/commands.md) for the full column list.

## Reload

`/reload command` re-reads `Command.txt` and this file.

---

## Post Command Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandPostType` | 0 | 0 to 5 | How `/post` messages are shown. `0` to `3` = only on this GameServer, in four different chat styles. `4` = sent to every GameServer of the same server group. `5` = same as 4 with the alternate style. Shipped: 4. |

## Add Point Command Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandAddPointAutoEnable_AL0..3` | 0 | 0, 1 | `1` = players can use the **auto** form of the stat commands (`/s auto <points>` and so on), which spends new level-up points automatically. |

## Change Command Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandChangeLimit_AL0..3` | 0 | 0 to 3 | Highest class evolution `/evo` can reach. `1` = second class, `2` = third class. Magic Gladiator, Dark Lord, Rage Fighter and Grow Lancer count one level higher. `0` = `/evo` always refuses. |

## Ware Command Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandWareNumber_AL0..3` | 0 | 1 and up | Number of warehouses an account can switch between with `/bau <number>` (numbers 0 to value − 1). Shipped: 1 / 2 / 3 / 4. |

## Reset Command Settings

`/reset`. The level, zen and points below are **fallbacks**: if `Data/Util/ResetTable.txt` has a row for the character's reset range, the table's values win. See [Reset and experience](../data/reset-and-experience.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandResetType` | 0 | 0, 1 | `0` = keep stats and points. `1` = reset stats to the class base and give points again (see `CommandResetPoint`). |
| `CommandResetAutoEnable_AL0..3` | 0 | 0, 1 | `1` = players can turn on automatic reset (`/reset auto`). |
| `CommandResetCheckItem_AL0..3` | 0 | 0, 1 | `1` = the character must have no equipped items. |
| `CommandResetMove_AL0..3` | 0 | 0, 1 | `1` = move the character to its town after the reset. |
| `CommandResetQuest_AL0..3` | 0 | 0, 1 | `1` = clear class quests (the character goes back to first class). |
| `CommandResetSkill_AL0..3` | 0 | 0, 1 | `1` = clear learned skills. |
| `CommandResetLevel_AL0..3` | 0 | level | Level required. |
| `CommandResetMoney_AL0..3` | 0 | zen | Zen cost when neither `ResetTable.txt` nor the `/reset` row of `Command.txt` sets one. |
| `CommandResetCount_AL0..3` | 0 | number | Resets gained per use (Bonus Manager events can add more). |
| `CommandResetLimit_AL0..3` | 0 | number | Maximum total resets. |
| `CommandResetLimitDay_AL0..3` | 0 | number | Maximum resets per day. |
| `CommandResetLimitWek_AL0..3` | 0 | number | Maximum resets per week. |
| `CommandResetLimitMon_AL0..3` | 0 | number | Maximum resets per month. |
| `CommandResetStartLevel_AL0..3` | 0 | level | Level after the reset. |
| `CommandResetPoint_AL0..3` | 0 | points per reset | With type 1: points = this value × total resets, plus level-up points, quest and fruit points. |
| `CommandResetPointRateDW`, `...DK`, `...FE`, `...MG`, `...DL`, `...SU`, `...RF` | 0 | percent | Percentage of the reset points each class receives. `100` = full. |

{% hint style="warning" %}
`CommandResetPointRate` keys for GL, RW, SL, GC, KUN, LEM and KIN are not in the shipped file and default to **0**. If your client has those classes, add them with `100`.
{% endhint %}

### Reset reward

Coins paid on every reset. Fixed value per reset, per account level. `0` = off. Never negative.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandResetRewardCoin1Value_AL0..3` | 0 | amount | WCoinC per reset. |
| `CommandResetRewardCoin2Value_AL0..3` | 0 | amount | WCoinP per reset. |
| `CommandResetRewardCoin3Value_AL0..3` | 0 | amount | Goblin Points per reset. |

## Master Reset Command Settings

`/mreset`. Like `/reset`, the level, reset count, zen and points are fallbacks for `Data/Util/MasterResetTable.txt`.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandMasterResetType` | 0 | 0, 1 | `0` = keep points. `1` = add the master reset points below. |
| `CommandMasterResetCheckItem_AL0..3` | 0 | 0, 1 | `1` = no equipped items allowed. |
| `CommandMasterResetMove_AL0..3` | 0 | 0, 1 | `1` = move to town afterwards. |
| `CommandMasterResetQuest_AL0..3` | 0 | 0, 1 | `1` = clear class quests. |
| `CommandMasterResetSkill_AL0..3` | 0 | 0, 1 | `1` = clear skills. |
| `CommandMasterResetLevel_AL0..3` | 0 | level | Level required. |
| `CommandMasterResetReset_AL0..3` | 0 | resets | Resets required. |
| `CommandMasterResetMoney_AL0..3` | 0 | zen | Zen cost. |
| `CommandMasterResetCount_AL0..3` | 0 | number | Master resets gained per use. |
| `CommandMasterResetLimit_AL0..3` | 0 | number | Maximum total master resets. |
| `CommandMasterResetLimitDay_AL0..3`, `...LimitWek_AL0..3`, `...LimitMon_AL0..3` | 0 | number | Maximum per day, week and month. |
| `CommandMasterResetStartLevel_AL0..3` | 0 | level | Level after the master reset. |
| `CommandMasterResetStartReset_AL0..3` | 0 | resets, -1 | Resets the character keeps. `-1` = subtract the required resets (`CommandMasterResetReset`). Ignored when `CommandMasterResetRemoveReset` is above 0. |
| `CommandMasterResetRemoveReset_AL0..3` | 0 | resets | When above 0, this many resets are **subtracted** instead of using `StartReset`. Shipped: 500. |
| `CommandMasterResetPoint_AL0..3` | 0 | points | Points per master reset (type 1). |
| `CommandMasterResetPointRateDW` ... `...KIN` (14 keys) | 0 | percent | Percentage of the master reset points each class receives. |

### Master reset reward

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandMasterResetRewardCoin1Value_AL0..3` | 0 | amount | WCoinC per master reset. |
| `CommandMasterResetRewardCoin2Value_AL0..3` | 0 | amount | WCoinP per master reset. |
| `CommandMasterResetRewardCoin3Value_AL0..3` | 0 | amount | Goblin Points per master reset. |

## Marry Command Settings

`/marry`.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandMarryLevel` | 0 | level | Minimum level to marry. |
| `CommandMarryCost` | 0 | zen | Zen cost. |
| `CommandMarryNoticeDelay` | 0 | seconds | Minimum time between marriage notices of the same player. |
| `CommandMarryMinTime` | 0 | seconds | Minimum time married before a divorce is allowed. `604800` = 7 days. |
| `CommandMarryOnlyGM` | 0 | 0, 1 | `1` = `/marry to` can only be used by a GM, who performs the marriage. `0` = players propose to each other. |

## Command Open WareHouse

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandOpenWareOnlySafeZone` | 0 | 0, 1 | `1` = `/abrirbau` (open warehouse anywhere) only works inside safe zones. |

## Command Change Class

`/classe` lets a player change the class of their character.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandChangeClassToDW` | 0 | 0, 1 | `1` = changing to Dark Wizard is allowed. |
| `CommandChangeClassToDK` | 0 | 0, 1 | Dark Knight. |
| `CommandChangeClassToELF` | 0 | 0, 1 | Fairy Elf. |
| `CommandChangeClassToMG` | 0 | 0, 1 | Magic Gladiator. |
| `CommandChangeClassToDL` | 0 | 0, 1 | Dark Lord. |
| `CommandChangeClassToSU` | 0 | 0, 1 | Summoner. |
| `CommandChangeClassToRF` | 0 | 0, 1 | Rage Fighter. Not in the shipped file: add it to allow it. |

## Reward Command Settings

`/premiar <name> <type> <value>` (GM): gives coins to one player.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandRewardMaxType` | 0 | 1 to 3 | Highest type accepted. Type 1 = Coin1 (WCoinC), 2 = Coin2 (WCoinP), 3 = Coin3 (Goblin Points). |
| `CommandRewardMaxValue` | 0 | amount | Highest value per use. |
| `CommandRewardType1Text`, `...Type2Text`, `...Type3Text` | (empty) | text | Global announcement for each type. `%s` = GM name, `%s` = player name, `%d` = amount. |

## RewardAll Command Settings

`/premiarall <type> <value>` (GM): gives coins to every online player.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandRewardAllMaxType` | 0 | 0 to 3 | Highest type accepted. **Shipped: 0, which disables the command.** Set 3 to allow all three coins. |
| `CommandRewardAllMaxValue` | 0 | amount | Highest value per use. |
| `CommandRewardAllType1Text`, `...Type2Text`, `...Type3Text` | (empty) | text | Global announcement. `%s` = GM name, `%d` = amount. |

## Command Rename (Premium)

`/mudarnome <new name>`.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandRenameDelay_AL0..3` | 0 | seconds | Minimum time between two renames. |
| `CommandRenameNeedTicket` | 0 | 0, 1 | `1` = the player needs a rename ticket. |

Characters in a guild cannot be renamed. Forbidden words come from the DataServer's `BadSyntax.txt` and `Data/Util/FilterRename.txt`.

## Gift Command Settings (/presente)

Drops an item from the **Gift** EventItemBag at the player's feet, with fireworks.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandGiftLimit` | 0 | number | How many gifts a character can claim in total (the DataServer counts the claims). With `0` every claim is refused. |

The items are configured in the Gift bag of `Data/EventItemBagManager.txt`. See [EventItemBag](../data/event-item-bag.md).

## Command Help (/ajuda)

Summons helper monsters that fight for the player. Using the command again dismisses them.

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandHelpMonsterIndex` | 0 | monster index | Monster summoned (index from `Monster.txt`). Shipped: 151. |
| `CommandHelpMonsterAmount` | 0 | 1 to 5 | How many are summoned. Keep it at 5 or less. |

## Command Bloc

GM ban commands (`/banacc`, `/banchar`).

| Key | Default | Values | Description |
|---|---|---|---|
| `CommandBlocAccShowMessage` | 0 | 0, 1 | `1` = announce account bans to the whole server. |
| `CommandBlocCharShowMessage` | 0 | 0, 1 | `1` = announce character bans to the whole server. |

---

## Example

Reset at level 400 (380 for VIP 1), keep points, 3 resets per day for free accounts:

```ini
CommandResetType = 0
CommandResetLevel_AL0 = 400
CommandResetLevel_AL1 = 380
CommandResetLevel_AL2 = 350
CommandResetLevel_AL3 = 320
CommandResetLimitDay_AL0 = 3
CommandResetLimitDay_AL1 = 10
CommandResetLimitDay_AL2 = 20
CommandResetLimitDay_AL3 = 50
CommandResetStartLevel_AL0 = 1
CommandResetStartLevel_AL1 = 1
CommandResetStartLevel_AL2 = 1
CommandResetStartLevel_AL3 = 1
```

## Rules and pitfalls

- **Is the command enabled?** A setting here has no effect if the command is disabled for that account level in `Command.txt`.
- **ResetTable wins.** If you change `CommandResetLevel` and nothing happens, `Data/Util/ResetTable.txt` probably has a row for that reset range.
- **`CommandMasterResetRemoveReset` overrides `StartReset`.** With the shipped 500, a master reset always removes 500 resets, whatever `CommandMasterResetStartReset` says.
- **Missing per-class rates are 0 points.** Check `CommandResetPointRate*` and `CommandMasterResetPointRate*` for every class you use.
- **Texts are not translated.** `CommandReward*Text` are sent as written; write them in your players' language.
- The Castle Siege server has its own copy of this file. The shipped CS copy has no reset reward keys.
