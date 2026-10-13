# Adding a custom pet

This tutorial adds a new pet, **Crystal Fox**, that follows the character, gives bonuses while equipped, has its own model and sparkles, and then shows how to turn the same idea into a **mount**.

What the client and the server each need:

| Part | Side | Where | Applied with |
|---|---|---|---|
| Item row (slot, requirements, classes) | Server | `Data/Item/Item.txt` | GameServer restart (or **Reload Item**) |
| Pet bonuses | Server | `Data/Item/PetItemOption.txt` | **Reload Item** (`/reload item`) |
| Item entry (name, requirements, classes) | Client | **Client Hub → Client Files → Item.bmd** | Publish update |
| Model, movement style, scale | Client | **Client settings → Custom loaders → Wings/Pets → CustomPet.txt** | Publish update |
| Model and textures | Client | Game client files (`Data/Item/`) | Included in the client you distribute |
| Sparkles and particles (optional) | Client | **Custom loaders → Effects → CustomPetEffectStatic.txt / CustomPetEffectDynamic.txt** | Publish update |

References: [Client custom files](../muserver/client/custom-files.md#custompet), [Effects and item manager files](../muserver/client/effects-and-item-manager.md), [Item options](../muserver/data/item-options.md), [Items](../muserver/data/items.md).

## The example

| | Value |
|---|---|
| Name | Crystal Fox |
| Group / index | `13` / `150` (item code `13 × 512 + 150` = **6806**) |
| Model | `crystalfox.bmd` |
| Movement | Panda style (walks next to the character) |
| Bonuses | +10% damage dealt, −5% damage taken, +1000 HP, +10 attack speed |

---

{% stepper %}
{% step %}
### Choose a free index

Pets are group `13` (the shipped mounts also use group `12`). Pick an index that is free in the server `Data/Item/Item.txt`, in `Data/Item/PetItemOption.txt`, in the client **Item.bmd** and in the client custom file `CustomPet.txt`. The shipped `CustomPet.txt` already uses `13 211`–`13 503` and several `12` indexes; `13 150` is free.
{% endstep %}

{% step %}
### Add the server item row (Item.txt)

In `Data/Item/Item.txt`, section `13`, before that section's `end`. Copy an existing pet (here **Pet Panda**) and change it:

```
//Index  Kind1  Kind2  Kind3  Slot  Width  Height  Drop  SkillIndex  Value  iZen  Name           ReqLevel  Durability  IceRes  PoisonRes  LightRes  FireRes  EarthRes  WindRes  WaterRes  DW DK FE MG DL SM RG GL RW SL GC KN LM IK AQ
150      5      45     0      8     1      1       0     0           0      0     "Crystal Fox"  1         255         0       0          0         0        0         0        0         1  1  1  1  1  1  1  1  1  1  1  0  1  1  0
```

| Value | Column | Effect |
|---|---|---|
| `5 45 0` | Kind1/2/3 | Pet category codes, copied from the original pets. |
| `8` | Slot | The pet (helper) slot. |
| `1` × `1` | Width × Height | Inventory size. |
| `0` | Drop | Not a random monster drop. |
| `1` | ReqLevel | Level to equip. |
| `255` | Durability | Pet life. |
| class columns | | Who can equip it (`1` = from the base class). Section 13 has 9 stat columns after the name, then 15 class columns. |

Restart the GameServer (recommended for new items) or use **Reload Item**.
{% endstep %}

{% step %}
### Add the pet bonuses (PetItemOption.txt)

The server reads custom pet bonuses from `Data/Item/PetItemOption.txt`. Without a row the pet is purely cosmetic. Open it in the File editor and add, before `end`:

```
//Type  Index  IncDmgRate  DecDmgRate  Opt1  Val1  Opt2  Val2  Opt3  Val3  Opt4  Val4
13      150    10          5           119   1000  97    10    0     0     0     0
end
```

| Value | Column | Effect |
|---|---|---|
| `13 150` | Type / Index | The pet. |
| `10` | IncDmgRate | Damage the owner deals × 110%. |
| `5` | DecDmgRate | Damage the owner takes × 95%. |
| `119 1000` | Opt1 / Val1 | Option code 119 = add HP: +1000 HP. |
| `97 10` | Opt2 / Val2 | Option code 97 = add speed: +10. |
| `0 0` | Opt3/Val3, Opt4/Val4 | Not used (`0` = none). |

The bonuses apply while the item is equipped in the pet slot (and in the Muun/eagle slots on servers that have them), and only while its durability is above 0. Up to four options per pet. The full list of option codes (80–127: damage, defense, HP, MP, experience rate, double damage, ignore defense…) is in [Item options](../muserver/data/item-options.md).

Apply with **Reload Item** on the [Dashboard](../dashboard/services-and-reload.md) (or `/reload item`).

{% hint style="info" %}
The shipped file has rows for the Shining Tail mounts (`12 83`, `12 84`: +50000 HP and +100 speed). Use them as a model.
{% endhint %}
{% endstep %}

{% step %}
### Add the client item entry (Item.bmd)

**Client Hub → Client Files → Item.bmd**, filter **Pets**: **Duplicate** a similar pet (or **+ New item**), set `13` / `150`, **Item name** `Crystal Fox`, size 1 × 1, level 1 and the same classes as the server row. Save every language.

The client does not know the `PetItemOption.txt` bonuses. To show them, add tooltip lines in **Client Files → ItemTooltipText** (the phrases, for example "Damage +10%") and **ItemTooltip** (the slot for `13, 150`, one **Line** per phrase).
{% endstep %}

{% step %}
### Add the model and textures

Put `crystalfox.bmd` in the client's `Data/Item/` folder and its textures in `Data/Item/Texture/`. The model must contain the animations the movement style expects (copy the skeleton/animations of the pet whose movement you use).

These are game client files: add them to the client you distribute so they go out with your next update. The Client Hub has no upload for new 3D models; if support maintains your client package, send them the files.
{% endstep %}

{% step %}
### Add the client CustomPet.txt row

**Client Hub → Client settings → Custom loaders → Wings/Pets → CustomPet.txt**:

```
//ID  Index  Remplace  Movement  Fenri  Scale  FileName
13    150    -1        2         0      1.0    "crystalfox"
```

| # | Column | Value | Meaning |
|---|---|---|---|
| 1 | ID | `13` | Item group (`12` or `13`). |
| 2 | Index | `150` | Item index. |
| 3 | Remplace | `-1` | Item code whose model is drawn **in the inventory and item previews** instead of this pet's own model. `-1` = use the pet's own model. The shipped mounts point to a smaller "card" model (for example `7938`) so a big mount fits in a 1 × 1 slot. |
| 4 | Movement | `2` | How the pet moves (table below). |
| 5 | Fenri | `0` | Read by the client but not used by the current version. Keep `0`. |
| 6 | Scale | `1.0` | Model size. `0.5` = half, `1.5` = 50% bigger. |
| 7 | FileName | `"crystalfox"` | Model file without `.bmd`, from `Data/Item/`. |

**Movement values**

| Value | Style | Kind |
|---|---|---|
| `0` | Demon (flies next to the character) | Follower |
| `1` | Rudolf | Follower |
| `2` | Panda (walks next to the character) | Follower |
| `3` | Unicorn | Follower |
| `4` | Skeleton | Follower |
| `5` | Fenrir | **Mount** (the character rides it) |
| `6` | Dark Horse | **Mount** |
| `7` | Uniria | **Mount** |
| `8` | Muun pet | Follower (Muun style) |
| `11` | Dinorant (flying) | **Mount** |
| `13` | Muun pet, second style (used by the shipped `satanpet` rows) | Follower |

Other values do nothing (the pet is not drawn). Click **Save**.
{% endstep %}

{% step %}
### Optional: sparkles and particles on the pet

Two client effect files attach effects to **bones** of the pet model. Edit them in **Custom loaders → Effects** (that copy is the one players receive):

- `CustomPetEffectStatic.txt`: sprites (static glows, flares).
- `CustomPetEffectDynamic.txt`: particles (trails, emitted sparks).

Both use the same 11 columns:

| # | Column | Example | Meaning |
|---|---|---|---|
| 1 | Item | `13` | Item group. |
| 2 | (index) | `150` | Item index. |
| 3 | Effect | `32380` | Effect sprite/particle ID (the shipped rows use `32114`, `32379`, `32380`, `32394`). |
| 4 | Join | `10` | Bone of the model where the effect is attached. |
| 5 | Level | `0` | Ignored. Keep `0`. |
| 6–8 | ColorR, ColorG, ColorB | `0.29 0.77 0.97` | Color, floats 0.0–1.0. |
| 9 | Scale | `0.8` | Size. |
| 10 | Angle | `0.0` | Rotation in degrees. |
| 11 | Type | `0` | Effect sub-type, usually `0`. |

Example, light blue sparkles on two bones:

```
// Item | Effect | Join | Level | ColorR | ColorG | ColorB | Scale | Angle | Type
13   150   32380    10     0       0.29     0.77     0.97     0.8     0.0     0
13   150   32380    15     0       0.29     0.77     0.97     0.8     0.0     0
end
```

Bone numbers depend on the model; a bone that does not exist draws nothing. Try a few numbers and check in game. Per-mesh glow also works for pets through **Renders → CustomItemRenderMesh.txt** (same columns as for items, see [Adding a custom item](custom-item.md)).
{% endstep %}

{% step %}
### Publish the client update

1. **Client settings → Publish update → Prepare**.
2. **Publish updates** → check the client configuration and **Item.bmd** (and the tooltip files if you changed them) → **Publish N files** → **Deploy**.
3. Restart the game through the launcher.

See [Publishing updates](../dashboard/client-hub/publishing-updates.md).
{% endstep %}

{% step %}
### Test in game

On a Game Master character:

```
/make 13 150 0 0 0 0 0 0 0
```

Equip the Crystal Fox in the pet slot and check:

- the fox appears and walks beside the character (Panda style), at the right size;
- the inventory shows its model (or the `Remplace` model);
- the character window shows +1000 HP;
- hits deal about 10% more damage and you take about 5% less;
- the sparkles are on the right bones.
{% endstep %}
{% endstepper %}

---

## Variant: a mount instead of a follower

The shipped mounts are group `12`. To make a rideable mount, for example `12 485`:

1. **Item.txt**, section `12`: copy the **Shining Tail** row (`12 83`, Slot `8`) and change index and name.
2. **PetItemOption.txt**: `12 485 0 0 119 50000 97 100 0 0 0 0` (same bonuses as the Shining Tail).
3. **CustomPet.txt**: a mount movement (`5` Fenrir, `6` Dark Horse, `7` Uniria or `11` Dinorant), and usually a small inventory model in `Remplace`:

```
//ID  Index  Remplace  Movement  Fenri  Scale  FileName
12    485    -1        5         0      1.0    "crystalwolf"
```

The model must have the riding animations of the mount type you choose (a Fenrir-style mount needs Fenrir-style animations).

---

## Common mistakes

| Symptom | Cause | Fix |
|---|---|---|
| Pet equipped but no bonus | No row in `PetItemOption.txt`, no **Reload Item**, or the pet's durability is 0. | Add the row; reload; repair or replace the pet. |
| Pet invisible | No `CustomPet.txt` row, wrong `FileName`, model missing from the client, or a `Movement` value that is not in the table. | Check the row and the model; use a listed movement. |
| Pet slides or T-poses | The model doesn't have the animations of the chosen movement style. | Use a movement that matches the model, or fix the model's animations. |
| Inventory shows a strange model | `Remplace` points to another item. | Use `-1` for the pet's own model. |
| `/make` creates nothing | No `Item.txt` row, or the GameServer was not restarted/reloaded. | Add the row; restart or **Reload Item**. |
| Can't put it in the pet slot | `Slot` is not `8` in `Item.txt`, or the Item.bmd entry differs. | Use slot 8 on both sides. |
| Effects don't show | Edited the wrong copy, or the bone doesn't exist on this model. | Edit in **Custom loaders → Effects**; try other bones. |
| Tooltip shows no bonus | The client doesn't read `PetItemOption.txt`. | Add ItemTooltip lines. |
| Players see nothing new | Saved but not published, or they didn't restart. | Publish and restart the game. |
