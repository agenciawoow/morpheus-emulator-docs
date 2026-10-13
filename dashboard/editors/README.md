# Visual editors

The **Editors** menu groups visual editors for the parts of the MuServer you change most often. Instead of counting TAB-separated columns in a `.txt` file, you see items with their real names and icons, maps with their real terrain, and the exact chances players get — and the editor writes the files in the format the GameServer expects.

| Editor | Sidebar label | What you edit | MuServer reference |
|---|---|---|---|
| [NPC Shop editor](npc-shop.md) | **NPC Shop editor** | What each NPC sells, with the grid exactly as players see it. | [Shops](../../muserver/data/shops.md) |
| [Spot editor](spot.md) | **Spot editor** | Where monsters and NPCs spawn, over the real map. | [Monsters](../../muserver/data/monsters.md) |
| [Cash Shop (X)](cash-shop.md) | **Cash Shop (X)** | The in-game cash shop: packages, variants, prices, categories. | [Cash Shop](../../muserver/data/cash-shop.md) |
| [Morpheus Shop](morpheus-shop.md) | **Morpheus Shop** | The Morpheus item shop: items, prices per duration and coin, options, limits, discounts, texts. | [Morpheus Shop](../../muserver/customs/morpheus-shop.md) |
| [Battle Pass](battle-pass.md) | **Battle Pass** | Seasons, prizes, track, missions, calendar, plans and XP sources. | [Battle Pass](../../muserver/customs/battle-pass.md) |
| [EventItemBag](event-item-bag.md) | **EventItemBag Editor** | Boxes, boss drops and event rewards: what drops, at what chance and when. | [EventItemBag](../../muserver/data/event-item-bag.md) |

{% hint style="info" %}
{% endhint %}

## Features shared by all editors

### Built-in guide

Most editors open with a collapsible guide at the top, for example **How does the Cash Shop (X) work?** or **How does EventItemBag work?**, explaining the concepts and the steps in order. Use **hide** / **show** to collapse it once you know the workflow.

### Three-pane layout

The newer editors use three panes: a navigation pane (categories, groups, sections or bags), a list, and a details/edit pane. On small screens the panes stack.

### Full screen

Click the full-screen icon (**Fullscreen**) to give the editor the whole browser window. Press **Esc** (**Exit fullscreen (Esc)**) to return.

### Item picker

Wherever you add items, the item picker opens with **Search by name, or by group,index (e.g. 0,3)** and category tabs (**Swords**, **Axes**, **Maces**, … **Miscellaneous**). It shows each item's icon and inventory size (`takes up 2×4`). You can click several items in a row (`3 added`) and then **Done**. Only the first 400 matches are listed — refine your search if needed.

### Status panel

A status panel above the content lists what the GameServer would complain about: `2 warning(s)` or `1 error(s) and 3 warning(s)`. Click **view** to expand it, **go** to jump to the problem and **hide** to collapse it. Errors block saving. See [Validation and conflicts](../validation-and-conflicts.md).

Some editors also show **switch chips** — for features that need a switch turned on both on the server and in the client (Morpheus Shop, Battle Pass), the chips show **on**, **OFF** or **?** for each side.

### Safe saving

- **Unsaved changes** are flagged and you're asked before anything discards them.
- **Only changed files are rewritten**, and editors such as EventItemBag preserve comments and formatting of lines you didn't touch.
- **All or nothing**: when an editor writes several files (for example engine + season + language files), either all are saved or none.
- **Backup**: each rewritten file is copied to the backup history first.
- **Conflict protection**: if someone changes the files on the server after you opened the editor, the save is stopped and you choose **Reload from server** or **Overwrite anyway**. See [Conflicts](../validation-and-conflicts.md#conflicts-the-file-changed-on-the-server).
- **Re-read from disk**: the circular-arrow button reloads the files from the server (discarding unsaved changes).

### Bulk price adjustment

In **Cash Shop (X)** and **Morpheus Shop**, tick entries in the list and click **Adjust prices (N)**. The **Bulk price adjustment** window works in three steps:

1. **How to adjust** – **Set value** (every selected price becomes the same), **Increase/decrease %** (e.g. +10%, or −25% for a sale), **Add/subtract** (e.g. +50 coins) or **Round** (multiples of 10, 50, 100…).
2. **How much and where** – the value, optional **Round result to**, **Leave 0 prices (unavailable) untouched**, and **Apply to** (which price groups).
3. **Before and after** – a preview table with **Before** / **After** per item, plus counters: `12 will change`, `3 unchanged`, `1 invalid` (would become negative and are skipped).

Click **Apply to N**, then **Save**. (*N price(s) adjusted. Remember to save.*)

## The Save, Reload and Publish bar

Cash Shop (X), Morpheus Shop, Battle Pass and EventItemBag have a fixed bar at the top that takes your change all the way to the players, step by step:

```
[ Save ]   After saving:   [ Reload <GameServer> ]   [ Publish to client ]
```

| Part | What it shows / does |
|---|---|
| Save state | **unsaved changes** while editing, or **saved at 14:32** after saving. |
| **Save** | Writes the files (disabled while there are errors or a conflict). |
| **After saving:** reload button(s) | One button per GameServer that supports reload, for example **Reload EventItemBag · GameServer** or **Reload custom (GameServer)**. Hover to read what it does. Confirm **Reload on the GameServer?** — *the GameServer will re-read the files now. Nobody gets disconnected.* The step then shows **reloading…** and **done at 14:33**. |
| **needed** badge | The last save changed something that only this reload delivers to players. It stays until you run the reload. |
| **Publish to client** | Only in editors that also change client files. Builds the client update and uploads it to your update hosting; the players' launcher downloads it on next start. Confirm **Publish to the client?** (it can take a few minutes; **publishing…**). |

Possible notes on the reload step:

| Note | Meaning |
|---|---|
| **&lt;GameServer&gt; is stopped** | Start the GameServer first; there's nothing to reload. |
| **no GameServer with reload** | No GameServer on your server supports hot reload. Contact support. |

{% hint style="info" %}
Saving again resets the reload and publish steps: players don't have the newest version until you reload/publish again.
{% endhint %}

Which steps each editor needs:

| Editor | Reload module | Publish to client? |
|---|---|---|
| NPC Shop | **Reload Shop** (from the Dashboard) | No |
| Spot | **Reload Monster** (from the Dashboard) | No |
| Cash Shop (X) | **cashshop** (in the bar) | Yes – the shop scripts are client files. |
| Morpheus Shop | **custom** (in the bar) | Only when the item **list** changes (**Encode + Publish to client**). |
| Battle Pass | **custom**, plus **lang** if a text changed | No |
| EventItemBag | **eventitembag** (in the bar) | No |

For NPC Shop and Spot, reload from the [Dashboard](../services-and-reload.md#hot-reload).
