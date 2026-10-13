# Mini tutorial: configure an EventItemBag

This tutorial builds two real bags from scratch, first by editing the files and then with the panel's EventItemBag editor:

- **Part A**: a box that players open by throwing it on the ground (**normal** model).
- **Part B**: a boss drop with several draws, per-class rewards and rolled options (**advanced** model).
- **Part C**: the same two bags made in the **EventItemBag editor**, checked with the chance calculator and the simulator.

The full column reference is on [EventItemBag](../muserver/data/event-item-bag.md) and the editor reference is on [EventItemBag editor](../dashboard/editors/event-item-bag.md). This page only walks through an example.

## Before you start

| You need | Where |
|---|---|
| A free bag number | The shipped bags use numbers up to `153`. This tutorial uses `160` and `161`. |
| The item that opens the box | Any item that exists in `Data/Item/Item.txt`. We use **Green Box** (group 14, index 157). |
| The monster that drops the boss bag | Any monster from `Data/Monster/Monster.txt`. We use **Dragon** (monster `44`). |
| A Game Master character | To create items with `/make` and summon monsters with `/spot` while testing. |

{% hint style="info" %}
**Item index.** The trigger file and the advanced item lists write an item as one number: `Group × 512 + Index`. Green Box = `14 × 512 + 157` = **7325**. Jewel of Bless (14, 13) = **7181**. See [File format basics](../muserver/file-format-basics.md).
{% endhint %}

---

## Part A: a simple box (normal model)

Goal: when a player throws a **Green Box** on the ground, it gives one of four rewards (Jewel of Bless, Jewel of Soul, Jewel of Life or a Dark Breaker +7 to +9), with an 80% chance of an item and a 20% chance of 500,000 Zen instead. Fireworks show when it opens.

{% stepper %}
{% step %}
### Pick the box item

Use an item that no other bag already uses as a trigger. Check `Data/EventItemBagManager.txt`: the `ItemIndex` column must not already contain `7325` (with the same level).

How a box opens: the player **drops the item from the inventory onto the ground**. The server then removes the item and opens the bag at that spot. This means:

- The item must be allowed to be dropped: it must **not** be listed with `AllowDrop 0` in `Data/Item/ItemMove.txt` ([Items](../muserver/data/items.md)).
- The box itself must not be excellent or ancient, and a box made from a weapon or armor (groups 0 to 11) must be +4 or lower. Otherwise the server refuses the drop.
{% endstep %}

{% step %}
### Add the trigger line

Open `Data/EventItemBagManager.txt` in the [File editor](../dashboard/file-editor.md) and add this line **before** the final `end`:

```
//Index   ItemIndex   ItemLevel   MonsterClass   SpecialValue   Comment
160       7325        0           *              *              //Green Box
end
```

- `160` = bag number. It links to the file `160 - ....txt`.
- `7325` = Green Box. `0` = only a Green Box +0 opens this bag (`*` = any level).
- `MonsterClass` and `SpecialValue` are `*` because this bag opens only by item.

{% hint style="warning" %}
Without this line the GameServer does not even load the bag file. A bag file with no trigger line never drops anything, and there is no error.
{% endhint %}
{% endstep %}

{% step %}
### Create the bag file

The panel's File editor edits existing files but does not create new ones, so create this bag with the **EventItemBag editor** (see [Part C](#part-c-the-same-bags-in-the-eventitembag-editor)) or add the file to `Data/EventItemBag/` directly. The name must follow the pattern three digits, space, dash, space:

```
Data/EventItemBag/160 - Green Box.txt
```

Content:

```
0
//EventName    DropZen   ItemDropRate  ItemDropCount  SetItemDropRate  ItemDropType  Fireworks
"Green Box"    500000    80            1              0                0             1
end

1
//Section  Type  MinLevel  MaxLevel  Skill  Luck  Option  Excellent
14         13    0         0         0      0     0       0          //Jewel of Bless
14         14    0         0         0      0     0       0          //Jewel of Soul
14         16    0         0         0      0     0       0          //Jewel of Life
0          17    7         9         1      1     2       1          //Dark Breaker +7~+9, skill, luck, +8, 1 excellent option
end
```

What each value of section `0` does here:

| Value | Column | Effect |
|---|---|---|
| `"Green Box"` | EventName | Name in the logs. Keep it under 32 characters. |
| `500000` | DropZen | Zen dropped when the item roll fails. |
| `80` | ItemDropRate | 80% item, 20% Zen. |
| `1` | ItemDropCount | One roll per box. `3` would give three independent rolls. |
| `0` | SetItemDropRate | No random ancient item. |
| `0` | ItemDropType | Options exactly as written in section 1. |
| `1` | Fireworks | Fireworks when the box opens. |

Section `1` is the item list: one line per possible item, all with the **same chance**. The Dark Breaker line rolls a level between 7 and 9, always has skill and luck, `+8` additional option (`2` × 4) and **one** excellent option picked at random.
{% endstep %}

{% step %}
### Do the math

Per box: 80% item × 1/4 lines = **20%** for each of the four items, and **20%** for 500,000 Zen.

To make one item rarer in the normal model, repeat the common lines (5 lines of Jewel of Bless + 1 Dark Breaker = Dark Breaker 1/6 of the item rolls), or use the advanced model, which has weights.
{% endstep %}

{% step %}
### Reload

On the [Dashboard](../dashboard/server-dashboard.md), open **Reload** on each GameServer and choose **Reload EventItemBag**, or type in game:

```
/reload eventitembag
```

Check the GameServer System Log: `[ServerInfo] EventItemBag loaded successfully` and **no red `[ItemBag]` line**. A red line names the file and the problem (missing `end`, wrong column count, unknown item).
{% endstep %}

{% step %}
### Test in game

On a Game Master character:

```
/make -10 14 157 0 0 0 0 0 0 0
```

This creates **ten** Green Boxes +0 (a negative first number is the quantity). Throw them on the ground one by one. You should see fireworks and, over ten boxes, roughly eight items and two Zen drops.
{% endstep %}
{% endstepper %}

---

## Part B: a boss drop (advanced model)

Goal: when **Dragon** (monster `44`) dies:

- **Draw 0 (always)**: everyone gets something. 70% of the time a jewel; 30% of the time an excellent sword, but only for Dark Knights after their 2nd class change (Dark Breaker) and Magic Gladiators (Rune Blade).
- **Draw 1 (25%)**: a bonus Jewel of Creation, delivered straight to the killer's inventory.

{% stepper %}
{% step %}
### Add the trigger line

In `Data/EventItemBagManager.txt`, before `end`:

```
//Index   ItemIndex   ItemLevel   MonsterClass   SpecialValue   Comment
161       *           *           44             *              //Dragon boss drop
end
```

{% hint style="warning" %}
A monster trigger **replaces** that monster's normal drop. Dragons will no longer drop the usual items from the drop tables, only this bag.
{% endhint %}
{% endstep %}

{% step %}
### Create the bag file

File `Data/EventItemBag/161 - Dragon Boss.txt`:

```
3
//Index  DropRate  DropInventory
0        10000     0
1        2500      1
end

4
//Index  Section  Rate   Money    OptionValue  DW DK FE MG DL SU RF
0        5        7000   0        1            1  1  1  1  1  1  1
0        6        3000   0        1            0  2  0  1  0  0  0
1        7        10000  1000000  0            1  1  1  1  1  1  1
end

5
//Index  Level  Grade  Option0  Option1  Option2  Option3  Option4  Option5  Option6  Duration
7181     0      0      *        *        *        *        *        *        *        0      // Jewel of Bless
7182     0      0      *        *        *        *        *        *        *        0      // Jewel of Soul
7184     0      0      *        *        *        *        *        *        *        0      // Jewel of Life
end

6
//Index  Level  Grade  Option0  Option1  Option2  Option3  Option4  Option5  Option6  Duration
0017     0      0      31       12       13       20       18       *        *        0      // Dark Breaker
0031     0      0      31       12       13       20       18       *        *        0      // Rune Blade
end

7
//Index  Level  Grade  Option0  Option1  Option2  Option3  Option4  Option5  Option6  Duration
7190     0      0      *        *        *        *        *        *        *        0      // Jewel of Creation
end
```

There is no section `0`/`1`/`2` here: an advanced bag only needs sections 3, 4 and the item lists.
{% endstep %}

{% step %}
### Read it section by section

**Section 3, the draws.** Each line is an independent roll out of 10,000:

| Draw | DropRate | DropInventory | Meaning |
|---|---|---|---|
| `0` | `10000` | `0` | Always happens; the item drops on the ground. |
| `1` | `2500` | `1` | 25% chance; the item goes into the killer's inventory (on the ground if it does not fit). |

**Section 4, who gets what.** Lines with the same `Index` belong to the same draw. The server keeps only the lines whose class flags accept the player, then picks one of them by `Rate` (a relative weight).

| Line | Draw | List | Rate | Classes | Meaning |
|---|---|---|---|---|---|
| 1 | 0 | 5 | 7000 | all `1` | Jewels for every class. |
| 2 | 0 | 6 | 3000 | DK `2`, MG `1` | Swords for Dark Knights **after the 2nd class change** and for Magic Gladiators from the start. |
| 3 | 1 | 7 | 10000 | all `1` | The bonus jewel. `Money 1000000` = 1,000,000 Zen if list 7 is missing or empty. |

`OptionValue 1` on lines 1 and 2 = fireworks when the item drops.

**Sections 5, 6, 7, the item lists.** One item is picked at random (same chance for each line). The `Option0`–`Option6` columns are **rows of `Data/Item/ItemOptionRate.txt`**:

| Column | Value used | Row in ItemOptionRate.txt | Result |
|---|---|---|---|
| Option0 (level) | `31` | `(EventItemBag) (2~4)` | +2, +3 or +4 |
| Option1 (skill) | `12` | `(EventItemBag) (1)` | always skill |
| Option2 (luck) | `13` | `(EventItemBag) (0~1)` | luck 50/50 |
| Option3 (additional) | `20` | `(EventItemBag) (0~2)` | +0, +4 or +8 |
| Option4 (excellent) | `18` | `(EventItemBag) (0~1)` | 0 or 1 excellent option |
| Option5 (ancient) | `*` | none | never ancient |
| Option6 (socket) | `*` | none | no sockets |

The jewels use `*` everywhere: nothing is rolled, they drop as plain +0 jewels.

{% hint style="danger" %}
**`0` is not "none".** Row `0` of every section of `ItemOptionRate.txt` is the "(Common)" row used by normal monster drops, and it rolls (about 6% skill, 4% luck, 4% additional option). Write `*` for "do not roll", or use an `(EventItemBag)` row (`11` = never, `12` = always in the skill and luck sections).
{% endhint %}
{% endstep %}

{% step %}
### Do the math

Chance of one item = `DropRate/10000 × Rate / (sum of the Rates valid for the class) × 1/(items in the list)`.

| Killer | Draw 0 | Draw 1 |
|---|---|---|
| Dark Wizard | Only line 1 is valid → 100% list 5 → each jewel **33.3%** | Jewel of Creation **25%** |
| Dark Knight (2nd class) | Lines 1 and 2 → 7000/10000 = 70% list 5 (23.3% per jewel), 30% list 6 (15% Dark Breaker, 15% Rune Blade) | **25%** |
| Dark Knight (base class) | Line 2 needs DK `2` → only line 1 → 100% list 5 | **25%** |
| Magic Gladiator | Lines 1 and 2 → same as the 2nd-class Dark Knight | **25%** |

{% hint style="info" %}
A Dark Knight can win a Rune Blade he can't equip, because list 6 is shared. To give each class only its own weapon, split it: list 6 = Dark Breaker with DK `2` only, list 8 = Rune Blade with MG `1` only, each rule with its own weight.
{% endhint %}
{% endstep %}

{% step %}
### Reload and test

```
/reload eventitembag
```

Then on a Game Master character, go to an open area and summon a few Dragons:

```
/spot 44 5
```

Kill them with characters of different classes and compare what drops with the table above. The System Log also confirms how the file was read: `[ItemBag] <file>: secao 3 com N colunas` and `secao 4 com N coluna(s) de classe`.
{% endstep %}
{% endstepper %}

---

## Part C: the same bags in the EventItemBag editor

The [EventItemBag editor](../dashboard/editors/event-item-bag.md) (**Editors → EventItemBag Editor**) writes both files for you, creates the trigger line, and shows the real chances.

{% stepper %}
{% step %}
### Create the box (normal model)

1. Click **+ New**.
2. **Number** `160`, **Name** `Green Box` (the dialog shows `File: 160 - Green Box.txt`).
3. Model: **Normal**.
4. **When it opens**: **On item use** → pick **Green Box** and **Only +0**.
5. Click **Create**.
{% endstep %}

{% step %}
### Fill the Content tab

**Opening rules**: **Event name** `Green Box`, **Item chance (%)** `80`, **Zen when no item drops** `500000`, **Rolls per opening** `1`, **Ancient chance (/10000)** `0`, **Item options** **Fixed (as in the table)**, **Fireworks** on.

**Items**: click **+ Add items**, pick Jewel of Bless, Jewel of Soul, Jewel of Life and Dark Breaker. On the Dark Breaker row set **Level** 7 to 9, tick **Skill** and **Luck**, **Option** +8, **Excellent** 1.

The **Chance** column shows 20% on each row.
{% endstep %}

{% step %}
### Check the Trigger tab

The **When an item is used (or dropped)** section shows Green Box, **Only +0**. If another bag with a lower number has the same trigger, the tab shows **Same trigger in:** and the status panel says the bag never opens. Change one of the triggers.
{% endstep %}

{% step %}
### Create the boss bag (advanced model)

1. **+ New** → **Number** `161`, **Name** `Dragon Boss`, model **Advanced**, **When it opens**: **On monster kill** → search `44` / Dragon → **Create**.
2. **Settings**: **Class columns** → **7 classes (DW to RF)**.
3. **Draws**: **+ Draw** twice. Draw `0` with **Chance (/10000)** `10000`; draw `1` with `2500` and **Straight to inventory** ticked.
4. **Item lists**: **+ List** three times (lists 5, 6 and 7) and add the items. In list 6 pick the option rows by name: **Level** `(EventItemBag) (2~4)`, **Skill** `(EventItemBag) (1)`, **Luck** `(EventItemBag) (0~1)`, **Option** `(EventItemBag) (0~2)`, **Excellent** `(EventItemBag) (0~1)`, **Ancient** and **Socket** `* (not rolled)`. **Duration** **Permanent**.
5. **Rules**: in draw 0, **+ Rule** → **List** 5, **Weight** 7000, **Fireworks**, all classes `1`. **+ Rule** → **List** 6, **Weight** 3000, **Fireworks**, click DK until it shows `2`, MG `1`, others off. In draw 1, **+ Rule** → **List** 7, **Weight** 10000, **Zen** 1000000, all classes `1`.
{% endstep %}

{% step %}
### Check the Chances tab and run the simulator

1. Open **Chances**. For the advanced bag choose **Class** = Dark Knight and **Evolution** = 2nd class: you should see 23.3% per jewel, 15% per sword and 25% Jewel of Creation. Switch to Dark Wizard: the swords disappear.
2. Click **Simulate 1000 openings**. The **In 1000** column should land close to the percentages (for example about 150 Dark Breakers for the Dark Knight). **Clear simulation** removes it.
{% endstep %}

{% step %}
### Save and reload

1. Click **Save** (*Saved: 2 files written… Use "Reload EventItemBag" to apply it in game.*).
2. Click **Reload EventItemBag** (for your GameServer). Do it on every GameServer that hosts the map where the box is used or the boss lives.
3. Test in game exactly as in Parts A and B.
{% endstep %}
{% endstepper %}

---

## Common mistakes

| Mistake | What you see | Fix |
|---|---|---|
| No line in `EventItemBagManager.txt` | Nothing drops, no error. The editor lists the bag under **No trigger (not loaded)**. | Add the trigger line (**Create trigger** in the Trigger tab). |
| Missing `end` at the end of a section | The **whole bag** is discarded, with a red `[ItemBag]` line in the log. | Close every section with `end`. The editor closes them when it saves. |
| Two files with the same number (`160 - Green Box.txt` and `160 - Old Box.txt`) | Only the first in alphabetical order is loaded; the other is ignored. | Give every bag its own number. Disable old files with `_` at the start of the name. |
| Wrong file name (`160-Green Box.txt`, `Green Box 160.txt`) | File ignored. | Use exactly `NNN - Name.txt`. |
| `ItemDropRate 100` in a normal bag | `DropZen` never drops. | Use a value below 100 if you want Zen sometimes. |
| `0` in the `Option0`–`Option6` columns of an advanced list | Items come with skill, luck or additional options you didn't ask for. | Use `*` (no roll) or an `(EventItemBag)` row. |
| Sections 0/1 and section 3 in the same file | The normal part is ignored; only the advanced part works. | Use one model per file. |
| Box marked `AllowDrop 0` in `ItemMove.txt`, or the box is excellent/ancient | The box can't be thrown, so it never opens. | Remove it from `ItemMove.txt` or use a plain +0 box. |
| A class flag of `0` on every rule of a draw for that class | That class gets nothing from the draw. The Chances tab says *This bag gives nothing to this class.* | Add a rule that accepts the class. |
| Edited the file but didn't reload | Old drops. | **Reload EventItemBag** on each GameServer. |
