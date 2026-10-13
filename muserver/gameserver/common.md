# GameServerInfo - Common

## What it does

The main settings file of a GameServer: who the server is and where its partners are, experience and drop rates, PK rules, trade, guilds, parties, potions, jewels, MU Helper (online and offline), party voice, anti-hack checks, cash shop and a set of Morpheus premium switches.

## Where it lives

- `GameServer/Data/GameServerInfo - Common.dat`
- `GameServerCS/Data/GameServerInfo - Common.dat` (the Castle Siege server has its own copy, see [Castle Siege server](castle-siege-server.md))

INI format, one section `[GameServerInfo]`. See [GameServer overview](README.md#how-to-read-the-key-tables) for the table columns, `_AL0..3` and class codes.

## Reload

| Keys | Reload |
|---|---|
| Server identity, ports, connection addresses and log switches (marked **restart** below) | Restart the GameServer |
| Cash Shop block | `/reload cashshop` |
| Hack and Hack Speed Skill blocks | `/reload hack` |
| `DefaultLanguageSystem` | `/reload lang` or `/reload common` |
| Everything else | `/reload common` |

---

## Lang System Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `DefaultLanguageSystem` | 0 | 0 ENG, 1 POR, 2 SPN, 3 CHS, 4 KOR, 5 VIE, 6 TAI | Language of server messages for players whose client has not told the server its language, and for the server logs. Players who chose a language in the client keep their own. Out-of-range values fall back to 0. See [Languages](languages.md). |

## Server Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `ServerName` | (empty) | text | Name of this GameServer, shown in its window and logs. **Restart.** |
| `ServerCode` | 0 | 0 to 199 | Code of this GameServer. Must match its row in `ConnectServer/Data/ServerList.dat` (group × 20 + code) and in `Data/MapServerInfo.dat`. **Restart.** |
| `ServerLock` | 0 | 0 to 3 | Minimum account level allowed to log in. `0` = everyone, `1` = VIP 1 and above, `2` = VIP 2 and above, `3` = VIP 3 only. |
| `ServerPort` | 0 | 1 to 65535 | TCP port players connect to. Must match `ServerPort` in `ServerList.dat`. **Restart.** |
| `ServerVersion` | (empty) | `X.XX.XX` | Client version this server accepts. Must match your client. **Restart.** |
| `ServerSerial` | (empty) | 16 characters | Client serial this server accepts. Must match your client. Keep it private. **Restart.** |
| `ServerMaxUserNumber` | 0 | 1 to the build limit | Maximum players online on this GameServer. |
| `ServerMinLevel` / `ServerMaxLevel` | 0 / 0 | level, 0 = no limit | Characters below the minimum or above the maximum level cannot enter this server. |
| `ServerMinReset` / `ServerMaxReset` | 0 / 0 | resets, 0 = no limit | Same, for resets. |
| `ServerMinMasterReset` / `ServerMaxMasterReset` | 0 / 0 | master resets, 0 = no limit | Same, for master resets. |
| `AccountLevelLock_AL0..3` | 1 | 0, 1 | `0` = accounts of that level cannot log in to this server. Use it to make a VIP-only server (`AccountLevelLock_AL0 = 0`). |
| `ServerLevelAddMaster` | 1 | 0, 1 | `1` = master level is added to the normal level when checking level requirements (move, gates, items, custom moves and quests). `0` = only the normal level counts. |

## Experience Calc System Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MaxLevelCharacter` | 400 | 1 and up | Maximum character level. Also sets the size of the experience table. Values above 400 scale level-based formulas proportionally. |

The other keys in this block (`ExpFactorBase`, `ExpIncreaseCurve`, `ExpIncreaseExplosive`) are not read by the server. To change how fast players level, use the [Experience Settings](#experience-settings) rates or `Data/Util/ExperienceTable.txt`.

## Connection Settings

All **restart**.

| Key | Default | Values | Description |
|---|---|---|---|
| `DataServerAddress` | (empty) | IP | Address of the DataServer. `127.0.0.1` when on the same machine. |
| `DataServerPort` | 0 | port | Must equal `DataServerPort` in `DataServer.ini` (shipped: 55960). |
| `JoinServerAddress` | (empty) | IP | Address of the JoinServer. |
| `JoinServerPort` | 0 | port | Must equal `JoinServerPort` in `JoinServer.ini` (shipped: 55970). |
| `ConnectServerAddress` | (empty) | IP | Address of the ConnectServer, for the online-count report. |
| `ConnectServerPort` | 0 | port | The ConnectServer **UDP** port (`ConnectServerPortUDP`, shipped: 55557). |

## Log Settings

Each switch writes a separate log folder next to `GameServer.exe`. `1` = on.

| Key | Default | Values | Description |
|---|---|---|---|
| `WriteChatLog` | 0 | 0, 1 | Chat messages. **Restart.** |
| `WriteCommandLog` | 0 | 0, 1 | Commands used (including GM commands and `/reload`). **Restart.** |
| `WriteTradeLog` | 0 | 0, 1 | Trades between players. **Restart.** |
| `WriteConnectLog` | 0 | 0, 1 | Connections and disconnections. **Restart.** |
| `WriteHackLog` | 0 | 0, 1 | Anti-hack detections. **Restart.** |
| `WriteCashShopLog` | 0 | 0, 1 | Cash shop purchases. **Restart.** |
| `WriteChaosMixLog` | 0 | 0, 1 | Chaos Machine mixes and results. **Restart.** |
| `WriteBattlePassLog` | 1 | 0, 1 | Battle Pass purchases, claims and missions. **Restart.** |
| `WriteAuditItemBagsLog` | 1 | 0, 1 | At startup and on reload, checks every `Data/EventItemBag/` file against `Item.txt` and logs entries that point to items that do not exist. |

## Hack Settings

Server-side checks against modified clients. Reload with `/reload hack`.

| Key | Default | Values | Description |
|---|---|---|---|
| `CheckSpeedHack` | 0 | 0, 1 | Compare the attack and magic speed the client reports with the speed the server calculated. |
| `CheckSpeedHackTolerance` | 0 | number | Allowed difference before the client is treated as hacked. |
| `CheckLatencyHack` | 0 | 0, 1 | Compare the client's clock with the server's to catch speed-up tools. |
| `CheckLatencyHackTolerance` | 0 | milliseconds | Allowed difference. |
| `CheckAutoPotionHack` | 0 | 0, 1 | Block potions used faster than a human can. |
| `CheckAutoPotionHackTolerance` | 0 | milliseconds | Minimum time between two potions. |
| `CheckAutoComboHack` | 0 | 0, 1 | Block combos executed faster than a human can. |
| `CheckAutoComboHackTolerance` | 0 | milliseconds | Minimum time between combo steps. |

{% hint style="warning" %}
Turn these on one at a time and watch the hack log. A tolerance that is too low punishes honest players with lag.
{% endhint %}

## Hack Speed Skill Settings (HackSkillCheck.txt)

Checks skill usage speed against the limits in `Data/Hack/HackSkillCheck.txt` (see [Anti-hack files](../data/anti-hack.md)). Reload with `/reload hack`.

| Key | Default | Values | Description |
|---|---|---|---|
| `SpeedHackSkillEnable` | 0 | 0, 1 | Turn the check on. |
| `SpeedHackSkillType` | 0 | 0 to 4 | What to do when caught: `0` log only, `1` disconnect, `2` disable attack, `3` disable attack for `SpeedHackSkillPenalty` ms, `4` disconnect and block the account. |
| `SpeedHackSkillPenalty` | 0 | milliseconds | Duration of type 3. |
| `SpeedHackSkillDialog` | 0 | 0, 1 | `1` = show the player a message box explaining why. |
| `SpeedHackSkillDebug` | 0 | 0, 1 | `1` = log every check (troubleshooting only). |

## Common Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MaxItemOption` | 0 | 1 to 7 | Highest "additional option" (+4 per step) an item can reach with Jewel of Life and drops. `4` = +16, `7` = +28. |
| `MaxIpConnection` | 0 | 1 and up | Maximum simultaneous connections from one IP to this GameServer. **0 refuses every connection.** Keep it high: shared Wi-Fi, mobile carriers and browser play (relay) put many players behind one IP. |
| `MaxHWIDConnection` | 0 | 1 and up | Maximum simultaneous connections from one device (hardware ID). **0 refuses every connection.** |
| `PersonalCodeCheck` | 0 | 0, 1 | `1` = deleting a character and guild master actions (kicking members, disbanding) require the account's personal code. `0` = any code is accepted. |
| `SetItemAcceptHarmonySwitch` | 0 | 0, 1 | `1` = ancient (set) items can receive Jewel of Harmony options. |
| `SetLuckyReleaseLock` | 0 | 0, 1 | `1` = lucky items can be traded, stored in the warehouse and receive custom jewels. `0` = they are locked to the character. |

## Monster Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MonsterLifeRate` | 0 | percent | Multiplies the life of every monster. `100` = as in `Monster.txt`, `200` = double. **Do not leave it at 0.** |

## Health Bar

| Key | Default | Values | Description |
|---|---|---|---|
| `MonsterHealthBarSwitch` | 0 | 0, 1 | Show a health bar over monsters. |
| `UserHealthBarSwitch` | 0 | 0, 1 | Show a health bar over players. |

## PK Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `NonPK` | 0 | 0, 1 | `1` = PvP is off on every map of this server (maps can override it in `MapManager.txt`). |
| `PKLimitFree` | 0 | 0, 1 | `1` = murderers (PK level 4 and up) can enter Blood Castle, Chaos Castle and Devil Square. With `0`, the per-event switches below decide. |
| `PKDownTime1` | 0 | PK-time points | A hero (positive PK status) loses one hero level when their PK time passes this value. PK time grows by 1 every second online and by the level of each monster killed. |
| `PKDownTime2` | 0 | PK-time points | A murderer loses one PK level when their PK time passes this value (same counting as above). Only used when `PKTimePenaltyForKill = 0`. |
| `PKDisableShop` | 0 | 0, 1 | `1` = murderers cannot buy from NPC shops. |
| `PKDisableTrade` | 0 | 0, 1 | `1` = murderers cannot trade. |
| `PKTimePenaltyForKill` | 0 | seconds, 0 = off | When greater than 0, each kill adds this many seconds of PK time, which counts down while online; the remaining time is shown on login. Replaces the classic decay of `PKDownTime2`. Shipped: 10800 (3 hours). |

## Pk Can Enter in events

| Key | Default | Values | Description |
|---|---|---|---|
| `PkCanEnterBC` | 0 | 0, 1 | `1` = murderers can enter Blood Castle. |
| `PkCanEnterCC` | 0 | 0, 1 | `1` = murderers can enter Chaos Castle. |
| `PkCanEnterDS` | 0 | 0, 1 | `1` = murderers can enter Devil Square. |

## Pk Item Drop

A murderer (PK level 6) who dies can drop an equipped item, on maps where the `PkDropItem` column of `MapManager.txt` is `1`.

| Key | Default | Values | Description |
|---|---|---|---|
| `PkItemDropSwitch` | 0 | 0, 1 | Turn the system on. |
| `PkItemDropEnable_AL0..3` | 0 | 0, 1 | Which account levels can lose items. |
| `PkItemDropMinLevel` / `PkItemDropMaxLevel` | 0 / 0 | 0 to 15 | Only items with a + level in this range can drop. |
| `PkItemDropExecellent` | 0 | 0, 1 | `1` = excellent items can drop. (The key name has this spelling.) |
| `PkItemDropSocketItem` | 0 | 0, 1 | `1` = socket items can drop. |
| `PkItemDropSetItem` | 0 | 0, 1 | `1` = ancient (set) items can drop. |
| `PkItemDropHarmony` | 0 | 0, 1 | `1` = items with Jewel of Harmony options can drop. |
| `PkItemDropRaven` | 0 | 0, 1 | `1` = Dark Raven can drop. |
| `PkItemDropHorse` | 0 | 0, 1 | `1` = Dark Horse can drop. |
| `PkItemDropFenrir` | 0 | 0, 1 | `1` = Fenrir can drop. |
| `PkItemDropRate` | 0 | 0 to 100 | Chance (percent) that a drop happens on death. |
| `PkItemDropInventory` | 0 | 0, 1 | Not in the shipped file. `1` = if no equipped item dropped, try an inventory item instead. |

## Trade Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `TradeSwitch` | 0 | 0, 1 | `1` = players can trade. |

## Personal Shop Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `PersonalShopSwitch` | 0 | 0, 1 | `1` = players can open personal shops. |

## Duel Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `DuelSwitch` | 0 | 0, 1 | `1` = duels allowed. |
| `DuelArenaAnnounceSwitch` | 0 | 0, 1 | `1` = announce duel start and end to the whole server. |
| `DuelArenaDisableRestoreHP` | 0 | 0, 1 | `1` = HP is not restored when the duel ends. |
| `DuelArenaDisableRestoreSD` | 0 | 0, 1 | `1` = SD is not restored when the duel ends. |
| `DuelMaxScore` | 10 | number | Points needed to win a duel. |
| `DuelBet` | 0 | 0, 1 | `1` = duels can be played for a bet; the winner takes it when the duel ends. |

## Guild Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `GuildCreateSwitch` | 0 | 0, 1 | Read by the server but currently has no effect. Use `GuildCreateMinLevel` to control creation. |
| `GuildDeleteSwitch` | 0 | 0, 1 | `1` = guild masters can disband their guild. |
| `GuildCreateMinLevel` | 0 | level | Minimum level to create a guild. |
| `GuildInsertMaxUser1` | 0 | 1 to 80 | Maximum members for a guild whose master is not a Dark Lord. A master with 0 resets gets level / 10 slots, up to this value. |
| `GuildInsertMaxUser2` | 0 | 1 to 80 | Same for Dark Lord masters (level / 10 + Command / 10 when 0 resets). |
| `GuildAllianceMinUser` | 0 | number | Minimum members a guild needs to join an alliance. |
| `GuildAllianceMaxCount` | 0 | number | Maximum guilds in one alliance (capped by the build limit). |
| `GuildOwnerDestroyLimit` | 0 | 0, 1 | `1` = the guild that owns the castle cannot be disbanded. |

## Elf Buffer Settings

The NPC Elf Soldier gives a free damage and defense buff to low players.

| Key | Default | Values | Description |
|---|---|---|---|
| `ElfBufferMaxLevel_AL0..3` | 0 | level | Highest level that can still receive the buff. |
| `ElfBufferMaxReset_AL0..3` | 0 | resets | Highest reset count that can still receive the buff. |
| `ElfBufferDamageConstA` / `ElfBufferDamageConstB` | 0 / 0 | number | Damage bonus = A + level / B. |
| `ElfBufferDefenseConstA` / `ElfBufferDefenseConstB` | 0 / 0 | number | Defense bonus = A + level / B. |

## Experience Settings

Rates are multipliers: `200` means 200 times the base experience. They are multiplied by map rates (`MapManager.txt`), Bonus Manager events and item effects.

| Key | Default | Values | Description |
|---|---|---|---|
| `AddExperienceRate_AL0..3` | 0 | multiplier | Normal experience from monsters. Shipped: 200 / 220 / 230 / 240. |
| `AddMasterExperienceRate_AL0..3` | 0 | multiplier | Experience for characters in master level. |
| `MinMasterExperienceMonsterLevel_AL0..3` | 0 | monster level | Master-level characters get no experience from monsters below this level. |
| `AddEventExperienceRate_AL0..3` | 0 | multiplier | Experience rewards of events (Blood Castle, Chaos Castle, Devil Square...). |
| `AddQuestExperienceRate_AL0..3` | 0 | multiplier | Experience rewards of quests. |
| `OffAttackExperienceRate_AL0..3` | 0 | multiplier | Experience while in offline attack (`/offattack`). |
| `AutoResetExperienceRate_AL0..3` | 0 | multiplier | Experience while auto reset is on. |

## Item Drop Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `ItemDropTime` | 0 | seconds | How long a dropped item stays on the ground. Only the killer can pick it up during the first half of this time. |
| `ItemDropRate_AL0..3` | 0 | number | Item drop chance from monsters. The chance is this value divided by the monster's `ItemRate` in `Monster.txt`, then multiplied by map and bonus rates. |

## Money Drop Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MoneyDropTime` | 0 | seconds | How long dropped zen stays on the ground. |
| `MoneyAmountDropRate_AL0..3` | 0 | percent | Multiplies the amount of zen dropped by monsters. |

## Advanced Money Drop Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MoneyValue_AL0..3` | 0 | number, 0 = off | When greater than 0, zen dropped = monster level × this value (plus item bonuses), instead of the standard formula. |

## Item Durability Settings

Percent of the normal durability. `100` = normal; higher values make items last longer.

| Key | Default | Values | Description |
|---|---|---|---|
| `WeaponDurabilityRate` | 0 | percent | Weapons. |
| `ArmorDurabilityRate` | 0 | percent | Armor pieces and shields. |
| `WingDurabilityRate` | 0 | percent | Wings. |
| `GuardianDurabilityRate` | 100 | percent | Guardians (Angel, Imp, Uniria, Dinorant...). 0 or less is treated as 100. |
| `PendantDurabilityRate` | 0 | percent | Pendants. |
| `RingDurabilityRate` | 0 | percent | Rings. |
| `PetDurabilityRate` | 100 | percent | Dark Horse, Dark Raven, Fenrir. 0 or less is treated as 100. |
| `PetDurabilityBlock` | 0 | 0, 1 | `1` = pets never lose durability. |

## Trade Item Block Settings

Blocks moving valuable items to other players. GMs are not affected.

| Key | Default | Values | Description |
|---|---|---|---|
| `TradeItemBlock` | 0 | 0, 1 | Master switch of this block. |
| `TradeItemBlockExc` | 0 | 0 to 6 | Block excellent items with at least this many excellent options. `0` = do not block excellent items. |
| `TradeItemBlockSet` | 0 | 0, 1 | Block ancient (set) items. |
| `TradeItemBlockHarmony` | 0 | 0, 1 | Block items with Jewel of Harmony options. |
| `TradeItemBlockSell` | 0 | 0, 1 | Also block selling those items to NPCs. |

`TradeItemBlockLucky` in the shipped file is not read; lucky items are controlled by `SetLuckyReleaseLock`.

## Level Up Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MaxLevelUp` | 0 | number, 0 = no limit | Maximum levels gained from a single monster kill. Extra experience beyond the last allowed level is lost. |
| `MaxLevelUpEvent` | 0 | number, 0 = no limit | Maximum levels gained from a single event reward (Blood Castle, Chaos Castle, Crywolf...). |
| `MaxLevelUpQuest` | 0 | number, 0 = no limit | Maximum levels gained from a single quest reward. |
| `MaxStatPoint_AL0..3` | 0 | 1 to 65000 | Highest value of each stat (Strength, Agility...). |
| `DWLevelUpPoint`, `DKLevelUpPoint`, `FELevelUpPoint`, `MGLevelUpPoint`, `DLLevelUpPoint`, `SULevelUpPoint`, `RFLevelUpPoint` | 0 | points | Stat points per level for each class. |
| `GLLevelUpPoint`, `RWLevelUpPoint`, `SLLevelUpPoint`, `GCLevelUpPoint`, `KUNLevelUpPoint`, `LEMLevelUpPoint`, `KINLevelUpPoint` | 1 | points | Same for the newer classes. |
| `PlusStatPoint` | 0 | points | Extra points per level above 220 after finishing the third quest (Marlon). |

## Create Character Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `CharacterCreateSwitch` | 0 | 0, 1 | `1` = players can create characters on this server. |
| `MGCreateType`, `DLCreateType`, `SUCreateType`, `RFCreateType` | 0 | 0, 1 | How the special classes are unlocked. `0` = by the level/reset/master reset requirements below. `1` = the standard way (account card / character level). |
| `MGCreateLevel_AL0..3` | 0 | level | Level of another character on the account required to create a Magic Gladiator. |
| `MGCreateReset_AL0..3` | 0 | resets | Resets required. |
| `MGCreateMasterReset_AL0..3` | 0 | master resets | Master resets required. |
| `DLCreateLevel_AL0..3`, `DLCreateReset_AL0..3`, `DLCreateMasterReset_AL0..3` | 0 | | Same for Dark Lord. |
| `SUCreateLevel_AL0..3`, `SUCreateReset_AL0..3`, `SUCreateMasterReset_AL0..3` | 0 | | Same for Summoner. |
| `RFCreateLevel_AL0..3`, `RFCreateReset_AL0..3`, `RFCreateMasterReset_AL0..3` | 0 | | Same for Rage Fighter. |

## Shield Gauge Settings

SD (shield) in PvP.

| Key | Default | Values | Description |
|---|---|---|---|
| `ShieldGaugeRate` | 0 | percent | Share of PvP damage taken by SD before HP. Shipped: 90. |
| `ShieldGaugeConstA` | 0 | number | Max SD = (stat total × A / 10) + ((level + master level)² / B) + defense / 2. |
| `ShieldGaugeConstB` | 0 | number | See above. |
| `ShieldGaugeAttackRate` | 0 | multiplier | Weight of attack success rate in the SD-piercing chance. |
| `ShieldGaugeAttackComboMiss` | 0 | 0, 1 | `1` = combos can miss against SD. |

## Master Skill Tree Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `MasterSkillTree` | 0 | 0, 1 | `1` = master level and the master skill tree are on. |
| `MasterSkillTreePoint` | 0 | points | Master points per master level. |
| `MasterSkillTreeMaxLevel` | 0 | level | Maximum master level. |

## Gens System Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `GensSystemSwitch` | 0 | 0, 1 | `1` = Gens on. |
| `GensSystemGuildLock` | 0 | 0, 1 | `1` = guild members must share the same Gens family; players without Gens cannot join guilds. |
| `GensSystemPartyLock` | 0 | 0, 1 | `1` = party members must share the same Gens family. |
| `GensSystemInsertMinLevel` | 0 | level | Minimum level to join a Gens family. |
| `GensSystemContributionFloodTime` | 0 | seconds | Killing the same player again within this time gives no contribution. |
| `GensSystemVictimContributionDecrease` | 0 | points | Base contribution lost by the victim. |
| `GensSystemKillerContributionIncrease` | 0 | points | Base contribution won by the killer. |
| `GensSystemVictimMinContributionDecrease` / `...MaxContributionDecrease` | 0 / 0 | points | Limits of the victim's loss. |
| `GensSystemKillerMinContributionIncrease` / `...MaxContributionIncrease` | 0 / 0 | points | Limits of the killer's gain. |
| `GensSystemStartRewardDay` / `GensSystemFinalRewardDay` | 0 / 0 | day of month | Gens rank rewards can be claimed between these days of each month. |

## Helper Settings

The in-game MU Helper (online).

| Key | Default | Values | Description |
|---|---|---|---|
| `HelperSwitch` | 0 | 0, 1 | `1` = MU Helper allowed. |
| `HelperActiveLevel` | 0 | level | Minimum level to use it. |
| `HelperActiveDelay` | 0 | minutes | Interval at which the helper fee is charged. |
| `HelperActiveMoney1..5` | 0 | zen per level | Fee per charge for stages 1 to 5. The stage goes up every 200 minutes of continuous use; after stage 5 (about 16 hours) the helper turns itself off. Charged = (level + master level) × this value. If the player cannot pay, the helper stops. |

## MU HELPER OFFLINE

The **OFF HELPER** button: the character keeps hunting with its MU Helper settings after the game is closed, like offline attack. The client button is controlled separately by `HelperActiveOfflineSystem` in the client settings (**Client Hub → Client settings**); both must be on. See [Client](../client/README.md).

| Key | Default | Values | Description |
|---|---|---|---|
| `HelperOfflineSwitch` | 0 | 0, 1 | Turn the system on. |
| `HelperOfflineMinLevel` | 0 | level | Minimum level to use it (`HelperActiveLevel` also applies). |
| `HelperOfflineTime_AL0..3` | 0 | minutes, 0 = unlimited | Maximum offline time. `60` = one hour, `720` = twelve hours. With 0 it lasts until the player logs back in or the server restarts. |
| `HelperOfflineDelay_AL0..3` | 1000 | ms, minimum 400 | Minimum time between two attacks. A floor, not a fixed speed: the character never attacks faster than its own agility allows. Lower = faster. Values under 400 are raised to 400 at startup (with a log line) to avoid speed-hack false positives. Shipped: 1200 / 1100 / 1000 / 900. |
| `HelperOfflineAutoPotionDelay_AL0..3` | 1500 | ms, minimum 200 | Minimum time between two potions. The HP % that triggers a potion comes from the player's MU Helper panel. |
| `HelperOfflineAutoBuffDelay_AL0..3` | 30000 | ms, minimum 1000 | How often buffs are recast. Which buffs comes from the MU Helper panel. |
| `HelperOfflineExperienceRate_AL0..3` | 0 | percent, 0 = inherit | Experience while offline. `100` = normal, `50` = half. `0` = use `OffAttackExperienceRate_AL*`. |

## Cash Shop Settings

Reload with `/reload cashshop`.

| Key | Default | Values | Description |
|---|---|---|---|
| `CashShopSwitch` | 0 | 0, 1 | `1` = in-game cash shop (X key) on. |
| `CashShopScriptVersion1..3` | 0 | number | Version of the cash shop item script. Must match the version in the client's cash shop files, or the shop shows nothing. Shipped: 512 / 2011 / 6. |
| `CashShopBannerVersion1..3` | 0 | number | Version of the cash shop banner files. Must match the client. Shipped: 583 / 2010 / 5. |

See [Cash Shop](../data/cash-shop.md) for the products.

## Party Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `PartyReconnectTime` | 0 | seconds | A player who disconnects keeps their party slot for this long and rejoins automatically on login. |
| `PartyMaxMembers` | 5 | 2 to 10 | Maximum party size. Values above 10 are capped. |
| `PartyGeneralExperience1..5` | 0 | percent | Experience bonus for a party of 1 to 5 members with fewer than 3 different classes. |
| `PartySpecialExperience1..5` | 0 | percent | Experience bonus for a party of 1 to 5 members with 3 or more different classes. |
| `PartyDisablePK` | 0 | 0, 1 | `1` = party members cannot damage each other. |

If you raise `PartyMaxMembers` above 5, add `PartyGeneralExperience6..10` and `PartySpecialExperience6..10` (they default to 0, which gives no experience to bigger parties).

## Party Voice

Voice chat for party members. Needs the [VoiceServer](../servers/relay-and-voice.md#voiceserver) running.

| Key | Default | Values | Description |
|---|---|---|---|
| `PartyVoiceEnabled` | 0 | 0, 1 | `1` = party voice on. |
| `PartyVoiceTokenSecret` | (empty) | text, 32+ random characters | Signs the voice tickets. Must be **identical** to `TokenSecret` in `VoiceServer.ini`. Never share it. |
| `PartyVoiceWsUrl` | (empty) | `ws://<public-ip>:<port>` | Address clients use to reach the VoiceServer. The port must equal `VoiceServerPort`. |

```ini
PartyVoiceEnabled = 1
PartyVoiceTokenSecret = <your own long random secret>
PartyVoiceWsUrl = ws://192.0.2.10:55600
```

## Potion Settings

Recovery in percent of the maximum.

| Key | Default | Values | Description |
|---|---|---|---|
| `ApplePotionRate` | 0 | percent | HP recovered by Apple. |
| `SmallLifePotionRate`, `MidleLifePotionRate`, `LargeLifePotionRate` | 0 | percent | HP recovered by small, medium and large healing potions. |
| `SmallManaPotionRate`, `MidleManaPotionRate`, `LargeManaPotionRate` | 0 | percent | Mana recovered by mana potions. |
| `SmallShieldPotionRate`, `MidleShieldPotionRate`, `LargeShieldPotionRate` | 0 | percent | SD recovered by shield potions. |
| `SmallCompoundPotionRate1` / `...Rate2` | 0 | percent | Small complex potion: HP (1) and SD (2). |
| `MidleCompoundPotionRate1` / `...Rate2` | 0 | percent | Medium complex potion. |
| `LargeCompoundPotionRate1` / `...Rate2` | 0 | percent | Large complex potion. |
| `EliteLifePotionRate`, `EliteManaPotionRate`, `EliteShieldPotionRate` | 0 | percent | Elite potions. |
| `DisablePotionCountDecrease` | 0 | 0, 1 | `1` = potions are not consumed when used (infinite stacks). |

## Transformation Ring Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `TransformationRing1..6` | 0 | monster index | Monster each transformation ring turns the player into (index from `Monster.txt`). Shipped: 2, 7, 14, 8, 9, 41. |

## Jewel Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `SoulSuccessRate_AL0..3` | 0 | percent | Jewel of Soul success (+6 to +9). |
| `LifeSuccessRate_AL0..3` | 0 | percent | Jewel of Life success. |
| `HarmonySuccessRate_AL0..3` | 0 | percent | Jewel of Harmony success. |
| `SmeltStoneSuccessRate1_AL0..3` | 0 | percent | Lower Refining Stone success (upgrading a Harmony option). |
| `SmeltStoneSuccessRate2_AL0..3` | 0 | percent | Higher Refining Stone success. |
| `AddLuckSuccessRate1_AL0..3` | 0 | percent | Bonus to Jewel of Soul when the item has Luck. |
| `AddLuckSuccessRate2_AL0..3` | 0 | percent | Bonus to the +10 to +15 Chaos Machine mix when the item has Luck. |

## Fruit Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `FruitAddPointMin` / `FruitAddPointMax` | 0 / 0 | points | Range of stat points added by a fruit. |
| `FruitAddPointSuccessRate_AL0..3` | 0 | percent, -1 = standard | Success chance. `-1` uses the standard formula (lower chance as more fruits are used). |
| `FruitSubPointMin` / `FruitSubPointMax` | 0 / 0 | points | Range of points removed by a minus fruit. |
| `FruitSubPointSuccessRate_AL0..3` | 0 | percent, -1 = standard | Success chance of minus fruits. |

## HP Bonus System

Extra life based on master resets.

| Key | Default | Values | Description |
|---|---|---|---|
| `HpBonus_AL0..3` | 0 | HP per master reset | Extra max HP = master resets × this value. |
| `HpBonus_MAX` | 0 | HP | Cap of the bonus. |

## Die System

| Key | Default | Values | Description |
|---|---|---|---|
| `DieUserSwitch` | 0 | 0, 1 | `1` = a chat message tells who killed whom in PvP. |
| `DieMonsterSwitch` | 0 | 0, 1 | `1` = a chat message tells the player which monster killed them. |

## Online User System

| Key | Default | Values | Description |
|---|---|---|---|
| `OnlineGmSwitch` | 0 | 0, 1 | `1` = announce when a GM logs in. |
| `OnlineUserSwitch` | 0 | 0, 1 | `1` = announce when a player logs in. |

## Disable exp message (Premium)

| Key | Default | Values | Description |
|---|---|---|---|
| `DisableExpMessage` | 0 | 0, 1 | `1` = no "experience gained" message per kill; a message appears only on level up. |

## Remover Recuo (Premium)

Removes the knockback (push) of skills.

| Key | Default | Values | Description |
|---|---|---|---|
| `DisableLightningEffect` | 0 | 0, 1 | `1` = no knockback from Lightning. |
| `DisableCycloneEffect` | 0 | 0, 1 | `1` = no knockback from Cyclone. |
| `DisableTwistEffect` | 0 | 0, 1 | `1` = no knockback from Twisting Slash. |
| `DisableLungEffect` | 0 | 0, 1 | `1` = no knockback from Lunge. |
| `DisableFallingEffect` | 0 | 0, 1 | `1` = no knockback from Falling Slash. |
| `DisableUpperCutEffect` | 0 | 0, 1 | `1` = no knockback from Uppercut. |

## Disable Wings Class Mix

| Key | Default | Values | Description |
|---|---|---|---|
| `DisableWingMixDL` | 0 | 0, 1 | `1` = the Chaos Machine cannot create the Dark Lord cape. |
| `DisableWingMixSU` | 0 | 0, 1 | `1` = it cannot create Summoner wings. |
| `DisableWingMixRF` | 0 | 0, 1 | `1` = it cannot create the Rage Fighter cloak. |

## Disconnect Online Account

| Key | Default | Values | Description |
|---|---|---|---|
| `DisconnectOnlineAccount` | 0 | 0, 1 | `1` = logging into an account that is already online kicks the old session (instead of refusing the new login). |

## Enable CS Skills in All Maps

| Key | Default | Values | Description |
|---|---|---|---|
| `EnableCsSkillsAllMaps` | 0 | 0, 1 | `1` = Castle Siege skills can be used on every map, not only Castle Siege and Land of Trials. |

## Enable Combo to all Classes

| Key | Default | Values | Description |
|---|---|---|---|
| `EnableComboToAllSwitch` | 0 | 0, 1 | `1` = every class can perform skill combos, with the sequences defined in `Data/Custom/CustomCombo.txt`. See [Combo](../customs/combo.md). |

## Reward Online Users

Pays coins to every player periodically while online.

| Key | Default | Values | Description |
|---|---|---|---|
| `OnlineRewardOfflineSystems` | 0 | 0, 1 | `1` = players in offline attack or offline helper also receive the reward. |
| `OnlineRewardCoin1Delay` | 0 | minutes | Interval for Coin1 (WCoinC). |
| `OnlineRewardCoin1Value_AL0..3` | 0 | amount | Coin1 paid each interval. `0` = none. |
| `OnlineRewardCoin2Delay` | 0 | minutes | Interval for Coin2 (WCoinP). |
| `OnlineRewardCoin2Value_AL0..3` | 0 | amount | Coin2 paid each interval. |
| `OnlineRewardCoin3Delay` | 0 | minutes | Interval for Coin3 (Goblin Points). |
| `OnlineRewardCoin3Value_AL0..3` | 0 | amount | Coin3 paid each interval. Shipped: 5 / 10 / 15 / 20 every 15 minutes. |

{% hint style="warning" %}
A delay of `0` with a value above 0 pays on every check (about once per second). Always set a delay when you set a value.
{% endhint %}

## Infinity Arrow Switch

| Key | Default | Values | Description |
|---|---|---|---|
| `InfinityArrow_AL0..3` | 0 | 0, 1 | `1` = elves of this account level do not consume arrows even without the Infinity Arrow buff. |

---

## Optional keys (not in the shipped file)

The server also reads these keys. Add them to `[GameServerInfo]` if you need them.

| Key | Default | Values | Description |
|---|---|---|---|
| `RequireHardwareId` | 1 | 0, 1 | Same as in `ConnectServer.ini`, applied at the GameServer: `1` = refuse clients without a valid hardware ID. |
| `PartyGeneralExperience6..10`, `PartySpecialExperience6..10` | 0 | percent | Party bonus for 6 to 10 members (see Party Settings). |
| `ALQLevelUpPoint` | 1 | points | Level-up points of the Alchemist class (newer clients only). |

## Example

A free server with 200x experience, VIP tiers slightly better, PK drops off and the offline helper limited to 12 hours for free accounts:

```ini
[GameServerInfo]
DefaultLanguageSystem = 0

ServerName = Main
ServerCode = 0
ServerLock = 0
ServerPort = 55901
ServerMaxUserNumber = 300

DataServerAddress = 127.0.0.1
DataServerPort = 55960
JoinServerAddress = 127.0.0.1
JoinServerPort = 55970
ConnectServerAddress = 127.0.0.1
ConnectServerPort = 55557

MaxIpConnection = 100
MaxHWIDConnection = 5

AddExperienceRate_AL0 = 200
AddExperienceRate_AL1 = 220
AddExperienceRate_AL2 = 230
AddExperienceRate_AL3 = 240

ItemDropTime = 30
ItemDropRate_AL0 = 10
ItemDropRate_AL1 = 15
ItemDropRate_AL2 = 20
ItemDropRate_AL3 = 25

PkItemDropSwitch = 0

HelperOfflineSwitch = 1
HelperOfflineTime_AL0 = 720
HelperOfflineTime_AL1 = 0
HelperOfflineTime_AL2 = 0
HelperOfflineTime_AL3 = 0
```

## Rules and pitfalls

- **Missing keys are 0.** Removing a line does not restore a sensible value: `MonsterLifeRate`, `AddExperienceRate_AL*`, `ItemDropRate_AL*` and most rates become 0 (no life, no experience, no drops).
- **`MaxIpConnection = 0` blocks everyone.** It is not "unlimited".
- **Identity must match everywhere:** `ServerCode` with `ServerList.dat` and `MapServerInfo.dat`; `ServerPort` with `ServerList.dat`; `ServerVersion`/`ServerSerial` with the client; `PartyVoiceTokenSecret` with `VoiceServer.ini`.
- **Running two GameServers:** each needs a different `ServerCode` and `ServerPort`. See [Multiple GameServers](multiple-gameservers.md).
- **Startup-only keys** (marked restart) do nothing until you restart, even after `/reload common`.
- **Do not put comments after text values** (`PartyVoiceWsUrl`, `ServerName`): they become part of the value.
