# Effects and item manager files

The client effect files (glow and particle effects attached to items and pets) and the client item manager files, both found in the Client Hub.

## What it is

Players see these as the extra sparkles, flares and particle trails on custom weapons, wings and pets (for example the light points along a Blessed Divine sword, or the red flames around a dragon pet). Each row attaches one effect to one bone of the item/pet model, with its own color, size and angle.

## Enable it

There is no switch: the client effect files are always published to the client. They only need to be correct and [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).

| File | Used by the client? | Notes |
|---|---|---|
| Client effect file `CustomEffectStatic.txt` | Yes | Item sprites (glows). |
| Client effect file `CustomEffectDynamic.txt` | Yes | Item particles. |
| Client effect file `CustomPetEffectStatic.txt` | Yes | Pet sprites. |
| Client effect file `CustomPetEffectDynamic.txt` | Yes | Pet particles. |
| Client effect file `CustomMonsterEffectStatic.txt` | **No** | Reserved; not published to the client in the current version. Ships empty. |
| Client effect file `CustomMonsterEffectDynamic.txt` | **No** | Reserved; not published to the client in the current version. Ships empty. |
| Client custom files `CustomEffect*.txt`, `CustomPetEffect*.txt` | **No** | Old copies, see below. |
| Client item manager files (`*.txt`) | **No** | Reference only; not used by the client. |

{% hint style="warning" %}
**Effect files vs custom files.** The same four files exist both as client effect files and as client custom files. Only the **client effect files** reach the players; editing the client custom file copy changes nothing in game. The shipped copies are identical; keep them that way (the Client Hub editor writes both at once).
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

### CustomBuffEffect.txt

Buff list (index, numbers, name, flags, description), e.g.:

```
1	1	255	255	"Increase Attack Power"	0	1	1	"Physical Damage: +%d/Wizardry Damage: +%d/Curse Damage: +%d"
8	8	255	255	"Increase HP"	0	1	1	"Max HP Increase: +%d%%"
```

### CustomItemToolTip.txt

Item name color rules by item range, level, excellent and ancient (section `0`, ships empty). The header documents the color codes (1 blue, 2 red, 3 gold, 4 green, ... 24 Turquoise).

### CustomTextTooltip.txt

`Index Text` pairs. Ships empty.

{% hint style="info" %}
The three client item manager files are not published to or read by the game client, so editing them has no effect in game. Their columns are not documented here because no loader defines them.
{% endhint %}

## Example

Item sprites on a weapon (client effect file `CustomEffectStatic.txt`):

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

Pet particles (client effect file `CustomPetEffectDynamic.txt`):

```
// Item | Effect | Join | Level | ColorR | ColorG | ColorB | Scale | Angle | Type
12   83   32394    10     1       0.29     0.77     0.97     1.1     100.0   0    // shining tails, bone 10
12   83   32394    15     1       0.29     0.77     0.97     1.1     100.0   0    // bone 15
end
```

Pet sprites (client effect file `CustomPetEffectStatic.txt`, red flames on the ice dragon mount 12 483):

```
12	483   32380    22     2	      1.00     0.10     0.10     0.75    150.0   0
12	483   32380    24     2	      1.00     0.10     0.10     1.50    150.0   0
end
```

## Pitfalls

- Editing the client custom file `CustomEffect*.txt` instead of the client effect file: no change in game.
- Expecting `CustomMonsterEffect*.txt` to work: they are not published to the client in the current version.
- Bone numbers depend on the model; a bone that does not exist simply draws nothing. Find bones by trial or with a model viewer.
- The `Level` column does not filter by item level; it is ignored.
- Every row needs all 11 values. A missing value shifts every following row.
- Each file holds a limited number of rows (rows beyond the internal maximum are dropped when the client data is built); keep the lists to what you actually use.
- Effects are visual only: no server file needs to match, but the item/pet itself must exist on the server and have a model on the client ([CustomItem](custom-files.md#customitem), [CustomPet](custom-files.md#custompet)).

## Reload module

No hot reload. Save the client effect file in the Client Hub (the editor keeps the client custom file copy in sync), [publish the update to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md), and have players restart the game.
