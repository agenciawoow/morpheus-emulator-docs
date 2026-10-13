# NPC Shop editor

**Build the shop while seeing the grid exactly as the player will.** The NPC Shop editor shows each NPC's shop as the in-game grid, with real item icons, and warns you when an item would silently disappear.

For the file format and all columns, see the MuServer reference: [Shops](../../muserver/data/shops.md).

## How item positions work

In a MuServer shop file, **the order of the rows defines the position** of each item in the grid. The GameServer fills the grid left-to-right, top-to-bottom, putting each item in the first place where it fits (a 2×4 armor takes 8 cells). That's why:

- Reordering the list changes where items appear.
- A big item late in the list may **no longer fit** and is dropped silently by the GameServer.

The editor simulates this packing for you.

## Opening a shop

1. Go to **Editors → NPC Shop editor** (**Loading the server catalog…**).
2. In the **NPC** selector, pick the shop. Each entry shows the NPC class and name (for example `253 — Amy`).
3. Under the selector you see the shop file, `shop N` and the `map N` where the NPC stands.

{% hint style="warning" %}
If you switch NPCs with unsaved changes, the editor asks first: *You have unsaved changes in this shop. Switching NPCs will lose them.*
{% endhint %}

## Screen layout

| Area | Content |
|---|---|
| **Shop grid** | The grid as players see it, with item icons. The counter shows `N items` and `used/total cells`. Click an item to select its row. |
| **Shop items** | The list of rows — *the order defines the position · drag to reorder*. |
| Item properties | The fields of the selected row. |

## Adding items

1. Click **Add item**.
2. In the picker, search by name or `group,index` and click the items to add (several at once).
3. Click **Done**. New items are appended at the end of the list.
4. Adjust their properties and click **Save**.

## Item properties

| Field | Meaning |
|---|---|
| **Level** | Item level (+0 to +15). |
| **Durab.** | Durability. **0 = uses the item's default durability.** |
| **Skill** | Item comes with skill. |
| **Luck** | Item comes with luck. |
| **Option** | Additional option (**+4 additional per point**). |
| **Excellent** | **Bitmask of the 6 options** (0 = none, 63 = all six). |
| **Ancient** | Ancient set option (16-column files only). |
| **Harmony** | Jewel of Harmony option (16-column files only). |

Use **Remove** on a row to delete it.

## Reordering and auto-arranging

- **Drag to reorder**: drag a row in the list up or down; the grid updates immediately.
- **Auto-arrange**: *Reorders to close gaps in the grid. It does not change any item.* Use it after adding or removing big items to pack the grid tightly.

## Items that don't show up

If some rows would not appear in the game, a red box lists them, for example **2 items do not show up in game**:

| Reason | Meaning | Fix |
|---|---|---|
| **does not exist in Item.txt** | The item code is not in the server's item list. | Remove the row or fix the item. |
| **no longer fits in the grid** | There's no room left when the GameServer reaches that row. | Remove other items, use **Auto-arrange**, or move the row earlier. |

Each entry shows `row N (group,index)` so you can find it.

## 8-column vs 16-column files

Older shop files have **8 columns** and don't support Ancient, Harmony, 380 options or sockets. When the editor opens such a file it:

- shows a **Warning** box,
- shows **two grids**: **As you wrote it** and **What the server loads today** (the GameServer reads 8-column files differently, pairing rows),
- offers **Convert to 16 columns**.

Click **Convert to 16 columns** to rewrite the file in the modern format; then the extra fields become available.

{% hint style="info" %}
If the editor detects **Conflicting files in the Shop folder** (for example two files for the same shop number), it lists them at the top. Clean up the extra files in the [File editor](../file-editor.md) to avoid surprises.
{% endhint %}

## Saving and applying

1. Click **Save** (highlighted when there are changes).
2. On the [Dashboard](../server-dashboard.md), open **Reload** on each GameServer and choose **Reload Shop**.
3. Players see the new shop the next time they open the NPC — nobody is disconnected.

## Tips

- Put large items (armors, wings) first and small items (potions, jewels) last to avoid gaps.
- Keep the same shop consistent across GameServers that host the same map.
- Create a [backup](../backups-and-restore.md) before rearranging many shops.
