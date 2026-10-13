# Server Features (GameServerInfo only)

Custom features that have no data file of their own: they are configured only by keys in `GameServer/Data/GameServerInfo - Custom.dat` or `GameServerInfo - Common.dat`.

Each section below lists what the feature does, its keys (default = value in the shipped file), related commands, pitfalls and how to reload it. As a rule: keys in `Custom.dat` reload with `/reload custom`, keys in `Common.dat` with `/reload common` (both admin commands).

---

## Announce Chaos Mix

### What it is

When a player tries a +10 to +15 item upgrade at the Chaos Machine, everyone receives a notice: "Name congratulations! The item X reached level +12" or "Name we are sorry, the item X failed at level +12".

### Keys (`Custom.dat`)

| Key | Default | Meaning |
|---|---|---|
| `AnnounceChaosMix` | `1` | `0` = off. `1` = notice to this GameServer, translated to each player's language. `2` = notice to every GameServer of the group (sent once, in the server language). |
| `AnnounceChaosMixLevel` | `5` | Upgrades are announced while `target level < 10 + value`. `5` announces +10 to +14; use `6` to include +15; `0` announces nothing. |
| `AnnounceChaosMixSuccess` | `%s congratulations the item %s reached the level +%d` | Fallback text, used only if Lang message 946 is missing. `%s` name, `%s` item, `%d` level. |
| `AnnounceChaosMixFailure` | `%s We are sorry the item %s failed in the level +%d` | Fallback text, used only if Lang message 947 is missing. |

### Pitfalls

- The real texts are `Data/Lang/NNN - Xxx_Message.txt` indexes **946** (success) and **947** (failure). Edit them there; the `.dat` texts are only a fallback.
- Keep the `%s %s %d` order in your translations.

### Reload

`/reload custom` for the keys, `/reload lang` for the Lang texts.

---

## Custom Store (offline store)

### What it is

`/store <type>` opens a personal shop where items are sold for **jewels or coins** instead of Zen: the price you type in the shop is the number of Jewels of Bless, Soul or Chaos, or the amount of WCoinC, WCoinP or GoblinPoint. The shop title is set automatically (".::BLESS STORE::." etc.) and buyers are told the currency when they open it. `/offstore` then lets the seller close the game while the shop stays open (offline store).

### Keys (`Custom.dat`)

| Key | Default | Meaning |
|---|---|---|
| `CustomStoreTime_AL0` .. `_AL3` | `0` | Minutes the custom store stays open, per account level. `0` = unlimited. |
| `CustomStoreCommandJoBSyntax` | `bless` | Word for a Jewel of Bless store (`/store bless`). |
| `CustomStoreCommandJoSSyntax` | `soul` | Word for a Jewel of Soul store. |
| `CustomStoreCommandJoCSyntax` | `chaos` | Word for a Jewel of Chaos store. |
| `CustomStoreCommandWCCSyntax` | `wcc` | Word for a WCoinC store. |
| `CustomStoreCommandWCPSyntax` | `wcp` | Word for a WCoinP store. |
| `CustomStoreCommandWCGSyntax` | `wcg` | Word for a GoblinPoint store. |
| `CustomStoreJoBName` .. `CustomStoreWCGName` | `.::BLESS STORE::.` ... | Shop title shown for each type. |

Other requirements: `PersonalShopSwitch = 1` in `Common.dat`, the map's `CustomStore` column = `1` in `Data/MapManager.txt`, a safe zone, level above 5, and items with a price in the shop.

### Commands

| Command | Who | What |
|---|---|---|
| `/store <type>` | Player | Opens the custom store of that type (type = one of the `*Syntax` words). |
| `/offstore` | Player | With a shop already open in a safe zone, disconnects the client and keeps the shop open offline. |

### Pitfalls

- Set the item prices **before** `/store` (the shop must not be empty).
- Not allowed in Chaos Castle / Illusion Temple maps, nor on maps with `CustomStore = 0` ("You cannot use this in this map!").
- When `CustomStoreTime_AL*` runs out, the shop closes (an offline seller is then removed from the game).
- Logging in again with the same account ends the offline store.

### Reload

`/reload custom`.

---

## Custom Attack (offline attack)

### What it is

`/attack` makes the character attack nearby monsters automatically with a skill, staying around the spot where it started, using potions when HP or mana drops below 50% and (optionally) recasting buffs on itself and party members. `/offattack` then closes the game while the character keeps hunting offline.

### Keys (`Custom.dat`)

| Key | Default | Meaning |
|---|---|---|
| `CustomAttackTime_AL0` .. `_AL3` | `720 / 1440 / 2160 / 2880` | Minutes the auto attack lasts per account level. `0` = unlimited. |
| `CustomAttackDelay` | `0` | Minimum milliseconds between two attacks. |
| `CustomAttackAutoPotionDelay` | `0` | Minimum milliseconds between two potions. |
| `CustomAttackAutoBuff_AL0` .. `_AL3` | `1` | `1` = the account level may use auto-buff (`/attack <skill> 1`). |
| `CustomAttackAutoBuffDelay` | `10000` | Milliseconds between buff recasts. |
| `CustomAttackAutoResume_AL0` .. `_AL3` | `1` | `1` = the attack state (skill, spot, auto-buff, plus auto-reset and PvP-off settings) is saved and restored at the next login. |

Related, in `Common.dat`: `OffAttackExperienceRate_AL0..3` (experience rate while in offline attack). Other requirements: the map's `CustomAttack` column = `1` in `Data/MapManager.txt`, not in a safe zone, MU Helper not running.

### Commands

| Command | Who | What |
|---|---|---|
| `/attack [skill] [buff]` | Player | Starts (or, if already on, stops) the auto attack. `skill` = skill number (omit or `0` = automatic), `buff` = `1` to enable auto-buff. |
| `/offattack` | Player | With `/attack` running outside a safe zone, disconnects the client and keeps attacking offline. |

### Pitfalls

- The character needs the skill; High Elves (3rd-class Elf) also need the Infinity Arrow skill.
- `/offattack` is refused in event maps (Blood Castle, Devil Square, Chaos Castle, Illusion Temple, Doppelganger, Imperial Guardian, Castle Siege, Arca).
- The attack rhythm already follows the character's attack speed; `CustomAttackDelay` only adds a slower floor (`0` = no extra limit).

### Reload

`/reload custom`.

---

## Event Time window

### What it is

A window (H key, or the Event Time button of the custom menu) listing the countdown to every scheduled event: Blood Castle, Devil Square, Chaos Castle, Illusion Temple, Online Lottery, Quiz, invasions, arenas and so on. Event names come from `Data/Event/EventName.dat`.

### Keys

| File | Key | Default | Meaning |
|---|---|---|---|
| `Custom.dat` | `CustomEventTimeSwitch` | `1` | `1` = the server answers the window. `0` = empty window. |
| Client settings (Client Hub) | `EnableEventTimeButton` | `1` | Shows the button in the custom menu. |
| Client settings (Client Hub) | `EnableWindowsEventTime` | `1` | Enables the window. |

### Pitfalls

- Event names on the client come from its own `EventNameDisplay.txt`; keep the indexes equal to `EventName.dat`.

### Reload

`/reload custom` for the switch; `/reload event` for schedules and names. Client keys must be published to players from the Client Hub.

---

## Guild Warehouse

### What it is

A shared vault for the whole guild. A guild member talks to the Guild Master NPC and, if their guild rank allows it, the guild warehouse opens.

### Keys (`Custom.dat`)

| Key | Default | Meaning |
|---|---|---|
| `GuildWarehouseEnable` | `1` | `1` = enabled. `0` = the NPC answers as usual. |
| `GuildWarehouseStatus` | `0` | Minimum guild rank: `0` all members, `32` Battle Master and above, `64` Guild Assistant and above, `128` Guild Master only. |

### Pitfalls

- Members below the required rank get "You are not authorized to use the Guild vault!".

### Reload

`/reload custom`.

---

## Shop MessageBox

### What it is

A confirmation box ("Do you want to buy...?") before buying from an **NPC shop**.

### Keys (`Custom.dat`)

| Key | Default | Meaning |
|---|---|---|
| `CustomShopMessageBox` | `0` | `0` = off. `1` = only for items sold for coins (WCoinC / WCoinP / GoblinPoint). `2` = for every purchase. |

### Pitfalls

- Not related to the [Morpheus Shop](morpheus-shop.md) (the cash shop window).

### Reload

`/reload custom`.

---

## Buy Inventory Extension

### What it is

Players buy the inventory extension rows (up to 4) for WCoinC directly from the inventory window, without an expansion item.

### Keys (`Custom.dat`)

| Key | Default | Meaning |
|---|---|---|
| `SwitchBuyInventoryExt` | `1` | `1` = buying is enabled. |
| `PriceInventoryExt1` .. `PriceInventoryExt4` | `100` | WCoinC price of the 1st, 2nd, 3rd and 4th extension. |
| `message_success` | `1` | **Lang message index** shown after a purchase. `0` = no message. |
| `message_failed` | `1` | Lang message index shown when the purchase is not possible now (dead, trading, window open...). `0` = no message. |
| `message_error` | `1` | Lang message index shown when the player lacks WCoinC. `0` = no message. |

Client: `InventoryExtension = 1` and `MaxInventoryExtension = 4` in the client settings (Client Hub).

### Pitfalls

- The three `message_*` keys are **message numbers** from `Data/Lang`, not on/off switches. As shipped (`1`) they show Lang message 1 ("Blood Castle has closed"). Add your own messages to the Lang files and put their indexes here, or set them to `0`.
- Extensions are bought in order; the price used is the one of the next extension.

### Reload

`/reload custom`.

---

## Chat Anti-Spam

### What it is

Blocks players who repeat the same chat message. After too many identical messages in a short time, the player cannot talk (normal chat and `/post`) for a while.

### Keys (`Custom.dat`)

| Key | Default | Meaning |
|---|---|---|
| `EnableAntiSpam` | `1` | `1` = enabled. |
| `LimitMaxMessage` | `3` | Number of identical consecutive messages that triggers the block (the 3rd repeat is blocked). |
| `LimitMaxTimeMessageCheck` | `60` | Window, in seconds, in which repeats are counted. |
| `TimeBlockedChat` | `1` | Block duration in **minutes**. |

### Pitfalls

- The block message says "You cannot speak for %d seconds", but the number is in minutes. Edit Lang message 269 if this confuses players.
- Only identical messages count; different messages are never blocked.

### Reload

`/reload custom`.

---

## MU Helper Offline

{% embed url="https://www.youtube.com/watch?v=uH3QRCLKqJ8" %}
MU Helper in action: set up the hunt, then let the character keep farming after the player closes the game.
{% endembed %}

### What it is

The "OFF HELPER" button: the character keeps hunting with the player's own MU Helper settings (attack skill, potion %, buffs) after the game is closed, like `/offattack` but configured from the MU Helper panel.

### Keys (`Common.dat`)

| Key | Default | Meaning |
|---|---|---|
| `HelperOfflineSwitch` | `1` | `1` = system enabled. |
| `HelperOfflineMinLevel` | `1` | Minimum character level (the normal `HelperActiveLevel` also applies). |
| `HelperOfflineTime_AL0` .. `_AL3` | `0` | Maximum offline time in minutes. `0` = unlimited. A negative value blocks that account level. |
| `HelperOfflineDelay_AL0` .. `_AL3` | `1200 / 1100 / 1000 / 900` | Minimum milliseconds between attacks (a floor; the character never attacks faster than its agility allows). **Values below 400 are raised to 400** at load, with a log warning. |
| `HelperOfflineAutoPotionDelay_AL0` .. `_AL3` | `2000 / 1500 / 1200 / 1000` | Minimum milliseconds between potions (minimum 200). The HP % that triggers a potion comes from the player's MU Helper panel. |
| `HelperOfflineAutoBuffDelay_AL0` .. `_AL3` | `30000` | Milliseconds between buff renewals (minimum 1000). Which buffs: the 3 buff slots of the MU Helper panel. |
| `HelperOfflineExperienceRate_AL0` .. `_AL3` | `0` | Experience rate while offline (same scale as the normal experience rate keys). `0` = use `OffAttackExperienceRate_AL*`. |

Also required: `HelperSwitch = 1` (`Common.dat`), the map's `HelperEnable` column = `1` in `Data/MapManager.txt`, and on the client `HelperActiveOfflineSystem = 1` in the client settings (shows the button). Both server and client switches must be on.

### Pitfalls

- The player must have saved a MU Helper configuration at least once.
- Refused in safe zones and event maps, and while `/attack`, `/offattack`, a personal shop or `/offstore` is active (only one offline system per character).
- Do not try to make VIPs faster than 400 ms: it is forced back to 400 to avoid the attack-speed anti-hack flagging the character.

### Reload

`/reload common`. Client `HelperActiveOfflineSystem` must be published to players from the Client Hub.

---

## Party Voice

### What it is

Voice chat between party members, inside the game, on every platform. Joining a party gives the player access to the party's voice room.

### Keys (`Common.dat`)

| Key | Default | Meaning |
|---|---|---|
| `PartyVoiceEnabled` | `1` | `1` = party voice enabled. `0` = disabled. |

`PartyVoiceTokenSecret` and `PartyVoiceWsUrl` (same file) are set by your installer and connect your server to the voice service. They must stay **private**: never share them, post them or change them by hand.

### Pitfalls

- If the token secret is empty, voice stays off even with `PartyVoiceEnabled = 1`.

### Reload

`/reload common`.

---

## Reward Online Users

### What it is

Pays coins to every online character at a fixed interval (for example 5 GoblinPoint every 15 minutes), with different amounts per account level.

### Keys (`Common.dat`)

| Key | Default | Meaning |
|---|---|---|
| `OnlineRewardOfflineSystems` | `0` | `0` = characters in offline attack / offline store / offline helper earn nothing. `1` = they earn too. |
| `OnlineRewardCoin1Delay` | `0` | Interval in minutes for WCoinC. |
| `OnlineRewardCoin1Value_AL0` .. `_AL3` | `0` | WCoinC paid per interval. |
| `OnlineRewardCoin2Delay` | `0` | Interval in minutes for WCoinP. |
| `OnlineRewardCoin2Value_AL0` .. `_AL3` | `0` | WCoinP paid per interval. |
| `OnlineRewardCoin3Delay` | `15` | Interval in minutes for GoblinPoint. |
| `OnlineRewardCoin3Value_AL0` .. `_AL3` | `5 / 10 / 15 / 20` | GoblinPoint paid per interval. |

### Pitfalls

- A coin with `Delay = 0` and a value above `0` is paid **every second**. Always set a delay when you set a value.
- The interval restarts at each login.

### Reload

`/reload common`.

---

## Combo to all classes

### What it is

Lets every class perform skill combos (normally only after the 3rd class quest), using the skill lists of [Custom Combo](combo.md).

### Keys (`Common.dat`)

| Key | Default | Meaning |
|---|---|---|
| `EnableComboToAllSwitch` | `1` | `1` = all classes can combo, no quest needed, using the primary/secondary skills from `Data/Custom/CustomCombo.txt`. `0` = original rule. |

### Reload

`/reload common` for the key, `/reload custom` for `CustomCombo.txt`.

---

## HP Bonus

### What it is

Extra maximum HP for each master reset, per account level.

### Keys (`Common.dat`)

| Key | Default | Meaning |
|---|---|---|
| `HpBonus_AL0` .. `_AL3` | `0` | HP added per master reset. |
| `HpBonus_MAX` | `10000` | Cap on the total bonus. |

Bonus = `min(MasterReset x HpBonus_ALx, HpBonus_MAX)`.

### Pitfalls

- `HpBonus_MAX = 0` (or a missing key) disables the bonus entirely.

### Reload

`/reload common`. The new HP applies the next time the character's stats are recalculated (relog, level-up, equipment change).

---

## Remove knockback

### What it is

Removes the push-back that some skills cause on the target, so fights (especially PvP) are not interrupted by knockback.

### Keys (`Common.dat`)

| Key | Default | Skill without knockback when `1` |
|---|---|---|
| `DisableLightningEffect` | `1` | Lightning (skill 3) |
| `DisableFallingEffect` | `1` | Falling Slash (19) |
| `DisableLungEffect` | `1` | Lunge (20) |
| `DisableUpperCutEffect` | `1` | Uppercut (21) |
| `DisableCycloneEffect` | `1` | Cyclone (22) |
| `DisableTwistEffect` | `1` | Slash (23) |

### Pitfalls

- `DisableTwistEffect` controls **Slash** (skill 23), not Twisting Slash.

### Reload

`/reload common`.

---

## Transformation Ring

### What it is

Chooses the monster that the classic Transformation Ring turns the character into, by ring level.

### Keys (`Common.dat`)

| Key | Default | Meaning |
|---|---|---|
| `TransformationRing1` | `2` | Ring +0: monster index (`2` Budge Dragon) |
| `TransformationRing2` | `7` | Ring +1 (`7` Giant) |
| `TransformationRing3` | `14` | Ring +2 (`14` Skeleton) |
| `TransformationRing4` | `8` | Ring +3 (`8` Poison Bull Fighter) |
| `TransformationRing5` | `9` | Ring +4 (`9` Thunder Lich) |
| `TransformationRing6` | `41` | Ring +5 (`41` Death Cow) |

Values are monster indexes from `Data/Monster/Monster.txt`.

### Pitfalls

- Use monsters whose model exists in the client.
- Event rings (Elite Skeleton, Panda, Skeleton, Christmas...) are not affected.

### Reload

`/reload common`. Players must unequip and re-equip the ring.

---

## Infinity Arrow

### What it is

Bows and crossbows stop consuming arrows/bolts, per account level.

### Keys (`Common.dat`)

| Key | Default | Meaning |
|---|---|---|
| `InfinityArrow_AL0` .. `_AL3` | `1` | `1` = arrows/bolts are not consumed for that account level. `0` = normal consumption. |

### Pitfalls

- Arrows or bolts must still be equipped: the client refuses to shoot with an empty quiver slot.

### Reload

`/reload common`.

---

## Reload module

| Feature group | Command |
|---|---|
| Announce Chaos Mix, Custom Store, Custom Attack, Event Time, Guild Warehouse, Shop MessageBox, Buy Inventory Extension, Chat Anti-Spam (`Custom.dat`) | `/reload custom` |
| MU Helper Offline, Party Voice, Reward Online Users, Combo to all, HP Bonus, Remove knockback, Transformation Ring, Infinity Arrow (`Common.dat`) | `/reload common` |
| Lang texts (946/947, 269, Buy Inventory messages) | `/reload lang` |
| Client keys (client settings) | No hot reload: must be [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md) |
