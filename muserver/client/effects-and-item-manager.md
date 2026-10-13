# Effects & ItemManager

Glow and particle effects attached to items and pets (`Tools/MainInfo/Common/Effect/`), and the `Tools/MainInfo/ItemManager/` reference files.

## What it is

Players see these as the extra sparkles, flares and particle trails on custom weapons, wings and pets (for example the light points along a Blessed Divine sword, or the red flames around a dragon pet). Each row attaches one effect to one bone of the item/pet model, with its own color, size and angle.

## Enable it

There is no switch: the effect files are always packed. They only need to be correct and the client repacked.

| Folder | Packed into the client? | Notes |
|---|---|---|
| `Common/Effect/CustomEffectStatic.txt` | Yes | Item sprites (glows). |
| `Common/Effect/CustomEffectDynamic.txt` | Yes | Item particles. |
| `Common/Effect/CustomPetEffectStatic.txt` | Yes | Pet sprites. |
| `Common/Effect/CustomPetEffectDynamic.txt` | Yes | Pet particles. |
| `Common/Effect/CustomMonsterEffectStatic.txt` | **No** | Reserved; the current packer skips it. Ships empty. |
| `Common/Effect/CustomMonsterEffectDynamic.txt` | **No** | Reserved; the current packer skips it. Ships empty. |
| `Local/CustomEffect*.txt`, `Local/CustomPetEffect*.txt` | **No** | Old location, see below. |
| `ItemManager/*.txt` | **No** | Not read by the packer nor by the client. |

{% hint style="warning" %}
**Common/Effect vs Local.** The same four files exist in `Local/` and in `Common/Effect/`. The packer reads **only** `Common/Effect/`. Editing the `Local/` copy changes nothing in game. The shipped copies are byte-identical; keep them that way (the control panel's editor writes both at once). If you edit by hand, edit `Common/Effect/` and copy the file to `Local/`.
{% endhint %}

## Files & syntax

All six effect files share the same 11-column format (one effect per row, list ends with `end`):

| # | Column (header name) | Meaning | Values |
|---|---|---|---|
| 1 | Item | Item group | 0-15 (12/13 for pets) |
| 2 | (index) | Item index in the group | 0-511 |
| 3 | Effect | Effect texture/sprite ID to draw | engine effect ID (e.g. `32114`, `32379`, `32380`, `32394` in the shipped rows) |
| 4 | Join | Bone of the model where the effect is attached | bone number |
| 5 | Level | **Ignored** (read and discarded) | any number, use 0 |
| 6-8 | ColorR, ColorG, ColorB | Effect color | floats 0.0-1.0 |
| 9 | Scale | Effect size | float |
| 10 | Angle | Rotation | float (degrees) |
| 11 | Type | Effect sub-type passed to the sprite/particle | number, usually 0 |

How the file name decides the behavior:

| File | Drawn as | Applied to |
|---|---|---|
| `CustomEffectStatic.txt` | Sprite (static glow / flare) | The item model (weapon, wing, shield...) |
| `CustomEffectDynamic.txt` | Particle (moving / emitted) | The item model |
| `CustomPetEffectStatic.txt` | Sprite | The pet / mount model |
| `CustomPetEffectDynamic.txt` | Particle | The pet / mount model |

The header line in the files lists `Item | Effect | Join | Level | ...` without the second column; every data row has **both** the group and the index.

### ItemManager/CustomBuffEffect.txt

Buff list (index, numbers, name, flags, description), e.g.:

```
1	1	255	255	"Increase Attack Power"	0	1	1	"Physical Damage: +%d/Wizardry Damage: +%d/Curse Damage: +%d"
8	8	255	255	"Increase HP"	0	1	1	"Max HP Increase: +%d%%"
```

### ItemManager/CustomItemToolTip.txt

Item name color rules by item range, level, excellent and ancient (section `0`, ships empty). The header documents the color codes (1 blue, 2 red, 3 gold, 4 green, ... 24 Turquoise).

### ItemManager/CustomTextTooltip.txt

`Index Text` pairs. Ships empty.

{% hint style="info" %}
The three `ItemManager` files are not read by the packer or by the game client, so editing them has no effect in game. Their columns are not documented here because no loader defines them.
{% endhint %}

## Example

Item sprites on a weapon (`Common/Effect/CustomEffectStatic.txt`):

```
//==================================================================================
// Item | Effect | Join | Level | ColorR | ColorG | ColorB | Scale | Angle | Type
//==================================================================================
//-- Blessed Divine Sword (absolute02_sword.bmd) = item 0 51
0    51   32114    1      0       0.19     0.18     0.78     2.0     0.0     0    // blue flare on bone 1
0    51   32380    45     0       1.00     0.27     1.00     0.2     0.0     0    // small pink light on bone 45
0    51   32380    4      0       0.11     0.50     0.49     0.2     0.0     0    // teal light on bone 4
end
```

Pet particles (`Common/Effect/CustomPetEffectDynamic.txt`):

```
// Item | Effect | Join | Level | ColorR | ColorG | ColorB | Scale | Angle | Type
12   83   32394    10     1       0.29     0.77     0.97     1.1     100.0   0    // shining tails, bone 10
12   83   32394    15     1       0.29     0.77     0.97     1.1     100.0   0    // bone 15
end
```

Pet sprites (`Common/Effect/CustomPetEffectStatic.txt`, red flames on the ice dragon mount 12 483):

```
12	483   32380    22     2	      1.00     0.10     0.10     0.75    150.0   0
12	483   32380    24     2	      1.00     0.10     0.10     1.50    150.0   0
end
```

## Pitfalls

- Editing `Local/CustomEffect*.txt` instead of `Common/Effect/`: no change in game.
- Expecting `CustomMonsterEffect*.txt` to work: they are not packed by the current packer.
- Bone numbers depend on the model; a bone that does not exist simply draws nothing. Find bones by trial or with a model viewer.
- The `Level` column does not filter by item level; it is ignored.
- Every row needs all 11 values. A missing value shifts every following row.
- Each file holds a limited number of rows (the packer stops reading at its internal maximum); keep the lists to what you actually use.
- Effects are visual only: no server file needs to match, but the item/pet itself must exist on the server and have a model on the client ([CustomItem](custom-files.md#customitem), [CustomPet](custom-files.md#custompet)).

## Reload module

No hot reload. Save the `Common/Effect/` file (and copy it to `Local/`), repack with the GetMainInfo screen (or `Encoder.exe` in `Tools/MainInfo/`), redistribute the client data and restart the game.
