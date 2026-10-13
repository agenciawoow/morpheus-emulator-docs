# GameServerInfo - ChaosMix

## What it does

Sets the success rate of each standard Chaos Machine mix, per account level. The ingredients and zen cost of these mixes are fixed by the game; this file only changes the chance of success.

For mixes you create yourself, see [Custom Mix](../customs/custom-mix.md) and [Mix Expansion](../customs/mix-expansion.md).

## Where it lives

- `GameServer/Data/GameServerInfo - ChaosMix.dat`
- `GameServerCS/Data/GameServerInfo - ChaosMix.dat` (separate copy)

INI format, section `[GameServerInfo]`. Every key exists four times, `_AL0` to `_AL3` (Free, VIP 1, VIP 2, VIP 3). See [GameServer overview](README.md#account-levels-al0-to-al3).

## Syntax

| Value | Meaning |
|---|---|
| `0` to `100` | Fixed success chance in percent |
| `-1` | Use the classic formula based on the value of the items placed in the machine (only for the mixes marked "-1 allowed" below). The classic formula is capped at 90 %. |

A **Talisman of Luck** in the machine adds to the rate, and the Crywolf benefit (`CrywolfBenefitChaosRate` in Event.dat) can add more. The final chance never goes above 100 %.

If a key is missing, its value is **0**: the mix always fails.

## Keys

### Chaos Item Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `ChaosItemMixRate_AL0..3` | 0 | percent, -1 allowed | Chaos weapons (Chaos Dragon Axe, Chaos Nature Bow, Chaos Lightning Staff). Shipped: 60. |

### Devil Square Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `DevilSquareMixRate1_AL0..3` to `DevilSquareMixRate7_AL0..3` | 0 | percent | Devil's Invitation, one key per Devil Square level (1 to 7). Shipped: 70. |

### Plus Item Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `PlusItemLevelMixRate1_AL0..3` | 0 | percent | Item +9 to +10. Shipped: 75. |
| `PlusItemLevelMixRate2_AL0..3` | 0 | percent | +10 to +11. Shipped: 65. |
| `PlusItemLevelMixRate3_AL0..3` | 0 | percent | +11 to +12. Shipped: 55. |
| `PlusItemLevelMixRate4_AL0..3` | 0 | percent | +12 to +13. Shipped: 45. |
| `PlusItemLevelMixRate5_AL0..3` | 0 | percent | +13 to +14. Shipped: 35. |
| `PlusItemLevelMixRate6_AL0..3` | 0 | percent | +14 to +15. Shipped: 25. |

An item with Luck adds `AddLuckSuccessRate2_AL*` from Common.dat to these rates.

### Dinorant Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `DinorantMixRate_AL0..3` | 0 | percent | Uniria + Jewel of Chaos into Dinorant. Shipped: 60. |

### Fruit Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `FruitMixRate_AL0..3` | 0 | percent | Creating stat fruits. Shipped: 30. |

### Wing2 Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `Wing2MixRate_AL0..3` | 0 | percent, -1 allowed | Second-level wings (and Cape of Lord / Cape of Fighter). Shipped: 50. |

### Blood Castle Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `BloodCastleMixRate1_AL0..3` to `BloodCastleMixRate8_AL0..3` | 0 | percent | Invisibility Cloak, one key per Blood Castle level (1 to 8). Shipped: 70. |

### Wing1 Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `Wing1MixRate_AL0..3` | 0 | percent, -1 allowed | First-level wings. Shipped: 60. |

### Pet Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `PetMixRate_AL0..3` | 0 | percent | Dark Horse and Dark Raven (spirit) creation. Shipped: 50. |

### Piece of Horn Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `PieceOfHornMixRate_AL0..3` | 0 | percent | Fenrir step 1: Piece of Horn. Shipped: 50. |

### Broken Horn Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `BrokenHornMixRate_AL0..3` | 0 | percent | Fenrir step 2: Broken Horn. Shipped: 50. |

### Horn of Fenrir Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `HornOfFenrirMixRate_AL0..3` | 0 | percent | Fenrir step 3: Horn of Fenrir. Shipped: 30. |

### Horn of Fenrir Upgrade Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `HornOfFenrirUpgradeMixRate_AL0..3` | 0 | percent, -1 allowed | Upgrading Fenrir to Black (destroy) or Blue (protect). Shipped: 30. |

### Shield Potion Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `ShieldPotionMixRate1_AL0..3` | 0 | percent | Small shield potion. Shipped: 40. |
| `ShieldPotionMixRate2_AL0..3` | 0 | percent | Medium shield potion. |
| `ShieldPotionMixRate3_AL0..3` | 0 | percent | Large shield potion. |

### Jewel of Harmony Item Purity Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `JewelOfHarmonyItemPurityMixRate_AL0..3` | 0 | percent | Gemstone into Jewel of Harmony. Shipped: 100. |

### Jewel of Harmony Item Smelt Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `JewelOfHarmonyItemSmeltMixRate1_AL0..3` | 0 | percent | Smelting a normal item into a Lower Refining Stone. Shipped: 50. |
| `JewelOfHarmonyItemSmeltMixRate2_AL0..3` | 0 | percent | Smelting an excellent item into a Higher Refining Stone. Shipped: 100. |

### Jewel of Harmony Item Restore Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `JewelOfHarmonyItemRestoreMixRate_AL0..3` | 0 | percent | Removing a Harmony option from an item. Shipped: 100. |

### Item380 Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `Item380MixRate1_AL0..3` | 0 | percent | Adding the 380 option to an item below +7. Shipped: 50. |
| `Item380MixRate2_AL0..3` | 0 | percent | Adding the 380 option to an item +7 or higher. Shipped: 50. |

### Illusion Temple Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `IllusionTempleMixRate1_AL0..3` to `IllusionTempleMixRate6_AL0..3` | 0 | percent | Blood Scroll, one key per Illusion Temple level (1 to 6). Shipped: 70. |

### Feather Of Condor Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `FeatherOfCondorMixRate_AL0..3` | 0 | percent, -1 allowed | Feather of Condor (third-wing ingredient). Shipped: 25. |

### Wing3 Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `Wing3MixRate_AL0..3` | 0 | percent, -1 allowed | Third-level wings. Shipped: 30. |

### Socket Item Create Seed Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `SocketItemCreateSeedMixRate_AL0..3` | 0 | percent, -1 allowed | Seed extraction. Shipped: 100. |

### Socket Item Create Seed Sphere Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `SocketItemCreateSeedSphereMixRate_AL0..3` | 0 | percent, -1 allowed | Seed + Sphere into Seed Sphere. Shipped: -1 (formula: item value / 200000 + 80, capped at 90). |

### Lucky Item Refine Mix Settings

| Key | Default | Values | Description |
|---|---|---|---|
| `LuckyItemRefineMixRate1_AL0..3` | 0 | percent | Refining a lucky item with full durability (255). Shipped: 60. |
| `LuckyItemRefineMixRate2_AL0..3` | 0 | percent | Refining a lucky item with less durability. Shipped: 10. |

## Example

VIP accounts get better odds on +13 to +15:

```ini
PlusItemLevelMixRate5_AL0 = 35
PlusItemLevelMixRate5_AL1 = 40
PlusItemLevelMixRate5_AL2 = 45
PlusItemLevelMixRate5_AL3 = 50
PlusItemLevelMixRate6_AL0 = 25
PlusItemLevelMixRate6_AL1 = 30
PlusItemLevelMixRate6_AL2 = 35
PlusItemLevelMixRate6_AL3 = 40
```

## Rules and pitfalls

- **Always fill the four account levels.** A missing `_AL3` key is 0 and VIP 3 players always fail.
- `-1` only works on the mixes marked "-1 allowed". On the others it behaves like a negative chance (always fails).
- The Castle Siege server has its own copy of this file. Players who mix while in Castle Siege maps use the CS values.
- Keys for mixes of later seasons (Summon Scroll, Monster Wings, Socket Weapon, Pentagram) are not used by this Season 6 build.
- `AnnounceChaosMix` in Custom.dat announces successful and failed +level mixes to the server. See [Custom](custom.md#announce-chaos-mix-settings).

## Reload

`/reload chaosmix`. Takes effect on the next mix.
