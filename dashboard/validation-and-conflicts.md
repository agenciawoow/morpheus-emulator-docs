# Validation and conflicts

The dashboard checks your files **the same way the GameServer reads them** — while you type and again when you save. It also stops two people from silently overwriting each other's work. This page explains the messages you can see and what to do.

## Errors vs warnings

| | **Error** (red) | **Warning** (amber) |
|---|---|---|
| Meaning | The GameServer would **not load** the file, would read it wrong, or could crash. | The file loads, but the result may not be what you expect. |
| Saving | **Blocked** until fixed. | Allowed after you confirm **Save anyway**. |
| Where | Red mark on the line, red counter in the status bar. | Amber mark on the line, amber counter. |

Validation runs about half a second after you stop typing (**validating…** in the header).

### Seeing the problems

- Each problem line has a marker in the gutter; hover it to read the message.
- Click the `2 errors` / `5 warnings` counter in the status bar (or press **Ctrl+Shift+M**) to open the **Diagnostics** list. Click an entry to jump to the line.
- Use **Go to line** (**Ctrl+G**) when a message mentions a line number.

### Saving with errors

When you click **Save** with errors, nothing is written and you see, for example:

> **1 error blocks saving: the GameServer would not load this file. Fix the line marked in red.**

Fix the marked lines and save again.

### Saving with warnings

When there are only warnings, the panel asks **Save with N warnings?** — *These won't crash the GameServer, but the configuration may not turn out as you expect. Review them before saving.* The first 20 warnings are listed with their line numbers (`line 42:`). Click **Save anyway** to continue or cancel to go back and fix them.

{% hint style="info" %}
Problems that were **already in the file** before your edit are marked **(already in the file)**. They are shown so you know about them, but they don't count against your change.
{% endhint %}

## Common messages and how to fix them

| Message (simplified) | What it means | Fix |
|---|---|---|
| **This line has 8 columns; the others in the section have 9. A value is missing or extra.** | A row has the wrong number of values; the GameServer would shift every following column. | Add or remove the missing value. Turn on **Show spaces and tabs** to see separators. |
| **Column 3 should be a number; the GameServer will read 0.** | Text where a number is expected. | Type a number. |
| **Number glued to "x": the GameServer swallows that character.** | E.g. `10x` instead of `10 x`. | Separate values with a space or TAB. |
| **Unclosed quote: the GameServer closes it at the next quote in the file and shifts every following column.** | A `"` without its pair. | Close the quote. |
| **Character "é" outside quotes: the GameServer reads it as 0 and shifts the columns.** | Text without quotes in a numeric table. | Put the text in quotes or remove it. |
| **Section 3 has no "end": the GameServer hangs while loading and exits.** | A table section was not closed. | Add a line with `end`. |
| **This line comes after "end" and will be ignored by the GameServer.** | Data after the end of the file/section. | Move it inside a section or delete it. |
| **A section number was expected here.** | Section header missing. | Add the section number line. |
| **Text is 40 bytes: the GameServer accepts at most 32 per field (memory overflow, crash).** | A name/text is too long. | Shorten it. Accented letters may count as 2 bytes. |
| **X = 300 is out of range (0 to 255): the GameServer would write outside its memory and crash.** | Index or value beyond the limit. | Use a value inside the range. |
| **The character "ÿ" (byte 0xFF) is read by the GameServer as END OF FILE.** | Everything after it is ignored. | Remove the character. |
| **The character "€" does not exist in this file's Windows-1252 encoding — it would be saved as "?".** | Unsupported character for this file. | Replace it with a supported character. |
| **The placeholders changed (before: %s %d; now: %d %s).** | In message files, the `%d`/`%s` order changed; the GameServer could crash. | Keep the same placeholders in the same order. |
| **The placeholders differ from Portuguese: the GameServer discards this translation and uses the Portuguese text.** | A translation doesn't match the reference text. | Use the same placeholders as the Portuguese line. |
| **Message is 270 bytes: the limit for this message is 255.** | Message too long. | Shorten it. |
| **The file had 120 data rows and now has 80. Check that nothing was deleted by accident.** (warning) | Many rows disappeared. | Confirm it was intentional; otherwise use **Compare → Unsaved changes**. |
| **The file had a BOM, which the GameServer does not understand. It will be removed on save.** (warning) | Informational; fixed automatically. | Nothing to do. |
| **The file encoding was not what the GameServer expects; it will be converted on save.** (warning) | Informational; fixed automatically. | Nothing to do. |
| **Binary file (key/data). Saving it as text would corrupt it.** | The file is not text. | Don't edit it in the text editor. |

### Configuration forms

In [Configuration forms](configuration-forms.md), invalid fields show short messages under the field: **required**, **must be a number**, **must be an integer**, **minimum 0**, **maximum 100**, **at most 10 characters**, **invalid format**.

### Visual editors

The visual editors show a **status panel** above the content, such as `3 warning(s)` or `1 error(s) and 2 warning(s)`. Click **view** to expand the list, **hide** to collapse it, and **go** to jump to the item with the problem. When everything is fine it shows a green message (for example **All good: the GameServer reads every bag exactly as shown here.**). Saving is blocked while there are errors.

## Conflicts: "the file changed on the server"

Every time you open a file, the panel remembers its exact version. When you save, it checks that nobody else changed the file in the meantime — another staff member in the panel, someone editing directly on the server machine, or an automatic process.

### In the File editor

If the file changed after you opened it, the save is refused with:

> **The file changed on the server since you opened it. Reload before saving.**

What to do:

1. Copy your changes somewhere safe (or open **Compare → Unsaved changes** to see them).
2. Click **reload** and confirm discarding the draft. You now see the current version.
3. Re-apply your changes and **Save**.

### In the visual editors

The editors show a yellow banner, for example **The files changed on the server after you opened the editor.** or **"&lt;file&gt;" was changed on the server after you opened the editor.**, with two choices:

| Choice | Effect | When to use |
|---|---|---|
| **Reload from server** / **Reload** / **Reload (lose my changes)** | Discards your unsaved edits and loads the current version. | When the other change matters, or you're not sure what changed. |
| **Overwrite anyway** | Saves your version over the other one. | Only when you are sure the other change can be lost (e.g. you made it yourself in another tab). |

### In Client settings

In **Client Hub → Client settings**, a banner tells you that the client or connection settings were changed on the server while you were editing. Use the **reload** option to see the current version (your edits are discarded) or **Overwrite with mine** to save your version over it.

### In the UI Editor

**The file changed on the server** — *&lt;file&gt; was changed on the server after you opened it.* Choose **Reload** (bring the server version; your edit to that file is discarded) or **Keep mine** (keep your edit here, unsaved, and decide later).

{% hint style="danger" %}
**Overwrite** really replaces the other version. If unsure, reload and redo your change — and remember that every save is backed up, so you can recover a lost version from [Backups and restore](backups-and-restore.md).
{% endhint %}

## Why this matters

A single shifted column in a MuServer table can make the GameServer load wrong prices, drop the wrong items, or close during startup. Validation catches these mistakes **before** they reach your players, and conflict protection makes sure teamwork never silently erases someone's work.
