# Client files

**Edit the client files (server list and friends) and publish the update to your players.** Open **Client Hub → Client Files**.

For details about each client file, see the MuServer reference: [Client files](../../muserver/client/README.md).

## Page layout

A yellow notice at the top appears when files are pending: **N file(s) pending publication for this server. Review and publish.** (the link opens [Publish updates](publishing-updates.md)). Below it, four tabs:

1. **Editors** – pick a client file and edit it with its visual editor.
2. **BMD files** – client files without a visual editor (download / upload).
3. **Data files** – send models, textures and terrain attributes to the client's `Data` folder. See [Data files](#data-files).
4. **History** – previous publications with **Version**, **When**, **By**, **Status**, **Files**, **Time** and **Details**.

## Files with a visual editor

| File | What it controls |
|---|---|
| **ServerList.bmd** | Server list shown after login (name, position, description, NonPvp per slot). |
| **Text.bmd** (per language) | Pool of localised strings that feeds the whole UI (chat, NPCs, error messages, buttons). |
| **Item.bmd** (per language) | Master item catalog: name, sprite, class, damage, defense, requirements, drop/trade/store flags. |
| **MoveReq** (per language) | Maps of the `/move` window (M key). |
| **ItemTooltip** | Tooltip of each item: name, colour and up to 12 text lines. |
| **ItemTooltipText** (per language) | Phrases used by the tooltip lines. |
| **Mix** | Mix window recipes for the 14 NPC types. |

{% hint style="info" %}
Every save on this page ends with a confirmation such as **Saved (English, Portuguese) — waiting to be published.** This means the file is saved on your server and is now waiting in the [publish queue](publishing-updates.md).
{% endhint %}

## Server list (ServerList.bmd)

| Column | Description |
|---|---|
| **Index** | Unique server ID (0–65535). Must never be duplicated. |
| **Name** | Text shown in the Select Server list. Max 31 characters. |
| **Pos** | Visual position/grouping (0–255). Servers with the same Pos form one group. |
| **Sequence** | Display order within the Pos group (lower = first). |
| **Description** | Free text shown on hover/tooltip. Empty = no description. |
| **NonPvp per slot** | Each slot controls the PvP level of one internal sub-server: **0** = free PvP, **1** = PK allowed with penalty, **2** = duel only, **3** = fully non-PvP. |

Steps:

1. Click **+ Add server** (or edit existing rows).
2. Fill in the columns. Use **remove** to delete a server (*The change only takes effect after you save.*).
3. Click **Save**, then publish.

{% hint style="warning" %}
The client list only controls what players **see**. The real server list is in the ConnectServer configuration — keep both consistent and use **Reload ServerList** on the ConnectServer (see [Services and reload](../services-and-reload.md#connectserver-modules)).
{% endhint %}

## Game texts (Text.bmd)

1. Select **Text.bmd**. The editor loads every available language (English, Portuguese, Spanish…).
2. Filter with **filter by key or text…**.
3. Edit the **Text** column of any **Key**; add entries with **+ entry**.
4. **Find and replace**: type in **search…** and **replace with… (empty = remove)**, then **replace** (filtered rows) or **apply to all** (`12 replacement(s) applied to all entries`).
5. Click **save N languages**.

## Items (Item.bmd)

A full item editor with category filters (**Weapons**, **Helms**, **Armors**, **Pants**, **Gloves**, **Boots**, **Shields**, **Pendants**, **Rings**, **Wings**, **Pets**, **Jewels**, **Other**) and **Search by name, type or index…**.

Item fields are organised in tabs: **Identity**, **Combat**, **Requirements**, **Classes**, **Resistances**, **Flags**, **Economy**, **Advanced**. Examples:

| Field | Description |
|---|---|
| **Item name** | Name shown in game. |
| **Size (X × Y)** | Inventory slots. |
| **Damage** / **Defense** / **Attack speed** | Combat values. |
| **Strength**, **Agility**, **Energy**, **Vitality**, **Command**, **Character level** | Requirements to equip. |
| Classes | Per class: blocked, **Base**, **1st**, **2nd**, **3rd** evolution. |
| **Drop**, **Trade**, **Store Personal**, **Warehouse**, **Sellable (NPC)**, **Repairable** | Behaviour flags. |
| **Base price (zen)**, **Stack (overlap)** | Economy. |

Useful tools:

- **Duplicate** an item, create a **+ New item**, or **Delete** (only applied on save; **Reload** before saving to undo).
- **group edits** – for armor sets, edit fields **concurrently** across all pieces of the set (Helm/Armor/Pants/Gloves/Boots). The name is never shared.
- Filters **any class** / **only &lt;class&gt;** and **complete sets only**.

{% hint style="warning" %}
Item changes must match the server item tables. Create a [backup](../backups-and-restore.md) before bulk changes and apply the server side with **Reload Item** or a restart.
{% endhint %}

## Move list (MoveReq)

Maps of the `/move` window: **No.** (unique), **Map**, **Sub-map**, **Min level**, **Max level** (*no limit* = any level), **Zen** and **Gate**. Each language has its own file. *The real /move is validated by the server (Move.txt/Gate.txt). Keep the same numbers and levels on both sides.*

## Tooltips (ItemTooltip and ItemTooltipText)

- **ItemTooltip** – per item: **Name**, **Name color**, **Bold**, **No tax**, **Special option**, **Show level**, **Level tooltip** and up to 12 **Lines** (each line = a phrase number from ItemTooltipText and a colour). A **Preview** shows the result. The file has exactly 8192 slots (`used of total slots in use`); adding uses a free slot.
- **ItemTooltipText** – phrases by number. Keep `%d` / `%s` in the same count and order as the original.

## Mix recipes (Mix)

Recipes of the mix window, by NPC type (**Goblin — normal**, **Goblin — chaos item**, **Castle (Senior)**, **Elpis**, socket NPCs…). Each recipe has names/descriptions (Text.bmd numbers), **Mix no.**, **Max rate (%)**, **Required level**, **Zen**, **Inventory space**, **Ingredient rule**, charms, **Ingredients** and a **Rate formula**.

{% hint style="warning" %}
The mix file only controls the client **screen** (recipes shown, items accepted and rate shown). The real result is decided by the GameServer — also change the server mix configuration.
{% endhint %}

## Visual and text modes

Several editors offer **Visual** and **Text** modes. Text mode shows the file as plain text: one line per record, texts in quotes, `//` for comments. A line with an error is marked and nothing is applied until it is fixed.

Rows can be moved (**Move up** / **Move down**), duplicated (**Duplicate**) or removed (**Remove**). If a language is missing a file, use **Create from the first language**.

## Other files (download / upload)

Files without a visual editor are listed with their **Role in the game**:

1. Click **Download**.
2. Edit the file in your own tool.
3. Click **Upload** to send it back. You'll see `uploaded (N B).` and the file joins the publish queue.

## Data files

Send `.bmd` (models), `.ozj` / `.ozt` / `.tga` (textures) and `.att` (terrain attributes) straight into the client's `Data` folder. They reach players on the next publication, on PC, Android and the web.

1. Open the folder that will receive the files (for example `object1`, `item`, `player`). Use **New folder** for a folder that doesn't exist yet: it's created on the first upload.
2. Drag the files onto the upload area, or click it to choose. Up to 25 MB per file. Names are converted to lowercase.
3. If a file already exists, you are asked to confirm the replacement.
4. Each file shows **New** or **Pending** until you [publish](publishing-updates.md). The pending list shows every Data file that goes out.

The panel checks the format before saving: a `.ozj` that is really a PNG, or a `.bmd` without the `BMD` signature, is refused with the reason.

**Undo** (the arrow on a pending file) brings back exactly the published file, or removes a new file. After publishing there is nothing to undo: send the previous file again instead.

{% hint style="warning" %}
**Replacing** an existing file takes effect as soon as you publish. A **new** file only shows up in game when something references it, for example a model registered in `CustomItem.txt` ([Client settings](client-settings.md)). Deleting a published file is not supported: players keep their copy.
{% endhint %}

The `local` and `ui` folders are managed by GetMainInfo and the UI Editor and are read-only here. Files with other extensions (`.map`, `.obj`, …) are not accepted.

## Release history

Every publication is listed with its version, date, author, status and file count. Use **reload** to refresh.

{% hint style="info" %}
The **Reindex from FTP** button is a recovery tool used after moving to a new machine or uploading updates manually. Use it only when support asks you to.
{% endhint %}
