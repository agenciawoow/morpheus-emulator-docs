# Client settings reference

Every client setting and connection setting edited in **Client Hub → Client settings**, what it does, its default and the value shipped.

## What it is

The **client settings** hold the client's feature switches and options (menus, interface, MU Helper, character attack speed caps, Discord presence, login buttons). The **connection settings** hold how the client connects and how its window looks. Both are edited in [Client Hub → Client settings](../../dashboard/client-hub/client-settings.md) and reach the players when you [publish an update from the Client Hub](../../dashboard/client-hub/publishing-updates.md). Players never edit them.

In the tables below:

- **Default** = value used when the key is missing (not set).
- **Shipped** = value in the client settings delivered with the server files.
- Keys are grouped by section (`[Custom]`, `[Interface]`, ...), shown in the headings below.

{% hint style="warning" %}
Rules that bite when the settings are edited as text:
- A key is only read under its **own section header** (`[Custom]`, `[Interface]`, ...). A key placed under the wrong header is silently ignored and falls back to its default.
- Lines starting with `;` are comments. A line starting with `//` is **not** a comment for this format; it is just an invalid line, and keys below it still belong to the section above.
- New sections must go at the **end** of the settings. Inserting a header in the middle "captures" every key below it.
{% endhint %}

## Enable it

Every switch in this page takes effect only after you save it and publish it to players from the Client Hub (see [Reload module](#reload-module)). Server-side counterparts are listed in [Client-side customs](README.md) (table "Server custom <-> client custom that must match").

## Settings

### Client settings - general

| Key | Default | Shipped | Meaning |
|---|---|---|---|
| `ClientName` | - | `Main.exe` | Informational only. Not used when the client is published. |
| `MaxOfInstance` | `2` | `5` | Maximum number of game windows a player can open at the same time on one PC. |
| `PluginAntiHack` | empty | empty | Optional plugin file name loaded by the client. Leave empty unless your installer provides one. |
| `PluginReShader` | empty | empty | Optional shader plugin file name. Leave empty unless your installer provides one. |
| `PathExport` | empty | `<client-folder>` | Client folder where the generated client data is written before it is published. If empty here, the value from the connection settings is used. Set by your installer; normally you never change it. |

### Client settings - [HelperInfo]

| Key | Default | Shipped | Meaning |
|---|---|---|---|
| `HelperActiveAlert` | `0` | `1` | 1 = show a notice when the character reaches `HelperActiveLevel` (MU Helper unlocked). |
| `HelperActiveLevel` | `80` | `80` | Minimum character level to open/start the MU Helper on the client. The server has its own `HelperActiveLevel` in `GameServerInfo - Common.dat`. |
| `HelperActiveOfflineSystem` | `0` | `1` | MU Helper Offline: 1 = shows the "OFF HELPER" button so the character keeps hunting after the game is closed. Also needs `HelperOfflineSwitch = 1` in `GameServerInfo - Common.dat`. See [Server features](../customs/server-features.md). |

### Client settings - [ReconnectInfo]

| Key | Default | Shipped | Meaning |
|---|---|---|---|
| `ReconnectTime` | `0` | `300000` | Auto-reconnect after a disconnection. `0` = off. Any value above 0 turns it on and is used as the reconnect time window (shipped `300000`, i.e. 5 minutes if read as milliseconds). |

### Client settings - [CharacterInfo]

Client-side attack speed cap per class (attack speed grows with Agility up to this value).

| Key | Default | Shipped | Class |
|---|---|---|---|
| `DWMaxAttackSpeed` | `65000` | `67000` | Dark Wizard |
| `DKMaxAttackSpeed` | `65000` | `65000` | Dark Knight |
| `FEMaxAttackSpeed` | `65000` | `67000` | Fairy Elf |
| `MGMaxAttackSpeed` | `65000` | `67000` | Magic Gladiator |
| `DLMaxAttackSpeed` | `65000` | `67000` | Dark Lord |
| `SUMaxAttackSpeed` | `65000` | `67000` | Summoner |
| `RFMaxAttackSpeed` | `65000` | `67000` | Rage Fighter |

### Client settings - [Custom]

| Key | Default | Shipped | Meaning |
|---|---|---|---|
| `PrintLogo` | `0` | `0` | Logo stamped on screenshots (`Data\Interface\HUD\MU-logo.OZT`). 0 = off, 1 = top-left, 2 = top-right, 3 = bottom-left, 4 = bottom-right, 5 = center. |
| `RemoveClass` | `0` | `0` | Hide classes on the character creation screen. 0 = none, 1 = hide Summoner/Dark Lord/Rage Fighter, 2 = hide Summoner/Rage Fighter, 3 = hide Rage Fighter. |
| `FpsRenderMax` | `30` | `120` | FPS limit. |
| `FpsWindowsOption` | `1` | `1` | 1 = show the FPS notice when entering the game. |
| `RenderGuildLogo` | `0` | `1` | 1 = show the guild logo next to chat messages. |
| `MaxItemLevelOn` | `15` | `15` | Highest item level the client allows in Chaos Machine mixes (10 to 15). Mixes above this level are blocked in the client. |
| `ShutdownLevelWing3` | `0` | `0` | 1 = block the Feather of Condor and 3rd-level wing mixes in the client. |
| `EnableTradeCoin` | `0` | `1` | 1 = coins can be added to a trade. |
| `EnableRememberAccount` | `0` | `1` | 1 = "remember account" option on the login screen. |
| `EnableJewelOfInventory` | `0` | `1` | 1 = Jewel Bank window. |
| `EnableWindowsResetInfo` | `0` | `0` | 1 = reset information window. |
| `EnableCsSkillsAllMaps` | `0` | `1` | 1 = Castle Siege skills usable on every map. Must match `EnableCsSkillsAllMaps` in `GameServerInfo - Common.dat`. |
| `RankUserSwitch` | `0` | `4` | How the player rank (Custom Rank User) is drawn: 0 = everything, 1 = hide stats, 2 = hide logo, 3 = hide stats and logo, 4 = logo on top, 5 = off. |
| `RankUserShowOverHead` | `0` | `1` | 1 = draw the rank over the character's head. |
| `RankUserOnlyOnSafeZone` | `1` | `1` | 1 = only draw the rank inside safe zones. |
| `RankUserShowReset` | `1` | `1` | 1 = include resets in the rank info. |
| `RankUserShowMasterReset` | `1` | `1` | 1 = include master resets in the rank info. |
| `RankUserNeedAltKey` | `0` | `0` | 1 = only show the rank while Alt is held. |
| `PartyHpBarType` | `1` | `0` | Party HP bar: 0 = normal, 1 = icon, 2 = off. |
| `CustomMenuSwitch` | `0` | `1` | 1 = custom menu button. |
| `CustomMenuType` | `0` | `2` | 1 = logo texture button, 2 = button next to the MU Helper. |
| `EnableVipShopButton` | `0` | `0` | Menu button: Buy VIP window ([Buy VIP](../customs/buy-vip.md)). |
| `EnableRankingButton` | `0` | `1` | Menu button: rankings. |
| `EnableCommandButton` | `0` | `1` | Menu button: command list (from the client custom file `CustomCommand.txt`). |
| `EnableOptionButton` | `0` | `1` | Menu button: options. |
| `EnableEventTimeButton` | `0` | `1` | Menu button: event timer. |
| `CustomBattlePassSwitch` | `0` | `1` | 1 = Battle Pass available in the client. With 0 the whole pass is hidden. Also needs `CustomBattlePassSwitch = 1` in `GameServerInfo - Custom.dat`. See [Battle Pass](../customs/battle-pass.md). |
| `EnableMenuEventSchedule` | `1` | `1` | Burger menu tile: event schedule. |
| `EnableMenuDailyBoss` | `1` | `1` | Burger menu tile: daily boss. |
| `EnableMenuTradeMarket` | `1` | `0` | Burger menu tile: trade market. |
| `EnableMenuShop` | `1` | `1` | Burger menu tile: shop. |
| `EnableMenuEventRoulette` | `1` | `0` | Burger menu tile: event roulette. |
| `EnableMenuChangeItemOpt` | `1` | `0` | Burger menu tile: change item option. |
| `EnableMenuCustomShortcutCommand` | `0` | `1` | 1 = "Quick commands" card in the burger menu, with the buttons from the client custom file `CustomShortcutButton.txt`. With 0 that file is not even published to the client. |
| `EnableMenuCustomBattlePass` | `1` | `1` | Burger menu tile for the Battle Pass (only used when `CustomBattlePassSwitch = 1`). |
| `CustomMorpheusShopSwitch` | `0` | `1` | 0 = legacy cash shop, 1 = Morpheus Shop (new HTML shop). Needs the client custom file `CustomMorpheusShop.txt` and `CustomMorpheusShopSwitch = 1` in `GameServerInfo - Custom.dat`. See [Morpheus Shop](../customs/morpheus-shop.md). |
| `EnableMorpheusShopCustomOptions` | `1` | `1` | 1 = clicking Buy opens a popup with sliders for Level/Luck/Skill/Additional/380/Excellent/Set/Socket, limited and priced by the server's `CustomMorpheusShop.txt` sections 1 and 2. 0 = buy the base item directly. |
| `EnableWindowsVipShop` | `0` | `0` | Custom window: Buy VIP. |
| `EnableWindowsRankTop` | `0` | `1` | Custom window: rankings. |
| `EnableWindowsCommand` | `0` | `1` | Custom window: command list. |
| `EnableWindowsEventTime` | `0` | `1` | Custom window: event timer. |
| `SwitchPersonalShopType` | `0` | `1` | Personal store type: 0 = normal, 1 = premium. |
| `EnableOffStoreButtom` | `0` | `1` | Personal store button: offline store (only when `SwitchPersonalShopType = 0`). |
| `EnableStoreBlessButtom` | `0` | `1` | Personal store button: sell for Jewel of Bless. |
| `EnableStoreSoulButtom` | `0` | `1` | Personal store button: sell for Jewel of Soul. |
| `EnableStoreChaosButtom` | `0` | `1` | Personal store button: sell for Jewel of Chaos. |
| `EnableStoreCoin1Buttom` | `0` | `1` | Personal store button: sell for coin 1 (WCoinC). |
| `EnableStoreCoin2Buttom` | `0` | `1` | Personal store button: sell for coin 2 (WCoinP). |
| `EnableStoreCoin3Buttom` | `0` | `1` | Personal store button: sell for coin 3 (GoblinPoint). |
| `SceneLogin` | `0` | `3` | Login scene: 0 = normal (map 74), 1 = S1, 2 = S2/S3 (map 56), 3 = S4 (map 78), 4 = S13 (map 95). |
| `SceneCharacter` | `0` | `0` | Character selection scene: same values as `SceneLogin` (0 = map 75, 2 = map 55, 3 = map 79, 4 = map 94). |
| `CustomFullMapSwitch` | `0` | `1` | 1 = custom markers (NPC, gate, spot, boss, event, quest, vault, point) on the full map (Tab) and minimap, from the client custom file `CustomFullMap.txt`. With 0 that file is not published to the client. |

### Client settings - [Interface]

| Key | Default | Shipped | Meaning |
|---|---|---|---|
| `ServerDefaultName` | `1` | `0` | Server name source: 0 = from the ConnectServer list, 1 = from `ServerList.bmd`. |
| `InterfaceMobileSwitch` | `1` | `1` | Android/iOS only. 1 = modern WebView panels (inventory, jewel bank, character, guild, party, commands, pet, quest), 0 = classic native windows. |
| `MobileBtnCombo` | `1` | `1` | Android/iOS only. Show the Auto Combo button. |
| `MobileBtnAttack` | `1` | `1` | Android/iOS only. Show the Auto Attack button. |
| `MobileBtnPick` | `1` | `1` | Android/iOS only. Show the Auto Pick button. |
| `MobileBtnCtrl` | `1` | `1` | Android/iOS only. Show the Ctrl button. |
| `NPCShopWebview` | `1` | `1` | Mobile: NPC shop (buy/sell/repair) as a WebView panel (0 = native). Needs `InterfaceMobileSwitch = 1`. |
| `StorageWebview` | `1` | `1` | Mobile: vault/storage as WebView. |
| `NpcDialogWebview` | `1` | `1` | Mobile: every other NPC window (dialogs, quests, Lucky Coin, Chaos Machine NPCs, guild creation) **and** the MU Helper settings, as WebView. One switch for the whole group. |
| `PersonalShopWebview` | `1` | `1` | Mobile: personal store as WebView. |
| `TradeWebview` | `1` | `1` | Mobile: trade window as WebView. |
| `MessageBoxWebview` | `1` | `1` | Mobile/web: confirmation boxes, text-input boxes and NPC menus as WebView. |
| `MasterSkillTreeWebview` | `1` | `1` | Mobile/web: master skill tree as WebView. |
| `NpcEventWebview` | `1` | `1` | Mobile/web: event entry screens (Blood Castle, Devil Square, Illusion Temple, Doppelganger, Imperial Guardian, Kanturu) as WebView. |
| `ScreenType` | `0` | `2` | Screen size: 0 = normal, 1 = widescreen height, 2 = full widescreen. |
| `LookAndFeel` | `0` | `0` | Interface style: 0 = original S6, 1 = S1 (needs `HUD/Look-1/`), 2 = S2/S3 (`HUD/Look-2/`), 3 = Ex700 S16 (`HUD/Look-3/`), 4 = Legends (`HUD/Look-4/`). |
| `CharInfo` | `0` | `1` | Character selection balloon: 0 = normal, 1 = show resets (needs `character_ex(1).ozt`), 2 = resets + master resets (`character_ex(2).ozt`). |
| `ChatEmoticon` | `0` | `1` | 0 = off, 1 = emoticons, 2 = block private whisper, 3 = both (needs `newui_chat_back.OZJ`). |
| `InventoryExtension` | `0` | `1` | 1 = inventory expansion. |
| `MaxInventoryExtension` | `2` | `4` | Number of inventory expansions (max 4). |
| `WareHouseExtension` | `0` | `1` | 1 = extra vaults. |
| `CommandWareNumber_AL0` | `0` | `5` | Extra vaults (besides the main one) for free accounts. Must equal `CommandWareNumber_AL0` in `GameServerInfo - Command.dat`. 0 = no vault selector. |
| `CommandWareNumber_AL1` | `0` | `2` | Same, VIP 1. |
| `CommandWareNumber_AL2` | `0` | `3` | Same, VIP 2. |
| `CommandWareNumber_AL3` | `0` | `4` | Same, VIP 3. |
| `CashShopInGame` | `0` | `1` | 1 = in-game item shop window (legacy cash shop). |
| `OficialMuHelper` | `0` | `1` | 1 = MU Helper interface. |
| `MasterSkill` | `0` | `1` | 1 = master skill tree interface. |
| `InvasionInfoTime` | `0` | `1` | 1 = show invasion time and monsters killed. |
| `RenderItemToolip` | `0` | `1` | 1 = new (SS18 style) item tooltip. |
| `RenderItemNameDrop` | `0` | `1` | 1 = names of dropped items in a box. |
| `RenderCharacterName` | `0` | `1` | 1 = new character/monster name plates. |
| `RenderEquipmentCostume` | `0` | `0` | 1 = costume/wing core equipment slot. |
| `RenderEquipmentPetMuun` | `0` | `0` | 1 = Muun pet equipment slot. |
| `RenderEquipmentPetEagle` | `0` | `1` | 1 = eagle pet equipment slot. |
| `RenderEquipmentEarring` | - | `0` | Not read (earring slot disabled in this version). |
| `RenderEquipmentFlagNat` | - | `0` | Not read (flag slot disabled in this version). |

### Client settings - [DiscordPresence]

Shows the character in the player's Discord profile. The application and its images must be registered in the Discord Developer Portal.

| Key | Default | Shipped | Meaning |
|---|---|---|---|
| `DiscordPresenceEnable` | `0` | `1` | 1 = Discord Rich Presence on. |
| `DiscordPresenceCharacterInfoType` | `4` | `4` | 0 = off, 1 = name, 2 = + class, 3 = + level, 4 = + resets, 5 = + master resets. |
| `DiscordPresenceShowGuild` | `1` | `1` | Show the guild. |
| `DiscordPresenceLocationType` | `2` | `2` | 0 = off, 1 = map, 2 = map + party. |
| `DiscordPresenceShowPlatform` | `1` | `1` | Show PC or Mobile. |
| `DiscordPresenceShowTime` | `1` | `1` | Show session time. |
| `DiscordPresenceShowClassImage` | `1` | `1` | Show the class icon. |
| `DiscordPresenceApplicationID` | empty | your ID | Your Discord application ID. |
| `DiscordPresenceDescription` | empty | your text | Text line under the game name. |
| `DiscordPresenceLargeImageKey` | empty | your key | Image key registered in your Discord application. |
| `DiscordPresenceButtonPlay` | `0` | `1` | 1 = first button. |
| `DiscordPresenceButtonPlayText` | empty | `Crie sua Conta` | First button label (in-game text, translate as you like, e.g. "Create your account"). |
| `DiscordPresenceButtonPlayURL` | empty | `<your-website>` | First button link. |
| `DiscordPresenceButtonWebsite` | `0` | `0` | 1 = second button. |
| `DiscordPresenceButtonWebsiteText` | empty | your text | Second button label. |
| `DiscordPresenceButtonWebsiteURL` | empty | `<your-website>` | Second button link. |

Discord allows at most 2 buttons, each with a valid text and URL.

### Client settings - [SocialLogin]

| Key | Default | Shipped | Meaning |
|---|---|---|---|
| `GoogleButtonRegisterSwitch` | `1` | `0` | 0 = hide the Google button on every platform, 1 = show it (respecting the Type below). |
| `GoogleButtonRegisterType` | `0` | `1` | 0 = all platforms, 1 = all but iPhone/iPad, 2 = desktop and web only. |
| `CreateAccountEnabled` | `1` | see pitfall | 0 = hide the "Create account" button everywhere. |
| `CreateAccountType` | `1` | see pitfall | 1 = register inside the game, 2 = open `CreateAccountURL` in the browser. |
| `CreateAccountURL` | empty | see pitfall | Registration page (used with Type 2). |
| `SiteURL` | empty | see pitfall | "Site" button on the login footer. Empty = hidden. |
| `SupportURL` | empty | see pitfall | "Support" button on the login footer. Empty = hidden. |

{% hint style="warning" %}
In the shipped client settings the `CreateAccount*`, `SiteURL` and `SupportURL` keys sit inside `[Interface]` (after a `//` line). They are read from `[SocialLogin]`, so as shipped they are ignored and the defaults apply. Move them under `[SocialLogin]` at the end of the settings.
{% endhint %}

### Connection settings

| Key | Meaning |
|---|---|
| `CustomerName` | Your customer name. Set by your installer. If empty, the client connection data is not generated. |
| `LicenseID` | Set by your installer. Must be the same value the server uses. Do not change or share it. |
| `IpAddress` | `<your-server-ip>`: public address of your ConnectServer. |
| `IpAddressPort` | `<connectserver-port>`: client port of your ConnectServer. |
| `ClientVersion` | Client version string. Must match the version expected by your server (e.g. `1.04.05`). |
| `ClientSerial` | Set by your installer. Keep secret. |
| `WindowName` | Brand shown as the window/app name. |
| `WindowFormat` | Window title template. Variables: `{$Name}`, `{$Level}`, `{$MasterLevel}`, `{$Resets}`, `{$MasterResets}`, `{$Gold}`, `{$Coin1}`, `{$Coin2}`, `{$Coin3}`, `{$fps}`, `{$Brand}`. Wrap a number in `COMMIFY(...)` for thousands separators. |
| `ScreenShotPath` | File name pattern for screenshots, e.g. `ScreenShots\Screen(%02d_%02d-%02d-%02d)-%04d.jpg`. |
| `PathExport` | Client folder where the generated client data is written (same as in the client settings; either one is enough). |
| `MainExePath` / `MainExeCRC32` | Set by your installer. Do not edit. |

## Example

```ini
; Connection settings
CustomerName = <set-by-installer>
LicenseID = <set-by-installer>
IpAddress = <your-server-ip>
IpAddressPort = <connectserver-port>
ClientVersion = 1.04.05
WindowName = [MU] MyServer
WindowFormat = {$Brand} | {$Name} - Level: {$Level} | Reset: {$Resets} | Zen: {COMMIFY($Gold)} | FPS: {$fps}
ScreenShotPath = ScreenShots\Screen(%02d_%02d-%02d-%02d)-%04d.jpg
PathExport = <client-folder>
```

```ini
; Client settings - turn on Battle Pass, Morpheus Shop and MU Helper Offline
[HelperInfo]
HelperActiveOfflineSystem = 1     ; + HelperOfflineSwitch = 1 in GameServerInfo - Common.dat

[Custom]
CustomBattlePassSwitch = 1        ; + CustomBattlePassSwitch = 1 in GameServerInfo - Custom.dat
EnableMenuCustomBattlePass = 1
CustomMorpheusShopSwitch = 1      ; + CustomMorpheusShopSwitch = 1 in GameServerInfo - Custom.dat
EnableMorpheusShopCustomOptions = 1

; ... other sections ...

[SocialLogin]                     ; always the LAST section
GoogleButtonRegisterSwitch = 0
GoogleButtonRegisterType = 1
CreateAccountEnabled = 1
CreateAccountType = 2
CreateAccountURL = https://<your-website>/register
SiteURL = https://<your-website>/
SupportURL = https://<your-website>/support
```

## Pitfalls

- Key under the wrong section = silently ignored (default used). Typical case: `CreateAccount*`/`SiteURL`/`SupportURL` under `[Interface]`, or `CustomFullMapSwitch` placed under `[SocialLogin]`; it must be in `[Custom]`.
- `CommandWareNumber_AL*` must be the same numbers as in `GameServerInfo - Command.dat`. Shipped: client `AL0 = 5`, server `AL0 = 1`; free accounts see 5 vault buttons but only 1 works.
- Client-only switches do not enable server features. Battle Pass, Morpheus Shop, MU Helper Offline and CS skills on all maps need the matching server key too.
- `HelperActiveLevel` exists on both sides with different meanings/values (client 80, server 1). The client one only gates the client UI.
- The `Render*` and `*Webview` keys are spelled exactly as listed (including `RenderItemToolip` and the `...Buttom` keys). A corrected spelling is a different key and is ignored.
- Never share your connection settings: they contain your license and serial.

## Reload module

No hot reload. After changing client or connection settings, save them in **Client Hub → Client settings**, [publish the update to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md), and have players restart the game. Server-side counterparts are applied with `/reload custom` (`GameServerInfo - Custom.dat`), `/reload common` (`GameServerInfo - Common.dat`) or `/reload command` (`GameServerInfo - Command.dat`).
