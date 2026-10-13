# EventItemBag editor

**Boxes, boss drops and event rewards:** what drops, at what chance and when the bag opens. Both GameServer models (**normal** and **advanced**) are edited in one place, with the real chances calculated and the trigger file (`EventItemBagManager.txt`) alongside.

For the file format and every column, see the MuServer reference: [EventItemBag](../../muserver/data/event-item-bag.md).

## Concepts

| Concept | Meaning |
|---|---|
| **Bag** | One file `Data/EventItemBag/NNN - Name.txt`. The 3-digit number identifies it. |
| **Trigger** | A line in `Data/EventItemBagManager.txt` saying **when** bag NNN opens: when an item is used, when a monster dies, or on an event. **Without a trigger, the GameServer does not even load the bag.** |
| **Normal model** | One item list picked evenly, with an item-or-Zen chance and N rolls per opening. Simple. |
| **Advanced model** | Independent **draws**, each with its own chance → per-class **rules** with weights → **item lists** whose options are rolled from `ItemOptionRate.txt`. Flexible. |

{% hint style="info" %}
If a bag has both models, the **advanced** one wins (from the normal part only "Rolls per opening" still counts, for reset rewards).
{% endhint %}

## Screen layout

- **Top bar**: Save → Reload bar (**Reload EventItemBag · &lt;GameServer&gt;**), re-read from disk and full screen, plus a summary like `48 bags · 51 triggers · 612 items`.
- **Status panel**: all problems found; when clean: **All good: the GameServer reads every bag exactly as shown here.**
- **Bags** pane (left) and **Edit** pane (right).

### The bag list

- Search with **Search number, name, item, monster…**.
- Filter: **All bags**, **Opened by item**, **Opened by monster**, **Opened by event**, **No trigger (not loaded)**, **Normal model**, **Advanced model**, **With errors**, **Changed**.
- Each entry shows the number, name, model (**Nor** / **Adv**), the trigger (`Item: Box of Kundun +3`, `Monster: Kundun`, `Event: Blood Castle`) or **Not in the manager: not loaded**, and a **Changed** flag.
- **+ New** creates a bag. **Triggers (EventItemBagManager.txt)** opens the whole trigger table.

## Creating a bag

1. Click **+ New** (**Create a new bag**).
2. In **New bag**, set:
   - **Number** – must be free (*A bag with this number already exists.*).
   - **Name** – e.g. *Christmas Event Box*. The file name is shown (`File: 120 - Christmas Event Box.txt`).
   - Model – **Advanced** (*Several draws with their own chances, per-class rules with weights and rolled options. Recommended.*) or **Normal** (*A simple list: each roll gives a random item from the list, or Zen.*).
   - **When it opens** – **On item use** (pick the item and level, or **Any level**), **On monster kill** (pick the monster), **On event** (pick the event), or **Later**.
3. Click **Create**. The bag is marked **New file** until you save.

{% hint style="warning" %}
With **Later**, an empty trigger line is created: the bag loads but does not open until you set a trigger in the **Trigger** tab.
{% endhint %}

## Duplicate, rename, delete, convert

At the top of the Edit pane, next to the file path (`Data/EventItemBag/120 - Christmas Event Box.txt`):

| Button | What it does |
|---|---|
| **Duplicate** | Opens **Duplicate bag** with a new number and the name `<name> (copy)`. |
| **Delete** | Confirms **Delete bag?** — the file is deleted on save (a copy stays in the backup history) and its trigger line is removed if no other bag uses that number. Pending deletions are listed: *Will be deleted on save: …* |
| **Convert to advanced** | Only for normal bags. See [Converting normal → advanced](#converting-normal-to-advanced). |
| Rename | In the **File** tab, change **Number** (renames the file and moves the trigger along) or **File name** (the text after `NNN - `). The header shows `(was <old file>)` until you save. |

## Tabs of a bag

Each bag has four tabs: **Content**, **Trigger**, **Chances**, **File**. The tab bar shows error and warning counts.

An empty bag offers **Start with the normal model** or **Start with the advanced model**. A bag with both models shows **This bag has both models:** with the part **in use** and a **Remove the … part** button.

### Content tab — normal model

**Opening rules (section 0)** – a single line; each opening makes "Rolls per opening" rolls; each roll gives an item, an Ancient or Zen.

| Field | Description |
|---|---|
| **Event name** | Up to 31 characters (above that the GameServer closes while loading). |
| **Item chance (%)** | Chance that a roll gives an item. *In the other N% the Zen below drops.* **100%: an item always drops, the Zen never does.** |
| **Zen when no item drops** | Zen amount when the item roll fails. |
| **Rolls per opening** | How many times the bag rolls per opening (1 to 50). |
| **Ancient chance (/10000)** | Chance per roll of a random Ancient, before the list. |
| **Item options** | **Fixed (as in the table)** – Skill, Luck and option drop exactly as checked. **Random** – checked Skill and Luck become 50%, the option is rolled, an Excellent item always drops with Skill. |
| **Fireworks** | Shows fireworks to whoever opened it. |

**Items (N)** – *When the roll gives an item, one of these drops, all with the same chance.* Click **+ Add items** to add several items at once.

| Column | Description |
|---|---|
| **Item** | Item with icon. |
| **Level** | Level rolled between the minimum and the maximum. |
| **Skill** / **Luck** | Item drops with Skill / Luck. |
| **Option** | Additional option (+4 to +28). |
| **Excellent** | **How many** Excellent options the item gets, picked among the 6. |
| **Ancient** / **Socket** | Shown with **Ancient and Socket**: drops as Ancient (if the item has a set); sockets rolled or N empty. |
| **Chance** | Chance of this item dropping at least once in one opening. |

{% hint style="info" %}
In the normal model every item has the same chance. To make an item rarer, use the advanced model (weights per rule) or repeat the common items.
{% endhint %}

**Bulk edit:** tick several rows (`5 selected`) and use **Level…**, **With Skill**, **With Luck**, **Option…**, **Excellent…**, **Clear options**, **Remove** or **Unselect**. Use **Filter items…** to narrow the list first.

### Content tab — advanced model

*Each DRAW rolls its own chance. If it passes, the GameServer picks ONE RULE of that draw by weight, among those valid for the player's class; the rule points to a LIST, and one random item drops from it (or all of them, with "Whole list"). Several draws = several drops per opening.*

**Settings**

- **Class columns** – **7 classes (DW to RF)** or **14 classes (DW to KIN)**; use 14 if your server has the newer classes.
- **"Straight to inventory" column** – turned on automatically when a draw delivers to the inventory.

**Draws (N)** – add with **+ Draw**.

| Field | Description |
|---|---|
| **Draw** | Draw number (must be unique). |
| **Chance (/10000)** | Chance of this draw happening (shown as %). |
| **Straight to inventory** | Delivers to the inventory; if it doesn't fit, it drops on the ground. |

**Rules** inside each draw – add with **+ Rule**.

| Field | Description |
|---|---|
| **List** | Item list the rule uses (`List 5 (12 items)`), or **+ New list…**. |
| **Weight** | Relative weight among the draw's rules (9000 and 1000 = 90% and 10%). The **%** column shows the share. |
| **Zen** | Zen that drops if the list doesn't exist or is empty. |
| **Options** | **Fireworks**, **Party class** (valid if anyone in the party has a checked class), **Whole list** (drops all items of the list), **Ancient** (replaces the item with a random Ancient). |
| **Classes** | Click each class to toggle: off = does not receive; **1** = receives; **2, 3, 4** = only from that evolution on. |

**Item lists (N)** – add with **+ List**. Each list shows which draws use it (or **No rule uses this list.**).

| Column | Description |
|---|---|
| **Item** | Item with icon. |
| **Level** | Base level (the ItemOptionRate Level column may roll another). |
| **Grade** | Fixed Excellent options (bitmask), used only when the Excellent column rolls none. |
| Option columns | **Level**, **Skill**, **Luck**, **Option**, **Excellent**, **Ancient**, **Socket** — each picks a row of `ItemOptionRate.txt` by its name, or `* (not rolled)`. |
| **Duration** | Temporary item: **Permanent**, **1 hour**, **1 day**, **7 days**, **30 days** or custom seconds. |

Bulk edit in lists: tick rows and use **Apply…** (an option row), **Duration…**, **Remove** or **Unselect**.

### Trigger tab

Edits the bag's line in `EventItemBagManager.txt`. *A bag can open by item, monster and event at the same time. If two bags share a trigger, the GameServer uses the one with the lowest number.*

| Section | Fields |
|---|---|
| **When an item is used (or dropped)** | The item and **Item level** (**Any level** or **Only +N**). For boxes, medals, gifts. |
| **When a monster dies** | **Search monster** by name or number. *Replaces the monster's normal drop with this bag.* |
| **On an event** | Blood Castle, Chaos Castle, Illusion Temple, mixes, Lucky Coin, reset reward… or **Other number…** (SpecialValue). |
| **Comment** | Text after `//` in the manager line. Empty = the bag name. |

If the bag has no line, you see *Bag N has no line in EventItemBagManager.txt: the GameServer does not even load the file.* Click **Create trigger**.

{% hint style="warning" %}
**Shadowed triggers.** When another bag with a lower number has the same trigger, the editor shows **Same trigger in:** with **N wins**, and the status panel says *Bag 120 never opens by item: bag 015 has the same trigger and a lower number.* Change one of the triggers.
{% endhint %}

{% hint style="info" %}
**Reset reward:** the prize goes straight to the inventory, and the number of prizes is section 0's "Rolls per opening" — even in the advanced model.
{% endhint %}

### Chances tab

Calculates, **with the same algorithm as the GameServer**, the chance of each result in one opening.

- Normal: *item chance × 1/list size, repeated on every roll.*
- Advanced: choose **Class** and **Evolution** — *draw chance × rule weight ÷ sum of the weights valid for the class × 1/list size.*

| Column | Meaning |
|---|---|
| **Result** | Item, **Zen (amount)** or **Random Ancient**. |
| **Chance** | Chance of dropping at least once in one opening (also shown as `1 in N`). |
| **Average** | How many drop, on average, per opening. |

Summary figures: **Drops per opening (average)**, **Opening with nothing** and **Possible results**. If the class gets nothing: *This bag gives nothing to this class.*

**Simulator:** click **Simulate 1000 openings** to open the bag a thousand times; the **In 1000** column shows how many times each result dropped, confirming the math. **Clear simulation** removes it.

### File tab

- **Number** and **File name** (rename).
- **File header comment** – lines before the first section; saved as `//` comments.
- **Lines the editor could not understand** – lines the GameServer would read shifted (wrong column count, non-numeric value, line outside a section, bad `end`). Each has **Remove line**. **While any remains, the bag cannot be saved.** Recreate the item in the Content tab and remove the bad line.
- If a section doesn't end with `end`, the editor warns that the GameServer currently discards the whole bag; saving closes the section.

*Lines you did not edit are written exactly as they are, with spacing, comments and commented-out lines. Only changed lines are reformatted.*

## The trigger table

Click **Triggers (EventItemBagManager.txt)** in the bag list to see all triggers at once: **No.**, **Bag**, **Item** (`*` = not used), **Level**, **Monster**, **Event**, **Comment**. Add lines with **+ Line**, remove with **Remove**. Unreadable lines are listed so you can fix them. If the file doesn't exist, it is created on save.

## Converting normal to advanced

1. Open a normal bag and click **Convert to advanced** (*Generates the draws, rules and list of the advanced model with the same effect.*).
2. Read the notes in **Convert to the advanced model?** — the list becomes List 5, each roll becomes a draw with the same chance, and Skill/Luck/Option/Excellent become "always" rows of ItemOptionRate. Possible notes:
   - *The failure Zen does not exist in the advanced model: when the draw fails, nothing drops.*
   - *"Random options" has no direct equivalent.*
   - *Items with a level range got the minimum level.*
   - *Options with no matching "always X" row were left unrolled (\*).*
   - *Fireworks became the "Fireworks" option on each rule.*
3. Click **Convert**, review the Content and **Chances** tabs, then **Save**.

## Saving and applying

1. Click **Save**. Only changed files are rewritten, all or nothing, with a backup copy. Message: *Saved: 2 files written, 1 deleted. Use "Reload EventItemBag" to apply it in game.*
2. Click **Reload EventItemBag · &lt;GameServer&gt;** (marked **needed**). It re-reads the manager and every bag without disconnecting anyone.

If the save is rejected (*The server rejected the save. See the errors in the status panel.*), fix the listed errors — for example a generated line the GameServer would not read — and save again. If someone changed a bag file meanwhile, choose **Reload (lose my changes)** or **Overwrite anyway**.

## Common problems

| Status message | Fix |
|---|---|
| **Bag N is not in EventItemBagManager.txt: the GameServer does not even load the file.** | Create the trigger in the Trigger tab. |
| **Bag N: no trigger (item, monster or event); it never opens.** | Set at least one trigger. |
| **Number N appears more than once in the manager: the GameServer only uses the first line.** | Remove the duplicate line in the trigger table. |
| **There are 2 files with number N: the GameServer loads only one of them.** | Rename or delete one file. |
| **Bag N: with a 100% item chance, the Zen never drops.** | Informational; lower the item chance if you want Zen. |
| **Bag N: N items, the GameServer keeps only max.** | Split into more bags or use the advanced model. |
| **Bag N: rule of draw D (list L) with no class; nobody receives it.** | Tick at least one class. |
| **Bag N: rule of draw D (list L) with weight 0; it is never picked.** | Set a weight above 0. |
| **"file" is not .txt but the GameServer opens this file too.** | Rename stray files to start with `_`. |
