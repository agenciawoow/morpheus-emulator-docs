# Adding a custom wing

This tutorial adds a new wing, **Wings of Morpheus**, with its own defense, damage increase and absorption, options, model and glow, makes it a possible result of the 3rd-wing mix, and tests it.

A custom wing has the same four parts as any new item (see [Adding a custom item](custom-item.md)) plus the wing stats:

| Part | Side | Where | Applied with |
|---|---|---|---|
| Item row (size, requirements, classes) | Server | `Data/Item/Item.txt`, section `12` | GameServer restart (or **Reload Item**) |
| Wing stats and options | Server | `Data/Custom/CustomWing.txt` | `/reload custom` |
| Mix result (optional) | Server | `Data/Custom/CustomWingMix.txt` | `/reload custom` |
| Item entry (name, size, requirements, classes) | Client | **Client Hub → Client Files → Item.bmd** | Publish update |
| Wing stats for the tooltip + model | Client | **Client settings → Custom loaders → Wings/Pets → CustomWing.txt** | Publish update |
| Model and textures | Client | Game client files (`Data/Item/`) | Included in the client you distribute |
| Glow, cape cloth, particles (optional) | Client | `CustomItemRenderMesh`, `CustomCape`, effect files | Publish update |

References: [Custom Wing](../muserver/customs/custom-wing.md), [Client custom files](../muserver/client/custom-files.md#customwing), [Items](../muserver/data/items.md).

## The example

| | Value |
|---|---|
| Name | Wings of Morpheus |
| Group / index | `12` / `450` (item code `12 × 512 + 450` = **6594**) |
| Model | `WingsMorpheus.bmd` |
| Classes | Every Season 6 class, from the base class |
| Stats at +0 | Deals 145% damage, takes 60% of incoming damage |
| Stats at +15 | +60 defense from the wing, deals 175% damage, takes 30% of incoming damage |

---

{% stepper %}
{% step %}
### Choose a free index

Wings are group `12`. Pick an index that is free in the server `Data/Item/Item.txt` (section `12`), in the server `Data/Custom/CustomWing.txt`, in the client **Item.bmd** and in the client custom file `CustomWing.txt`. The shipped files already use many indexes between `27` and `496`; `450` is free in all of them.
{% endstep %}

{% step %}
### Add the server item row (Item.txt)

In `Data/Item/Item.txt`, section `12`, before that section's `end`. Copy an existing wing and change it:

```
//Index  Kind1  Kind2  Kind3  Slot  Width  Height  Drop  SkillIndex  Value  iZen  Name                 Level  PowerATTK  Defense  Durability  DamageMin  DamageMax  ReqLevel  ReqStrength  ReqDexterity  ReqEnergy  ReqVitality  ReqCharisma  DW DK FE MG DL SM RG GL RW SL GC KN LM IK AQ
450      6      25     1      7     4      3       0     0           0      0     "Wings of Morpheus"  150    0          60       255         0          0          400       0            0             0          0            0            1  1  1  1  1  1  1  0  0  0  0  0  0  0  0
```

| Value | Column | Effect |
|---|---|---|
| `6 25 1` | Kind1/2/3 | Wing category codes, copied from **Wing of Storm** (a 3rd-level wing). |
| `7` | Slot | Wings slot. |
| `4` × `3` | Width × Height | Inventory size. |
| `0` | Drop | Not a random monster drop. |
| `60` | Defense | Base defense of the item. |
| `255` | Durability | Max durability. |
| `400` | ReqLevel | Level to equip. |
| `1 1 1 1 1 1 1 0 …` | Classes | DW, DK, FE, MG, DL, SM, RF from the base class. Use `2` or `3` to require the 2nd or 3rd class (the original 3rd wings use `3`). |

Restart the GameServer (recommended for new items) or use **Reload Item**.
{% endstep %}

{% step %}
### Add the wing stats on the server (CustomWing.txt)

Without this row the item is a plain item in the wing slot: no damage increase, no absorption, no wing options. Open `Data/Custom/CustomWing.txt` in the File editor and add, before `end`:

```
//#  Index  DefA  IncDmgA  IncDmgB  DecDmgA  DecDmgB  Opt1  Val1  Opt2  Val2  Opt3  Val3  NewOpt1  NewVal1  NewOpt2  NewVal2  NewOpt3  NewVal3  NewOpt4  NewVal4  ModelType  ModelName
12   450    4     145      2        60       2        80    4     85    1     83    4     110      5        111      5        109      5        102      5        0          "WingsMorpheus"
```

How each value works on this wing:

| Value | Column | Effect | At +15 |
|---|---|---|---|
| `4` | DefA | Defense added per item level (`DefA × level`) | +60 defense |
| `145` / `2` | IncDmgA / IncDmgB | Damage dealt in %: `IncDmgA + level × IncDmgB` | 145 + 30 = **175%** |
| `60` / `2` | DecDmgA / DecDmgB | Damage taken in %: `DecDmgA − level × DecDmgB` | 60 − 30 = **30%** taken (70% absorbed) |
| `80 4` | Opt1 / Val1 | Additional option (Jewel of Life): additional damage, 4 per step | |
| `85 1` | Opt2 / Val2 | Alternative additional option used when the item has excellent bit 16: HP recovery 1% per step | |
| `83 4` | Opt3 / Val3 | Alternative additional option used with excellent bit 32: additional defense | |
| `110 5` | NewOpt1 / NewVal1 | Excellent bit 1: 5% chance of full life recovery | |
| `111 5` | NewOpt2 / NewVal2 | Excellent bit 2: 5% chance of full mana recovery | |
| `109 5` | NewOpt3 / NewVal3 | Excellent bit 4: return 5% of the enemy's attack | |
| `102 5` | NewOpt4 / NewVal4 | Excellent bit 8: ignore 5% of the opponent's defense | |
| `0` / `"WingsMorpheus"` | ModelType / ModelName | Read but ignored by the server (used by the client). | |

Option codes: `80` additional damage, `81` additional wizardry damage, `82` defense rate, `83` defense, `84` luck, `85` HP recovery %, `102` ignore defense %, `109` reflect %, `110` full HP recovery %, `111` full mana recovery %, `113` additional curse damage.

{% hint style="warning" %}
Keep `DecDmgA − 15 × DecDmgB` above 0 and `IncDmgA + 15 × IncDmgB` above 100, or the wing becomes a penalty at high levels.
{% endhint %}

Apply with **Reload Custom** on the [Dashboard](../dashboard/services-and-reload.md) (or `/reload custom`). Wings already equipped pick up new values on relog or re-equip.
{% endstep %}

{% step %}
### Add the client item entry (Item.bmd)

**Client Hub → Client Files → Item.bmd**, filter **Wings**: **Duplicate** a similar wing (or **+ New item**), set `12` / `450`, **Item name** `Wings of Morpheus`, **Size** 4 × 3, the same **Defense**, **Character level** 400 and the same classes as the server row. Save every language.
{% endstep %}

{% step %}
### Add the model and textures

Put `WingsMorpheus.bmd` in the client's `Data/Item/` folder and its textures in `Data/Item/Texture/`. Upload them in [Client Files → Data files](../dashboard/client-hub/client-files.md#data-files): open `item`, drag `WingsMorpheus.bmd`; open `item/texture`, drag the textures; then publish the update.

You do **not** need a `CustomItem.txt` row for a wing: `CustomWing.txt` on the client already registers the model.
{% endstep %}

{% step %}
### Add the client CustomWing.txt row

**Client Hub → Client settings → Custom loaders → Wings/Pets → CustomWing.txt**. Add the **same line** as the server, with the client-only columns filled:

```
//#  Index  DefA  IncDmgA  IncDmgB  DecDmgA  DecDmgB  Opt1  Val1  Opt2  Val2  Opt3  Val3  NewOpt1  NewVal1  NewOpt2  NewVal2  NewOpt3  NewVal3  NewOpt4  NewVal4  ModelType  ModelName
12   450    4     145      2        60       2        80    4     85    1     83    4     110      5        111      5        109      5        102      5        0          "WingsMorpheus"
```

| # | Column | Client use |
|---|---|---|
| 1–2 | `#`, Index | Item group `12` and index `450`. |
| 3–7 | DefA … DecDmgB | Shown in the tooltip (defense, damage increase, absorption). The **server** values are the real ones; keep them identical so the tooltip tells the truth. |
| 8–13 | Opt1 … Val3 | Additional option shown in the tooltip. |
| 14–21 | NewOpt1 … NewVal4 | Excellent options shown in the tooltip. |
| 22 | ModelType | `0` = drawn as wings on the back. Any other value = attached like a cape/cloak (Dark Lord and Rage Fighter style). |
| 23 | ModelName | Model file without `.bmd`, from `Data/Item/`. |

Click **Save**.
{% endstep %}

{% step %}
### Optional: glow, cloth and particles

- **Glow per mesh** (**Renders → CustomItemRenderMesh.txt**): one row per mesh of the wing model. Example, mesh 0 normal and mesh 1 bright:

```
//#Type #Index #GroupId #RenderFlag #Texture #Color #R   #G   #B    #Luminosity #RenderType #TimeEffect #TimeTexture #GlowLevel
12      450    0        2           -1       1      1.0  1.0  1.0   1.0         -1          -1.0        -1.0         1
12      450    1        66          -1       1      0.8  0.9  1.0   1.0         -1          -1.0        -1.0         0
```

`66` = texture + bright. `RenderType` animates the texture (`0` move X, `1` move Y, `4` frame XY…).

- **Built-in wing presets** (**Renders → CustomRenderEffect.txt**): `12 450 <EffectIndex>`. Presets such as `9` Heavenly Wings or `58` Wings of Power are made for those specific models; use them only when your model is based on one of them.
- **Cape cloth** (**Wings/Pets → CustomCape.txt**): only for capes (`ModelType` not 0). It replaces a mesh of the model with waving cloth:

```
//#Index #SubIndex #RenderType #IsStrip #Hide1GroupId #Hide2GroupId #TextureLayer          #TextureStrip  #TextureLayer_R  #TextureStrip_R
12       450       0           0        0             -1            "morpheus_cape.tga"    ""             ""               ""
```

- **Sprites/particles on bones** (**Effects → CustomEffectStatic.txt / CustomEffectDynamic.txt**): `12 450 <Effect> <Bone> 0 <R> <G> <B> <Scale> <Angle> 0`. See [Effects and item manager files](../muserver/client/effects-and-item-manager.md).
{% endstep %}

{% step %}
### Optional: make it come out of a mix

**Option A, the 3rd-wing mix (Chaos Goblin).** Add the wing to `Data/Custom/CustomWingMix.txt`, section `0` (up to 50 rows):

```
0
//Category  Index
12          450      // Wings of Morpheus
end
```

A successful 3rd-wing mix now picks at random among the original 3rd wings **and** every row of this file, all with the same weight. `/reload custom` applies it. The mix window on the client already shows the 3rd-wing recipe, so nothing changes on the client.

**Option B, your own recipe.** Use a [Custom Mix](../muserver/customs/custom-mix.md) whose prize is an event bag that contains the wing (in an advanced bag list the wing is `6594`). This lets you choose the ingredients, Zen cost and success rate per VIP level. See the [EventItemBag tutorial](event-item-bag-step-by-step.md) to build the prize bag with a `SpecialValue` trigger.
{% endstep %}

{% step %}
### Publish the client update

1. **Client settings → Publish update → Prepare**.
2. **Publish updates** → check that the client configuration and **Item.bmd** are listed → **Publish N files** → **Deploy**.
3. Restart the game through the launcher.

See [Publishing updates](../dashboard/client-hub/publishing-updates.md).
{% endstep %}

{% step %}
### Test in game

On a Game Master character:

```
/make 12 450 15 0 1 4 15 0 0
```

Wings of Morpheus +15, luck, additional option `4`, excellent mask `15` (bits 1 + 2 + 4 + 8 = all four wing excellent options).

Check:

- the wing model shows on the character (and as a cape if you used `ModelType 1`);
- the tooltip shows the defense, damage increase and absorption of your row (at +15: +60 defense, 175% damage dealt, 30% damage taken) and the four excellent options;
- your damage goes up and the damage you take goes down compared with no wing;
- a class without a `1` in the class columns cannot equip it.
{% endstep %}
{% endstepper %}

---

## Common mistakes

| Symptom | Cause | Fix |
|---|---|---|
| The wing equips but gives no damage/absorption | No row in the **server** `Data/Custom/CustomWing.txt`, or no `/reload custom`. | Add the row; reload; re-equip. |
| Tooltip shows different numbers from what happens in game | Server and client `CustomWing.txt` rows differ. The server values are the real ones. | Copy the same stat columns to both files. |
| Wing is invisible | No client `CustomWing.txt` row, wrong `ModelName`, or the model isn't in the client. | Check the row, the name (without `.bmd`) and `Data/Item/`. |
| Wing floats in the wrong place / sticks out of the back | Wrong `ModelType` (cape model drawn as wings or the opposite). | Use `0` for wings, `1` for capes. |
| `/make` creates nothing | No `Item.txt` row, or the GameServer was not restarted/reloaded. | Add the row; restart or **Reload Item**. |
| Wing becomes a penalty at high level | `DecDmgA − 15 × DecDmgB` went to 0 or below, or `IncDmgA + 15 × IncDmgB` below 100. | Lower the per-level constants. |
| Wing never comes out of the 3rd-wing mix | Not in `CustomWingMix.txt`, missing `end`, or more than 50 rows. | Fix the file; `/reload custom`. |
| Columns shifted on every wing after yours | One column too many or too few (each row has 23 values). | Count against the header. |
| Players don't see any change | Saved in the Client Hub but not published, or they didn't restart. | Publish and restart the game. |
