# Client custom files

Reference for every client custom file edited in **Client Hub → Client settings** (custom files): purpose, columns in the order they are read, examples, and the server file it must match.

## What it is

These files describe client-only data: models and textures for custom items, wings, capes, pets and monsters, glow/render effects, custom maps, stack sizes, the command window, the Buy VIP list, the Morpheus Shop catalogue, event icons, map markers and WebView translations. You edit them as tables or text in [Client Hub → Client settings](../../dashboard/client-hub/client-settings.md) (**Custom loaders** tab), and they reach the players when you [publish an update from the Client Hub](../../dashboard/client-hub/publishing-updates.md).

## Enable it

Most files are always published to the client. Three depend on a client setting:

| File | Client setting | Default | Meaning |
|---|---|---|---|
| `CustomShortcutButton.txt` | `[Custom] EnableMenuCustomShortcutCommand` | `0` | 0 = file not published, quick-command card hidden. |
| `CustomFullMap.txt` | `[Custom] CustomFullMapSwitch` | `0` | 0 = file not published, map shows only the default markers. |
| `CustomMorpheusShop.txt` | `[Custom] CustomMorpheusShopSwitch` | `0` | File is always published, but only used when the Morpheus Shop is on. |

See [Client settings reference](client-settings-reference.md).

## Files & syntax

General format (same as the server MemScript files):

- `//` starts a comment; blank lines are ignored.
- Columns are separated by spaces or tabs. Text goes in double quotes.
- `*` in a numeric column means `-1` ("any" / "not used").
- The list ends with `end`. Two files are split into numbered sections (`EventNameDisplay.txt`, `message.txt`).
- **Item codes.** Many files use two columns `Type Index` (item group 0-15 and index inside the group). Others use a single item code = `Type * 512 + Index` (for example `14 244` = `7412`, `12 160` = `6304`).
- **Colors.** Two notations exist, depending on the file: integer RGB `0-255` (three columns) or float RGB `0.0-1.0` (three columns, `value / 255`). Floats above 1.0 make the color brighter.
- Textures for items are loaded from `Data/Item/Texture/`; item models from `Data/Item/` (or `Data/Player/` for armor sets).

{% hint style="info" %}
Several files carry an old header in Portuguese or Vietnamese, sometimes with column names that do not describe what the column really does. The tables below follow the order in which the columns are actually read when the client data is built. Where a header is misleading, a warning says so.
{% endhint %}

## CEffectRenderMesh

`CEffectRenderMesh.txt` - extra render layers (glow, chrome, color) on item meshes, in an alternative column order. Its rows are added to the same list as [CustomItemRenderMesh](#customitemrendermesh). Several rows can target the same item (one per mesh/effect layer).

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Type | Item group | 0-15 |
| 2 | Index | Item index in the group | 0-511 |
| 3-5 | Red, Green, Blue | Mesh light color | floats, 1.0 = 255 |
| 6 | Group | Mesh index inside the model (`.bmd`) | 0 = first mesh |
| 7 | RenderEffect | Render flags (see [CustomItemRenderMesh](#customitemrendermesh) RenderFlag list). 2 = texture, 70 = texture + chrome + bright (glow) | bit flags |
| 8 | EffectType | Texture animation type | -1 = none, 0 = move X, 1 = move Y, 2 = move -X, 3 = move -Y, 4 = frame XY, 5 = frame Y |
| 9 | TimeEffectType | Animation speed. `1.0` is converted to `-1` (static) | float |
| 10 | TextureID | Replacement texture index (from [CustomImage](#customimage)) | -1 = model texture |
| 11 | TimeTextureID | Light blend pulse time | float, -1 = none |
| 12 | LevelGlow | Level glow flag (per file header) | 0 / 1 |

The mesh blend strength is set automatically: 0.25 for EffectType 4 or higher, 1.0 otherwise.

```
// Type  Index  Red   Green  Blue  Group  RenderEffect  EffectType  TimeEffectType  TextureID  TimeTextureID  LevelGlow
   13    166    1.0   1.0    1.0   0      2             -1          1.0             -1         -1             0    // Bronze Coin: base render
   13    166    0.78  0.58   0.20  0      70            -1          1.0             -1         -1             0    // Bronze Coin: gold glow
   14    180    1.96  0.69   0.0   0      70            -1          1.0             -1         -1             0    // Jewel: orange glow
```

Server counterpart: none (visual only).

## CustomBuyVip

`CustomBuyVip.txt` - VIP packages shown in the Buy VIP window (opened with the menu button / the End key). See [Buy VIP](../customs/buy-vip.md).

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Package ID sent to the server when buying | > 0, unique |
| 2 | Experience+ | Experience bonus shown | number (%) |
| 3 | DropRate+ | Drop bonus shown | number (%) |
| 4 | Days | Duration | days |
| 5 | AccountLevel | VIP level granted | 1-3 |
| 6 | Coin1 | Price in WCoinC | number, 0 = free in this coin |
| 7 | Coin2 | Price in WCoinP | number |
| 8 | Coin3 | Price in GoblinPoint | number |
| 9 | VIPName | Name shown | "text" |

```
//#Index  #Experience+  #DropRate+  #Days  #AccountLevel  #Coin1  #Coin2  #Coin3  #VIPName
  1       10            10          7      1              10      0       0       "VIP Bronze"
  4       10            10          7      2              10      0       0       "VIP Silver"
  9       30            30          30     3              20      0       0       "VIP Gold"
end
```

Must match: `Data/Custom/CustomBuyVip.txt` (same columns). The server looks the package up by `Index` and charges/grants what **its** row says.

{% hint style="warning" %}
As shipped, every row of the server `Data/Custom/CustomBuyVip.txt` is commented out. The client shows 9 packages but nothing can be bought until you add the same rows on the server.
{% endhint %}

## CustomCape

`CustomCape.txt` - cloth physics for capes/cloaks (the waving cloth layers).

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Item group | usually 12 |
| 2 | SubIndex | Item index | 0-511 |
| 3 | RenderType | Built-in cape behavior. A value other than 0 overrides the remaining columns | 0 = custom (use the textures below), 1 = Wings of Angel and Devil, 2 = Cloak of Death, 3 = Control Cloak, 4 = Referee Cloak, 5 = Transcendence, 6 = Wings of Power, 7 = Cloak of Isolation |
| 4 | IsStrip | Cape type (0 = plain, 1 = plain + strip) | 0 / 1 |
| 5 | Hide1GroupId | Mesh index of the model that is replaced by the cloth layer | mesh index, -1 = none |
| 6 | Hide2GroupId | Mesh index replaced by the strip layer. Non-zero enables the strip (needs TextureStrip) | mesh index, -1 / 0 = none |
| 7 | TextureLayer | Texture of the main cloth | "file" or "" |
| 8 | TextureStrip | Texture of the strip | "file" or "" |
| 9 | TextureLayer_R | Second texture of the main cloth | "file" or "" |
| 10 | TextureStrip_R | Second texture of the strip | "file" or "" |

```
//#Index  #SubIndex  #RenderType  #IsStrip  #Hide1GroupId  #Hide2GroupId  #TextureLayer                 #TextureStrip                 #TextureLayer_R                  #TextureStrip_R
  12      160        0            1         0              7              "Fifth_Darklord_cape_03.tga"  "Fifth_Darklord_cape_05.tga"  "Fifth_Darklord_cape_Glow1.jpg"  ""
  12      262        2            1         -1             -1             ""                            ""                            ""                               ""
end
```

Server counterpart: none, but the item must exist on the server (`Data/Item/Item.txt`, and `Data/Custom/CustomWing.txt` if it is a custom wing).

## CustomCommand

`CustomCommand.txt` - the in-game command list (command window / menu button, and the mobile command panel). Shows each command with its syntax and description, filtered by account level.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Command index. **Same index as `Data/Command.txt`**; the client uses it when it sends a command | 0-N |
| 2 | Color | Text color | 0 = default |
| 3 | Command | Command text | "/cmd" |
| 4-7 | Enable AL0..AL3 | Show to free / VIP1 / VIP2 / VIP3 accounts | 0 / 1 |
| 8 | GameMaster | Who sees it | 0 = everyone, 1 = GM, 2 = admin |
| 9 | Syntax | Usage line shown | "text" |
| 10 | Description | Description shown | "text" |

```
// #Index #Color #Command   #AL0 #AL1 #AL2 #AL3  #GameMaster  #Syntax            #Description
   0      0      "/move"    1    1    1    1     0            "/move <Map>"      "Teleport"
   10     0      "/bau"     1    1    1    1     0            "/bau <num>"       "Change Vault"
   35     0      "/reload"  1    1    1    1     2            "/reload <Type>"   "Reload Configuration"
end
```

Must match: `Data/Command.txt` (same Index and command text). The mobile vault selector relies on index 10 (`/bau`) and 31 (`/abrirbau`). Hiding a command here does not disable it: the server rules in `Data/Command.txt` still apply.

## CustomEffectDynamic

`CustomEffectDynamic.txt` - particle effects attached to item bones. **Not published from here**: the client uses the client effect file of the same name (Client Hub, **Effects** category) instead. Keep both copies identical (the Client Hub editor does this). Columns and examples: [Effects and item manager files](effects-and-item-manager.md).

## CustomEffectStatic

`CustomEffectStatic.txt` - sprite (glow) effects attached to item bones. **Not published from here**: the client uses the client effect file of the same name (Client Hub, **Effects** category). See [Effects and item manager files](effects-and-item-manager.md).

## CustomFullMap

`CustomFullMap.txt` - custom markers on the full map (Tab) and minimap. Only published when `[Custom] CustomFullMapSwitch = 1`.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Marker ID | number |
| 2 | MapNum | Map number | map index |
| 3 | Type | Marker icon | 1 = NPC, 2 = Gate, 3 = Spot, 4 = Boss, 5 = Event, 6 = Quest, 7 = Vault, 8 = Point |
| 4 | CordX | X coordinate | 0-255 |
| 5 | CordY | Y coordinate | 0-255 |
| 6 | FixName | 0 = name only on hover/click, 1 = name always visible | 0 / 1 |
| 7 | MiniMap | 0 = hidden on the minimap, 1 = icon (no name) on the minimap | 0 / 1 |
| 8 | Name | Label | "text" (long names are truncated) |

```
//  Index  MapNum  Type  CordX  CordY  FixName  MiniMap  Name
    1      0       1     122    110    0        0        "Trainer"
    2      0       1     125    144    0        0        "Wandering Merchant Alex"
    100    2       3     14     121    1        0        "Elite Yeti"
end
```

Server counterpart: none (positions should match your `MonsterSetBase` / gates if you want the markers to be accurate).

## CustomImage

`CustomImage.txt` - registers extra textures (from `Data/Item/Texture/`) under an image index, so render files can use them as `Texture` / `TextureID`.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Texture | Image index used by other files | from 33001 |
| 2 | Wrap (header name) | **Texture filter** | 9728 = nearest (pixelated), 9729 = linear (smooth) |
| 3 | Type (header name) | **Texture wrap mode** | 10497 = repeat, 10496 = clamp, 33071 = clamp to edge |
| 4 | Path | File name in `Data/Item/Texture/` | "file" |

{% hint style="warning" %}
The header calls column 2 "Wrap" and column 3 "Type", but the client uses column 2 as the **filter** and column 3 as the **wrap mode**. The shipped rows (`9728 10497` = nearest + repeat) are correct; keep that order.
{% endhint %}

```
// Texture  #Wrap(filter)  #Type(wrap)  #Path
33001       9728           10497        "absolute02_swordrender.JPG"
33013       9728           10497        "rare_shiningtails_1_render.jpg"
end
```

Server counterpart: none.

## CustomItem

`CustomItem.txt` - registers the model (`.bmd`) of new items and the tint of their light/glow layers.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Type | Item group | 0-15 |
| 2 | Index | Item index | 0-511 |
| 3-5 | ColorR, ColorG, ColorB | Tint of the item's light/glow layers | integers 0-255 |
| 6 | FileName | Model file name without extension. Loaded from `Data/Item/` (or `Data/Player/` for groups 7-11, armor). Claws also load `<name>_L` / `<name>_R` | "name" |

```
//Type #Index  #ColorR  #ColorG  #ColorB  #FileName
0      51      255      178      63       "absolute02_sword"
0      46      255      178      63       "BloodangelClo"      //-- claws: also BloodangelClo_L / _R
end
```

Server counterpart: the item must exist in `Data/Item/Item.txt` (and the client's item data). The first row for an item wins.

{% hint style="warning" %}
Never leave a row with an empty file name (`""`): the client stops reading the file there, so every item registered after it loses its model.
{% endhint %}

## CustomItemRenderMesh

`CustomItemRenderMesh.txt` - per-mesh render layers for items (chrome, glow, bright, animated textures). Several rows per item, one per mesh/layer.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Type | Item group | 0-15 |
| 2 | Index | Item index | 0-511 |
| 3 | GroupId | Mesh index inside the model | 0 = first mesh |
| 4 | RenderFlag | Render flags (sum). Negative = render the normal body instead | see list below |
| 5 | Texture | Texture index (from [CustomImage](#customimage)) | -1 = model texture |
| 6 | Color | 1 = use the BodyLightColor below, 0 = no tint | 0 / 1 |
| 7-9 | BodyLightColor R, G, B | Mesh tint | floats 0.0-1.0 |
| 10 | Luminosity | Light intensity | float 0.0-1.0 |
| 11 | RenderType | Texture animation | -1 = none, 0 = move X, 1 = move Y, 2 = move -X, 3 = move -Y, 4 = frame XY, 5 = frame Y |
| 12 | TimeEffect | Texture animation speed | float, -1 = none |
| 13 | TimeTexture | Light blend pulse time | float, -1 = none |
| 14 | GlowLevel | Level glow flag (per file header) | float, 0 / 1 |

RenderFlag values: 1 color, 2 texture, 4 chrome, 8 metal, 16 lightmap, 32 shadowmap, 64 bright, 128 dark, 256 extra, 512 chrome2, 1024 wave, 2048 chrome3, 4096 chrome4, 8192 no depth, 16384 chrome5, 32768 oil, 65536 chrome6, 131072 chrome7, 262144 doppelganger, 524288 chrome8. Example: `66` = texture + bright, `68` = chrome + bright.

```
//#Type #Index #GroupId #RenderFlag #Texture #Color #R   #G   #B    #Luminosity #RenderType #TimeEffect #TimeTexture #GlowLevel
0      124    0        2           -1       1      1.0  1.0  1.0   1.0         -1          -1.0        -1.0         1
0      124    1        68          33022    1      1.0  1.0  1.0   1.0         -1          -1.0        4.4          0
12     83     1        68          33012    1      0.8  0.9  0.75  1.0         -1          -1.0        -1.0         0
end
```

Server counterpart: none.

## CustomJewel

`CustomJewel.txt` - custom jewels: model, sale price and the client-side rules for dropping the jewel on an item. See [Custom Jewel](../customs/custom-jewel.md).

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Type | Item group of the jewel | usually 14 |
| 2 | Index | Item index of the jewel | 0-511 |
| 3 | MinItemLevel | Minimum target item level | 0-15, `*` = any |
| 4 | MaxItemLevel | Maximum target item level | 0-15, `*` = any |
| 5 | MaxItemOption1 | Max skill option (0 = only items without it) | `*` = not checked |
| 6 | MaxItemOption2 | Max luck option | `*` = not checked |
| 7 | MinItemOption3 | Min additional option | `*` = not checked |
| 8 | MaxItemOption3 | Max additional option | `*` = not checked |
| 9 | MinItemNewOption | Min excellent options | `*` = not checked |
| 10 | MaxItemNewOption | Max excellent options | `*` = not checked |
| 11 | MaxItemSetOption | Ancient (set) option requirement | `*` = not checked |
| 12 | MinItemSocketOption | Min sockets | `*` = not checked |
| 13 | MaxItemSocketOption | Max sockets | `*` = not checked |
| 14 | EnableSlotWeapon | Can be used on weapons | 0 = no, other / `*` = yes |
| 15 | EnableSlotArmor | Can be used on armor | 0 = no, other / `*` = yes |
| 16 | EnableSlotWing | Can be used on wings | 0 = no, other / `*` = yes |
| 17-20 | SuccessRate AL0..AL3 | Success rate shown per account level | 0-100 |
| 21 | SalePrice | Sale price | number |
| 22 | ModelName | Model file name (in `Data/Item/`) | "name" |

{% hint style="warning" %}
The file has two header lines; the second one (starting with `#Index #ItemIndex`) is the server layout. The client file uses the first one: **two** columns `Type Index` and no `#Name` column.
{% endhint %}

```
//#Type #Index #MinLvl #MaxLvl #Opt1 #Opt2 #MinOpt3 #MaxOpt3 #MinExc #MaxExc #Set #MinSock #MaxSock #Weapon #Armor #Wing #SR0 #SR1 #SR2 #SR3 #SalePrice #ModelName
14      245    0       15      0     *     *        *        *       *       *    *        *        *       *      *     100  100  100  100  20         "Jewel181" //Jewel of Skill
14      248    0       15      *     *     *        *        0       5       *    *        *        *       *      *     100  100  100  100  20         "Jewel184" //Jewel of Excellent
end
```

Must match: `Data/Custom/CustomJewel.txt` section 0. Server `ItemIndex` = `Type * 512 + Index` (`14 245` = `7413`). Copy the same limits so the client and the server agree on where the jewel can be used.

## CustomMonster

`CustomMonster.txt` - model, size and name of **new** monsters and NPCs (index 579 or higher; lower indexes are the original game monsters). See [Custom Monster](../customs/custom-monster.md).

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | ID | Monster/NPC index (same as the server's `Monster.txt` / `MonsterSetBase`) | >= 579 |
| 2 | Type | 1 = monster, any other value = NPC | 0 / 1 |
| 3 | Size | Model scale | float |
| 4 | Name | Name shown over the head | "text" |
| 5 | Directory | Folder under `Data/` where the model is | "path\\" |
| 6 | FileName | Model file name without extension | "name" |

```
// ID   Type  Size  Name                    Directory        FileName
581     0     0.80  "Morpheus Tournament"   "custom\\npc\\"  "lycandor00"
end
```

Reserved indexes per the file header: 580, 581, 582, 583, 584, 604, 643, 651.

Must match: the server must spawn the same index (`Data/Monster/Monster.txt` + `MonsterSetBase`). The server `Data/Custom/CustomMonster.txt` is a **different** file (stats, rewards, messages) and does not replace this one. NPC names can be overridden per position with [CustomNpcName](#customnpcname).

## CustomMonsterRenderMesh

`CustomMonsterRenderMesh.txt` - per-mesh render layers for monsters. Same idea as [CustomItemRenderMesh](#customitemrendermesh), with a single monster index and **no** GlowLevel column.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Monster index | number |
| 2 | GroupId | Mesh index | 0 = first mesh |
| 3 | RenderFlag | Render flags | same list as CustomItemRenderMesh |
| 4 | Texture | Texture index | -1 = model texture |
| 5 | Color | 1 = use the color below | 0 / 1 |
| 6-8 | R, G, B | Mesh tint | floats 0.0-1.0 |
| 9 | Luminosity | Light intensity | float |
| 10 | RenderType | Texture animation | -1 ... 5 |
| 11 | TimeEffect | Animation speed | float |
| 12 | TimeTexture | Light blend pulse time | float |

{% hint style="warning" %}
The file's header line omits the `Color` and `Luminosity` columns. Use the 12 columns above. The file ships empty, and the current client does not apply monster render meshes; treat it as reserved.
{% endhint %}

```
//#Index #GroupId #RenderFlag #Texture #Color #R   #G   #B   #Luminosity #RenderType #TimeEffect #TimeTexture
581      0        66          -1       1      1.0  0.5  0.5  1.0         -1          -1.0        -1.0
end
```

Server counterpart: none.

## CustomMorpheusShop

`CustomMorpheusShop.txt` - which items appear in the Morpheus Shop and how they are shown. Prices, option prices, limits, discounts and texts are **not** here; they are in the server file. See [Morpheus Shop](../customs/morpheus-shop.md).

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | ItemIndex | Item code (`Type * 512 + Index`) | e.g. `0011`, `3086` |
| 2 | ItemGroup | Shop tab | 0 Swords, 1 Axes, 2 Scepters, 3 Spears, 4 Bows, 5 Staffs, 6 Shields, 7 Helms, 8 Armors, 9 Pants, 10 Gloves, 11 Boots, 12 Others 1, 13 Others 2, 14 Others 3 |
| 3 | ItemPosX | 3D preview X offset | number |
| 4 | ItemPosY | 3D preview Y offset | number |
| 5 | ItemScale | 3D preview scale | float, 0 or less = 1.0 |

No sections; one item per line, up to 5000 items.

```
//ItemIndex   ItemGroup    ItemPosX   ItemPosY   ItemScale
0011          0            0          0          1.0  //Legendary Sword
3086          6            0          -5         1.0  //Legendary Shield
6668          14           0          -5         1.0  //Pendant of Lighting
end
```

Must match: `Data/Custom/CustomMorpheusShop.txt` section 0 must have a price row covering every `ItemIndex` listed here. Also needs `CustomMorpheusShopSwitch = 1` in both the client settings and `GameServerInfo - Custom.dat`.

{% hint style="warning" %}
The header of the **server** `Data/Custom/CustomMorpheusShop.txt` says it is used to build the client. It is not: the client only uses this client custom file (simple 5-column list). The server file (5 sections) is read by the GameServer.
{% endhint %}

## CustomNameWave

`CustomNameWave.txt` - characters whose name is drawn with the 7-color glowing effect.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Row ID | number |
| 2 | Character Name | Exact character name | "name" |

```
// Index    Character Name
1       "PhongCom2"
2       "ImpELF"
end
```

Server counterpart: none (names must exist on your server to be visible).

## CustomNpcName

`CustomNpcName.txt` - overrides the name shown for an NPC, optionally only at a given position. Ships empty.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | ID | NPC index | number |
| 2 | World | Map number (informational; matching uses ID + X + Y) | map index |
| 3 | PositionX | X where the NPC stands | -1 = any |
| 4 | PositionY | Y where the NPC stands | -1 = any |
| 5 | Name | Name shown | "text" |

```
//ID   World  PositionX  PositionY  Name
249    0      133        79         "Guard Captain"
581    0      -1         -1         "Tournament Master"
end
```

Server counterpart: the NPC spawn in `MonsterSetBase` (same index and coordinates).

## CustomPet

`CustomPet.txt` - custom pets and mounts: model, movement style and scale.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | ID | Item group | 12 or 13 |
| 2 | Index | Item index | 0-511 |
| 3 | Remplace | Item code whose model is shown in the inventory/3D preview instead of this item | item code, -1 = own model |
| 4 | Movement | How the pet moves | 0 Demon, 1 Rudolf, 2 Panda, 3 Unicorn, 4 Skeleton, 5 Fenrir (mount), 6 Dark Horse (mount), 7 Uniria (mount), 8 Muun pet, 11 Dinorant-style flying mount, 13 second Muun style. Values 9, 10 and 12 draw nothing |
| 5 | Fenri | Reserved (all shipped rows use 0) | 0 |
| 6 | Scale | Model scale | float |
| 7 | FileName | Model file name (in `Data/Item/`) | "name" |

```
//ID  Index  Remplace  Movement  Fenri  Scale  FileName
12    83     7938      5         0      1.2    "shiningtails"
12    471    7942      6         0      1.0    "ghost_horse"
13    212    -1        8         0      1.0    "dragonpet"
end
```

Server counterpart: the item must exist in `Data/Item/Item.txt`; pet stats/options are server-side (`Data/Item/PetItemOption.txt` and related).

## CustomPetEffectDynamic

`CustomPetEffectDynamic.txt` - particle effects attached to pet bones. **Not published from here**: the client uses the client effect file of the same name (Client Hub, **Effects** category). See [Effects and item manager files](effects-and-item-manager.md).

## CustomPetEffectStatic

`CustomPetEffectStatic.txt` - sprite effects attached to pet bones. **Not published from here**: the client uses the client effect file of the same name (Client Hub, **Effects** category). See [Effects and item manager files](effects-and-item-manager.md).

## CustomRenderEffect

`CustomRenderEffect.txt` - gives an item one of the built-in animated render presets (Blessed Divine weapons, Holy Angel, Awakening, Blood Angel, special wings, shields...). The full preset list (0-105) is in the file header.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Type | Item group | 0-15 |
| 2 | Index | Item index | 0-511 |
| 3 | EffectIndex | Preset number from the file header | 0-105 |

```
//#Type #Index #EffectIndex  #Comment
0       51     1             //absolute02_sword (Blessed Divine Sword preset)
12      480    58            //WingsOfPower
end
```

Server counterpart: none.

## CustomShortcutButton

`CustomShortcutButton.txt` - the "Quick commands" buttons in the burger menu (Android, iOS, web). Only published when `[Custom] EnableMenuCustomShortcutCommand = 1`.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Button ID (free numbering, unrelated to CustomCommand) | number |
| 2 | Color | Button color | 0 = by tab (player blue, GM gold, admin red), 1 blue, 2 gold, 3 red, 4 green, 5 purple; other = 0 |
| 3 | Command | Exact text sent to the server, as if typed in chat. May include a fixed argument | "/cmd arg" |
| 4-7 | Enable AL0..AL3 | Show to free / VIP1 / VIP2 / VIP3 accounts | 0 / 1 |
| 8 | GameMaster | Tab and visibility: 0 = player (everyone), 1 = GM (GM and admin), 2 = admin only | 0 / 1 / 2 |
| 9 | ButtonText | Button label. The first word is highlighted | "text" |

```
//  #Index #Color #Command          #AL0 #AL1 #AL2 #AL3 #GameMaster #ButtonText
    1      0      "/pk"             1    1    1    1    0           "/pk"
    2      0      "/zen 2000000"    1    1    1    1    0           "/zen 2000000"
    32     0      "/startdrop"      1    1    1    1    1           "/startdrop"
end
```

Must match: the command must exist, spelled exactly, in `Data/Command.txt`, `Data/Custom/CustomCommandDescription.txt`, `Data/Custom/CustomExchangeCoin.txt` or `Data/Custom/CustomMove.txt` (tried in that order). The server still applies its own rules (level, resets, zen, delay, coins).

{% hint style="danger" %}
Two failures are silent (the button does nothing): the command does not exist in any server table (typo), or it is disabled (`Enable = 0`) in `Data/Command.txt`. Be careful with destructive commands: `/zen` without an argument sets Zen to 0, `/reset` without an argument really resets the character.
{% endhint %}

## CustomSmokeItem

`CustomSmokeItem.txt` - color of the boots "smoke" effect of full +10 (or higher) sets, for custom armor sets.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | SetIndex | Armor set index (the boots item index inside group 11) | 0-511 |
| 2-4 | Red, Green, Blue | Smoke color | floats 0.0-1.0 |

```
//SetIndex | Red | Green | Blue
1            0.5   0.5     0.5
17           1.0   0.0     0.0
end
```

Only shown when the set is +10 or higher and `MaxItemLevelOn` is 10 or more. Server counterpart: none.

## CustomStack

`CustomStack.txt` - which items stack in the inventory and up to how many.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Item code (`Type * 512 + Index`) | e.g. `7169` |
| 2 | Level | Item level the rule applies to | 0-15, `*` = any |
| 3 | MaxStack | Maximum stack size | 1-255 |
| 4 | CreateIndex | Item created when the stack is full (e.g. fragments to a full item) | item code, `*` = none |
| 5 | BreakUp | Break-up flag | 0 / 1 |

```
//Index   Level  MaxStack   CreateIndex   BreakUp  Comment
2055      *      255        *             0        //Bolt
6288      *      5          6292          0        //Mithril Fragment
7197      *      5          7196          0        //Symbol of Kundun
end
```

Must match: `Data/Item/ItemStack.txt` (same columns; the server writes `-1` where the client writes `*`). If they differ, the client shows a stack size the server refuses.

## CustomWing

`CustomWing.txt` - custom wings: model and the values shown in the tooltip. See [Custom Wing](../customs/custom-wing.md).

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | # (Type) | Item group | 12 |
| 2 | Index | Item index | 0-511 |
| 3 | DefA | Defense constant | number |
| 4 | IncDmgA | Damage increase constant A | number |
| 5 | IncDmgB | Damage increase constant B | number |
| 6 | DecDmgA | Damage absorption constant A | number |
| 7 | DecDmgB | Damage absorption constant B | number |
| 8, 10, 12 | Opt1..Opt3 | Normal option IDs | option ID (see below) |
| 9, 11, 13 | Val1..Val3 | Values of Opt1..Opt3 | number |
| 14, 16, 18, 20 | NewOpt1..NewOpt4 | Excellent option IDs | option ID |
| 15, 17, 19, 21 | NewVal1..NewVal4 | Values of NewOpt1..NewOpt4 | number |
| 22 | ModelType | 0 = normal wing, other = attached like a cloak (back bone) | 0 / 1 |
| 23 | ModelName | Model file name (in `Data/Item/`) | "name" |

Option IDs: 80 additional damage, 81 additional wizardry damage, 82 additional defense rate, 83 additional defense, 84 luck, 85 HP recovery %, 102 ignore defense %, 109 reflect damage %, 110 full HP recovery %, 111 full mana recovery %, 113 additional curse damage.

```
//# Index DefA IncDmgA IncDmgB DecDmgA DecDmgB Opt1 Val1 Opt2 Val2 Opt3 Val3 NewOpt1 NewVal1 NewOpt2 NewVal2 NewOpt3 NewVal3 NewOpt4 NewVal4 ModelType ModelName
12   27    4    139     2       61      2       80   4    85   1    83   4    110     5       111     5       109     5       102     5       0         "SpiritualWorldWing"
12   160   4    139     2       61      2       80   4    85   1    83   4    110     5       111     5       109     5       102     5       1         "Wing505"
end
```

Must match: `Data/Custom/CustomWing.txt`, same rows and same columns (the server ignores ModelType and ModelName). The server values are the real stats; the client values are what the tooltip shows.

{% hint style="warning" %}
As shipped, the stat columns are identical on both sides; only ModelType/ModelName differ on a few rows (for example `12 160`: client `1 "Wing505"`, server `0 "ResplendentCape"`), and the client has an extra row `12 161` that the server does not. That is fine because the server ignores those two columns, but a wing that exists only on the client has no server stats. Keep all stat columns identical.
{% endhint %}

## CustomWorld

`CustomWorld.txt` - custom maps (index above 81): which original map behavior they reuse, their name, title image and music.

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Map | Custom map number | > 81 |
| 2 | Effect | Original map whose behavior/effects are reused (e.g. 91 Acheron, 95 Debenter, 100 Uruk Mountain, 110 Nars, 112 Ferea, 113 Nixie Lake) | map number, -1 = none |
| 3 | TextID | Text index for the map name (in the client text file) | number, -1 = default |
| 4 | Ext | 1 = load the extended terrain for this map | 0 / 1 |
| 5 | ImgName | Map title image | "file.tga" |
| 6 | Mp3File | Background music | "file" or "None" |

```
//#Map  #Effect  #TextID  #Ext #ImgName              #Mp3File
91      91       4000     0    "acheron.tga"         "None"
100     100      4002     1    "urukmountain.tga"    "None"
114     -1       4007     0    "labyrinth.tga"       "None"
end
```

Server counterpart: the map must exist on the server (`MapServerInfo`, terrain/attribute files, `Data/Move/` and gates).

## EventNameDisplay

`EventNameDisplay.txt` - name and icon of each event and invasion in the mobile event schedule. Two sections, each ending with `end`. See [Event names](../events/event-name.md).

| Section | Contents |
|---|---|
| `0` | Global events (fixed engine indexes: 0 Blood Castle, 1 Devil Square, 2 Chaos Castle, 3 Illusion Temple, 4 Online Lottery, 5 Custom Quiz, 6 Custom Bonus, 7 Custom Drop, 8 King of MU, 9 TvT, 10 GvG, 11 Moss Merchant, 12 Morpheus Tournament) |
| `1` | Invasions / daily bosses (index = `Data/Event/InvasionManager.dat` index) |

| # | Column | Meaning | Values |
|---|---|---|---|
| 1 | Index | Event/invasion index | number |
| 2 | Name | Name shown (max 47 characters) | "text" |
| 3 | IconName | Icon file, with extension. Missing = text only | "file.png" (max 63 characters) |

```
0
// Index  Name              IconName
   0      "Blood Castle"    "bc.png"
   12     "Morpheus Tournament"  "morpheus.png"
end
1
   0      "Skeleton King"   "skeletonking.png"
   8      "Medusa"          "medusa.png"
end
```

Only events 0-3 and the invasions have real start times in the schedule; the other global events appear with a countdown only.

Must match: `Data/Event/EventName.dat` sections 0 and 1, same index for the same event.

{% hint style="warning" %}
As shipped, section 1 does not match the server: client index 9 is "Medusa2" while the server's index 9 is "Mini Boss Balrog", and server indexes 10-15 (mini bosses, Selupan, Kundun) have no client row. Align both files with your `InvasionManager.dat`.
{% endhint %}

## message.txt

`message.txt` - translations of the WebView interface texts (mobile/web panels). One section per language; published as key -> translated text.

| Section number | Language |
|---|---|
| 0 | English |
| 1 | Portuguese (**source**: the texts exactly as they appear in the interface) |
| 2 | Spanish |
| 3 | Chinese |
| 4 | Korean |
| 5 | Vietnamese |
| 6 | Thai |

Rules:

- Each section is the section number, then one `"text"` per line, then `end`.
- Translations are matched **by position**: the 3rd line of section 0 translates the 3rd line of section 1. All sections must have the same lines in the same order.
- A missing language section falls back to Portuguese. Use `""` to leave one line untranslated without shifting the rest.
- Section 1 text must be identical to the interface text (accents and punctuation included). No double quotes inside a text; max 255 characters per line.

```
1
"CONFIGURAÇÕES"
"Jogo"
"Áudio"
end
0
"SETTINGS"
"Game"
"Audio"
end
```

Server counterpart: none (server texts are in `Data/Lang/`).

## Pitfalls

- A missing or misnamed file is published as an empty list without any error. Keep the exact file names.
- Two-column `Type Index` vs single item code: `CustomStack`, `CustomMorpheusShop` and the server `CustomJewel` use the single code (`Type * 512 + Index`); most other files use two columns.
- Color notation differs: `CustomItem` uses 0-255 integers; render, smoke and effect files use 0.0-1.0 floats.
- Editing the `CustomEffect*.txt` / `CustomPetEffect*.txt` client custom files has no effect on its own; the copy the client uses is the client effect file (Client Hub, **Effects** category).
- A trailing comment after the last quoted column is fine, but do not add extra values: every extra token shifts the next row.
- Client files only control what the player sees. Stats, prices, rates and permissions always come from the server files.

## Reload module

No hot reload. Save the file in **Client Hub → Client settings**, [publish the update to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md), and have players restart the game. Apply the server-side counterpart with its `/reload` group (`/reload custom` for `Data/Custom/*`, `/reload event` for `EventName.dat`).
