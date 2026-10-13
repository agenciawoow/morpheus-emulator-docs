# File editor

The **Files** page (**File editor**) lets you browse the server's folders and edit configuration files directly from the browser. Every save is validated, backed up, audited and protected against two people overwriting each other.

{% hint style="info" %}
{% endhint %}

## Page layout

- **Left: Folders** – the folder tree of your MuServer, one top-level folder per service (GameServer, ConnectServer, DataServer…).
- **Right: the editor** – the open file, its toolbar and a status bar.

## Browsing the tree

1. Open **Files** in the sidebar. At the root you see **root · pick a service**.
2. Click a service folder, then sub-folders, to navigate. The breadcrumb at the top (**root** / GameServer / Data / …) lets you jump back up.
3. Click a file to open it (**opening file…**).
4. Click **reload** at the top of the tree to refresh the listing.

Markers in the tree:

| Marker | Meaning |
|---|---|
| **RO** | Read-only: the file can be opened but not saved. |
| Struck-through name | **Binary/executable file — it cannot be opened by the editor** (for example `.exe`, `.dll`, archives, images). |
| Greyed name with tooltip **read-only (extension not in the whitelist)** | The file type is not editable text; it opens read-only. |

When a whole service is read-only you see **Editing disabled by the administrator for this service.** By default DataServer and JoinServer are read-only to protect the database layer.

{% hint style="info" %}
Editable text types include `.txt`, `.ini`, `.cfg`, `.dat`, `.xml`, `.json`, `.lua`, `.csv`, `.yml`/`.yaml` and `.conf`. Files outside this list open with **Extension outside the whitelist — read only.**
{% endhint %}

The top of the tree also has **create backup** and **view backups** for the current service — see [Backups and restore](backups-and-restore.md).

## Editing a file

When a file is open, the header shows its size, **modified &lt;date&gt;**, the encoding (for example `Windows-1252 · detected`) and live flags: **read only**, **modified** (unsaved changes) and **validating…**.

Header buttons:

| Button | Action |
|---|---|
| **Save** (shows `Save *` when there are changes) | Saves the file. Shortcut **Ctrl+S**. |
| **Compare** | **Unsaved changes** or **With another file…** – see [Comparing](#comparing). |
| **Full screen** | Expands the editor. **Exit full screen (Esc)** to return. |
| **reload** | Re-reads the file from the server (asks before discarding your draft). |
| **close** | Closes the file. |

After a successful save you see **Saved.** The previous version is kept automatically as a safety copy.

{% hint style="success" %}
**Encoding is handled for you.** MuServer files are saved in the exact format the MU server expects (Windows-1252 and Windows line breaks for `.txt` / `.dat` / `.ini` / `.cfg`), so accented characters are never corrupted. Language files follow the encoding configured for that language.
{% endhint %}

### Pasting text

When you paste text from a website, Word or a chat, invisible or typographic characters (curly quotes, non-breaking spaces…) can break MuServer files. The editor fixes them automatically and tells you: **N invisible or typographic characters were fixed in the pasted text (curly quotes, non-breaking space…).**

## Status bar and tools

The bar under the editor shows **line**, **col**, total lines and characters, and the current selection (`120 sel. (3 ln.)`). On the right:

| Tool | Shortcut | Description |
|---|---|---|
| Errors / warnings counter | **Ctrl+Shift+M** | Click to open the problems list. See [Validation and conflicts](validation-and-conflicts.md). |
| **Find and replace** | **Ctrl+F** / **Ctrl+H** | Search panel with **next**, **previous**, **all**, **match case**, **regexp**, **by word**, **replace** and **replace all**. |
| **Go to line** | **Ctrl+G** | Jump to a line number. Handy when an error message mentions a line. |
| **Wrap long lines** | – | Toggle line wrapping. |
| **Show spaces and tabs** | – | Shows whitespace. Useful because many MuServer files separate columns with TAB. |
| **Tab width (4 or 8)** | – | Changes how wide a tab is displayed (`Tab 4` / `Tab 8`). |
| **Smaller font** / **Larger font** | – | Adjusts the font size (remembered in your browser). |

Other editor features:

- **Line numbers** and **folding** arrows in the gutter (fold long sections; **unfold** to expand).
- **Undo / redo** with Ctrl+Z / Ctrl+Y.
- **Multiple cursors** (Alt+click) and **rectangular selection** (Alt+drag) to edit columns.
- **Tab** inserts a real tab character; **Shift+Tab** removes indentation.
- Matching words are highlighted when you select a word.

### Find and replace step by step

1. Press **Ctrl+F** (find) or **Ctrl+H** (find and replace).
2. Type the text. Turn on **match case** for exact upper/lower case, **by word** to match whole words only, or **regexp** for regular expressions.
3. Use **next** / **previous** to move between matches, or **all** to select every match.
4. Type the replacement and click **replace** (one) or **replace all**.
5. Press **Esc** or **close** to hide the panel.

{% hint style="warning" %}
**Replace all** on a large table can touch hundreds of lines. Check the warnings counter afterwards, and use **Compare → Unsaved changes** to review before saving.
{% endhint %}

## Comparing

The **Compare** menu opens a side-by-side view:

- **Unsaved changes** – **Comparing the file on disk with your draft.** The left side is **On disk (last saved version)**, the right side is your **Draft**. The arrows in the middle revert a single block.
- **With another file…** – pick any other file in the server (**Choose the file to compare**), for example `GameServer/Data/...` vs `GameServerCS/Data/...` to spot differences between two servers.

The header shows `N changed blocks` and **Previous difference** / **Next difference** buttons. Click **Back to editor** when done.

## Unsaved changes protection

The panel warns you (**Discard changes?**) before you lose a draft by:

- opening another file (**Opening another file discards the changes in &lt;file&gt;.**),
- reloading from disk (**Reloading from disk will overwrite the current draft.**),
- closing the file, or closing/reloading the browser tab.

## Read-only files

A file can be read-only because:

1. The service is protected (**Editing disabled by the administrator for this service.**).
2. The extension is not an editable text type.
3. The file is a binary key/data file (saving it as text would corrupt it).

You can still open, search and compare read-only files. If you really need to change a protected file, contact support.

## Tips

- Prefer the [Configuration forms](configuration-forms.md) and the [visual editors](editors/README.md) for common tasks: they explain each field and validate values.
- After saving, apply the change with the right [hot reload module](services-and-reload.md#gameserver-modules) or a restart.
- Create a backup before large edits — it takes a few seconds and lets you restore the whole folder.
