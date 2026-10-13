# Adding a custom item (CustomItem)

This tutorial adds a brand-new sword, **Morpheus Blade**, to the game: the server knows its stats, the client shows its 3D model, name and tooltip, it can glow, and players can get it from a shop, a box or the Morpheus Shop.

A new item always has **four parts**. If one is missing, the item is invisible, nameless, or cannot be created.

| Part | Side | Where you edit it | Applied with |
|---|---|---|---|
| Item row (stats, size, requirements, classes) | Server | `Data/Item/Item.txt` in the [File editor](../dashboard/file-editor.md) | GameServer restart (or **Reload Item**) |
| Item entry (name, size, requirements, classes shown) | Client | **Client Hub → Client Files → Item.bmd** | Publish update |
| 3D model and textures | Client | Game client files (`Data/Item/`) | Included in the client you distribute |
| Model registration (`CustomItem.txt`) | Client | **Client Hub → Client settings → Custom loaders → Items** | Publish update |

References: [Items](../muserver/data/items.md), [Client custom files](../muserver/client/custom-files.md#customitem), [Client files](../dashboard/client-hub/client-files.md), [Client settings](../dashboard/client-hub/client-settings.md), [Publishing updates](../dashboard/client-hub/publishing-updates.md).

## The example

| | Value |
|---|---|
| Name | Morpheus Blade |
| Group / index | `0` (swords) / `120` |
| Item code | `0 × 512 + 120` = **120** |
| Model file | `MorpheusBlade.bmd` |
| Who can use it | Dark Knight and Magic Gladiator |

---

{% stepper %}
{% step %}
### Choose a free group and index

The **group** (0 to 15) decides what the item is: 0 swords, 5 staffs, 6 shields, 7 to 11 armor pieces, 12 wings and misc, 13 pets/rings/pendants, 14 consumables and jewels. The **index** (0 to 511) must be free in **all three** places:

- the server `Data/Item/Item.txt` (section `0` for swords),
- the client **Item.bmd** (search `0,120` in **Client Files → Item.bmd**),
- the client custom file `CustomItem.txt` (search the `0  120` row).

{% hint style="warning" %}
Some indexes are already used on the client only (the shipped `CustomItem.txt` registers models for many items, for example swords `0 36` to `0 98`). Pick an index that is free on both sides, not just on the server.
{% endhint %}
{% endstep %}

{% step %}
### Add the server row in Item.txt

Open `Data/Item/Item.txt` in the [File editor](../dashboard/file-editor.md). Find section `0` (it starts with a line containing only `0`) and add the new line **before that section's `end`**. The easiest way is to copy a similar item and change it:

```
//Index  Kind1  Kind2  Kind3  Slot  Width  Height  Drop  SkillIndex  Value  iZen  Name              TwoHand  Level  PowerATTK  DamageMin  DamageMax  WeaponSpeed  Durability  MagicDurability  Magicpower  ReqLevel  ReqStrength  ReqDexterity  ReqEnergy  ReqVitality  ReqCharisma  DW DK FE MG DL SM RG GL RW SL GC KN LM IK AQ
120      1      1      1      0     1      4       0     22          0      0     "Morpheus Blade"  0        150    0          250        280        50           200         0                0           380       160          60            0          0            0            0  1  0  1  0  0  0  0  0  0  0  0  0  0  0
```

What the important values do:

| Value | Column | Effect |
|---|---|---|
| `1 1 1` | Kind1/2/3 | Category codes copied from another one-handed sword. Always copy them from an item of the same type. |
| `0` | Slot | Right hand. |
| `1` / `4` | Width / Height | Takes 1 × 4 slots in the inventory. |
| `0` | Drop | **Not** a random monster drop; it only comes from shops, bags, mixes or commands. |
| `22` | SkillIndex | Skill the weapon gives (copied from a similar sword). `0` = none. |
| `150` | Level | Drop level, also used by the price formula. |
| `250` / `280` | DamageMin / DamageMax | Base damage. |
| `380`, `160`, `60` | ReqLevel, ReqStrength, ReqDexterity | Requirements to equip. |
| `0 1 0 1 0 …` | Class columns | DK and MG from the base class (`1`); `2`/`3` would require the 2nd/3rd class. Every line needs all **15** class columns. |

{% hint style="danger" %}
Count the columns. One value too many or too few shifts every item after it. Section 0 has 11 base columns, the quoted name, 15 weapon columns and 15 class columns.
{% endhint %}

Apply it: **restart the GameServer** (recommended after adding items), or use **Reload Item** on the [Dashboard](../dashboard/services-and-reload.md). `/reload all` does **not** reload the item tables.
{% endstep %}

{% step %}
### Add the client item entry (Item.bmd)

The client has its own item table, with the name, size and requirements it displays. Open **Client Hub → Client Files**, select **Item.bmd**:

1. Find a similar sword (filter **Weapons**, search its name) and click **Duplicate**, or click **+ New item**.
2. Set the type/index to `0` / `120`.
3. **Identity** tab: **Item name** `Morpheus Blade`, **Size (X × Y)** 1 × 4.
4. **Combat**: damage 250–280, attack speed 50.
5. **Requirements**: **Character level** 380, **Strength** 160, **Agility** 60.
6. **Classes**: Dark Knight and Magic Gladiator **Base**, everyone else blocked.
7. **Flags** and **Economy**: as you want (drop, trade, warehouse, sellable).
8. Save. Item.bmd exists **per language**: fill the name in every language your players use (the save message lists the languages saved).

{% hint style="warning" %}
Size, requirements and classes must match the server row. If the client says 1 × 4 and the server says 2 × 4, items overlap in the inventory; if the requirements differ, the client shows the item as usable while the server refuses it (or the opposite).
{% endhint %}
{% endstep %}

{% step %}
### Add the 3D model and textures

Every item is drawn from a `.bmd` model file plus the textures it references.

| Item type | Model goes in | Textures go in |
|---|---|---|
| Weapons, shields, wings, pets, jewels, misc. (groups 0 to 6, 12 to 15) | `Data/Item/` | `Data/Item/Texture/` |
| Armor pieces (groups 7 to 11) | `Data/Player/` | `Data/Item/Texture/` |
| Claws (Rage Fighter) | `Data/Item/` as `<name>.bmd`, `<name>_L.bmd` and `<name>_R.bmd` | `Data/Item/Texture/` |

For the example: `Data/Item/MorpheusBlade.bmd`, and its textures (the file names written inside the model, usually `.ozj`/`.ozt`/`.jpg`/`.tga`) in `Data/Item/Texture/`.

These are game client files: add them to the client you distribute so they go out with your next client update. The Client Hub edits the client's text files and data tables but has no upload for new 3D models; if support maintains your client package, send them the files.

{% hint style="info" %}
**Test the flow first, model later.** Until your own model is ready you can point the next step at a model that already ships with the client, for example `"absolute02_sword"`. Two items can use the same model file.
{% endhint %}
{% endstep %}

{% step %}
### Register the model in CustomItem.txt

Open **Client Hub → Client settings**, tab **Custom loaders**, category **Items**, file **CustomItem.txt**, and add a row (before `end` if you use the text mode):

```
//Type  #Index  #ColorR  #ColorG  #ColorB  #FileName
0       120     255      178      63       "MorpheusBlade"
```

| # | Column | Value | Meaning |
|---|---|---|---|
| 1 | Type | `0` | Item group. |
| 2 | Index | `120` | Item index in the group. |
| 3–5 | ColorR, ColorG, ColorB | `255 178 63` | Tint (0–255) the client uses for the item's light/glow layers. `255 178 63` is the warm gold used by most shipped rows. |
| 6 | FileName | `"MorpheusBlade"` | Model file **without** `.bmd`. Loaded from `Data/Item/` (`Data/Player/` for groups 7 to 11). For claws, the client also loads `MorpheusBlade_L` and `MorpheusBlade_R`. |

Click **Save**.

{% hint style="warning" %}
Never leave a row with an empty file name (`""`). The client stops reading the list at the first row without a model, so every item **after** it loses its model too.
{% endhint %}
{% endstep %}

{% step %}
### Optional: glow, render layers and particles

All of these are client custom files in **Custom loaders**. Skip this step for a plain item.

**Render layers** (category **Renders**, `CustomItemRenderMesh.txt`): one row per mesh of the model. Example: draw mesh 0 normally and give mesh 1 a chrome + bright glow using extra texture `33022`:

```
//#Type #Index #GroupId #RenderFlag #Texture #Color #R   #G   #B   #Luminosity #RenderType #TimeEffect #TimeTexture #GlowLevel
0       120    0        2           -1       1      1.0  1.0  1.0  1.0         -1          -1.0        -1.0         1
0       120    1        68          33022    1      1.0  1.0  1.0  1.0         -1          -1.0        4.4          0
```

- `RenderFlag` is a sum: `2` texture, `4` chrome, `64` bright. `68` = chrome + bright.
- `Texture` `-1` = the model's own texture; a number from `33001` up is an extra texture registered in `CustomImage.txt` (category **Worlds/Others**). To use your own glow texture, put it in `Data/Item/Texture/` and register it:

```
// Texture  #Wrap(filter)  #Type(wrap)  #Path
33100       9728           10497        "morpheusblade_glow.jpg"
```

- Shipped items list one row per mesh. If a part of your model disappears after you add render rows, add a plain row (`RenderFlag 2`) for that mesh.

**Built-in animated presets** (category **Renders**, `CustomRenderEffect.txt`): `Type Index EffectIndex`. The presets (0–105, listed in the file header) are made for specific shipped models, so use them only when your model is a re-texture of that model.

**Sprites and particles on bones** (category **Effects**, `CustomEffectStatic.txt` for glows, `CustomEffectDynamic.txt` for particles):

```
// Item | Effect | Join | Level | ColorR | ColorG | ColorB | Scale | Angle | Type
0    120   32114    1      0       0.19     0.18     0.78     2.0     0.0     0    // blue flare on bone 1
0    120   32380    4      0       1.00     0.27     1.00     0.2     0.0     0    // small pink light on bone 4
end
```

`Join` is the bone number of the model; a bone that does not exist draws nothing. Edit these effect files in the **Effects** category of the Client Hub (that copy is the one players receive). Columns: [Effects and item manager files](../muserver/client/effects-and-item-manager.md).

**Armor sets only** (category **Items**, `CustomSmokeItem.txt`): the color of the boots smoke on a full +10 set, `SetIndex R G B` with floats 0.0–1.0.
{% endstep %}

{% step %}
### Optional: tooltip text

The name comes from **Item.bmd**. For a colored name or extra lines (for example "Forged in the Morpheus realm"):

1. **Client Files → ItemTooltipText**: add the phrase (per language) and note its number.
2. **Client Files → ItemTooltip**: find or add the slot for `0, 120`, set **Name**, **Name color**, **Show level**, and add a **Line** that uses the phrase number. The **Preview** shows the result.
3. Save.
{% endstep %}

{% step %}
### Make the item obtainable

Pick one or more:

| Source | How | Apply |
|---|---|---|
| NPC shop | [NPC Shop editor](../dashboard/editors/npc-shop.md) → open the shop → **Add item** → search `0,120` → set level/options → **Save** | **Reload Shop** |
| Box or boss drop | [EventItemBag editor](../dashboard/editors/event-item-bag.md), or the [EventItemBag tutorial](event-item-bag-step-by-step.md). In an advanced list the item index is `0120`. | **Reload EventItemBag** |
| Morpheus Shop | [Morpheus Shop editor](../dashboard/editors/morpheus-shop.md) → **Add item** → set **Tab (group)**, offsets and scale → **Create a price range for this item** | Price: **Reload** (custom). Item list: **Encode + Publish to client** |
| Custom mix | [Custom Mix](../muserver/customs/custom-mix.md): recipe whose prize is a bag that contains the item | `/reload custom` and `/reload eventitembag` |
| Normal monster drop | Set `Drop 1` in `Item.txt` and check its `Level` ([Item drop](../muserver/data/item-drop.md)) | Restart / **Reload Item** |
{% endstep %}

{% step %}
### Publish the client update

1. In **Client Hub → Client settings**, click **Publish update** → **Prepare**. This builds the client configuration with your custom files and puts it in the publish queue.
2. Open **Client Hub → Publish updates**. You should see the client configuration and **Item.bmd** (plus ItemTooltip/ItemTooltipText if you changed them).
3. Click **Publish N files** → **Deploy** and wait for **finished ✓**.
4. Restart the game through the launcher so it downloads the update.

Details: [Publishing updates](../dashboard/client-hub/publishing-updates.md).
{% endstep %}

{% step %}
### Test in game

On a Game Master character:

```
/make 0 120 9 1 1 4 0 0 0
```

Arguments: group `0`, index `120`, level `+9`, skill `1`, luck `1`, additional option `4` (+16), excellent mask `0`, ancient `0`, sockets `0`. Use an excellent mask such as `63` to get all six excellent options.

Check:

- the model shows in the inventory, on the ground and in the hand;
- the name, size and requirements in the tooltip;
- a Dark Knight can equip it and a Dark Wizard cannot;
- the damage on the character window changes when you equip it;
- the glow/effects if you added them.
{% endstep %}
{% endstepper %}

---

## Common mistakes

| Symptom | Cause | Fix |
|---|---|---|
| `/make` creates nothing | The row is not in the server `Item.txt`, or the GameServer was not restarted/reloaded. | Add the row; restart or **Reload Item**. |
| Item is invisible (empty square or nothing in the hand) | No `CustomItem.txt` row, wrong `FileName`, or the model file isn't in the client. | Check the row and that `Data/Item/<FileName>.bmd` exists in the client players have. |
| Item is white / has no texture | Texture files missing from `Data/Item/Texture/` (names must match the ones inside the model). | Add the textures with the exact names. |
| Several custom items lost their model at once | A row with an empty file name stops the list. | Remove or fix that row. |
| Name shows wrong or blank, size overlaps | Item.bmd not updated (or only in one language), or size differs from the server. | Edit Item.bmd in every language; match the server row. |
| Client lets players equip it but the server refuses (or the opposite) | Requirements or class flags differ between Item.txt and Item.bmd. | Make both identical. |
| Everything is correct but players still don't see it | Saved but not published, or players didn't restart. | **Publish update** → **Publish updates** → **Deploy**; restart the game. |
| Items after the new line behave strangely | Wrong column count in `Item.txt`. | Count the columns against the `//Index` header. |
| Glow row has no effect | `Texture` number not registered in `CustomImage.txt`, or wrong `GroupId`. | Register the texture; try `GroupId 0`, `1`, `2`… |
| Particles don't show | Edited the copy outside the **Effects** category, or the bone (`Join`) doesn't exist. | Edit the effect file in **Effects**; try other bone numbers. |
