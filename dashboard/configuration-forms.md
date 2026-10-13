# Configuration forms

The **Configuration** page turns the main MuServer `.ini`-style settings files into **forms**. Every field already knows its type, allowed range and validation, so you can change settings without worrying about syntax. A key that does not exist in the file yet is **created** when you save.

{% hint style="info" %}
You need file access (roles `admin`, `reseller`, `client`). Changes made here are saved to the same files you see in the [File editor](file-editor.md), with the same backup and audit protection.
{% endhint %}

## Page layout

The page shows the forms available for your server as a grid of cards. Each card has:

- The **display name** of the settings file (for example the GameServer common settings).
- The real file name underneath, so you know which file you are editing.
- A **high risk** badge for files where a wrong value can stop the server or disconnect players.

Below the cards you may see a list of **table files** with the note **Table schemas. Use the file editor for these for now.** Those files (shops, item lists, monster tables…) are edited in the [File editor](file-editor.md) or in a [visual editor](editors/README.md).

If you see **No INI schema enabled for this server.**, no form has been enabled for your server yet — contact support.

## Editing settings

1. Open **Configuration** in the sidebar.
2. Click the card of the file you want (**loading schema…**).
3. The form shows the file's sections as boxes titled `[SectionName]`. Each field shows the setting key (for example `ExperienceRate`).
4. Change values:

| Field type | Control |
|---|---|
| On/off switch | Drop-down with **0 — off** / **1 — on**. |
| List of options | Drop-down showing each value with its meaning (for example `2 — Hard`). |
| Number | Numeric input; the allowed range is enforced. |
| Text | Text input with a maximum length. |

5. The **Save** button shows how many fields you changed, for example `Save (3)`. Click it to save.
6. Apply the change on the running server with the matching [hot reload module](services-and-reload.md#hot-reload) (for example **Reload Common** for `GameServerInfo - Common.dat`) or a restart.
7. Click **close** to return to the list.

### Badges and messages on fields

| Indicator | Meaning |
|---|---|
| **new** | **Not in the file; saving appends the line.** The setting is supported but missing from your file. Fill it in and save to add it. |
| Red border + message | The value is invalid: **required**, **must be a number**, **must be an integer**, **minimum N**, **maximum N**, **at most N characters** or **invalid format**. Saving is blocked until you fix it. |
| Amber border + warning | The field is empty and leaving it empty is dangerous in production. Read the warning carefully. |
| Rules list at the bottom | Some files have rules that involve several fields (for example "minimum must be less than maximum"). They are listed at the end of the form. |

{% hint style="warning" %}
Fields left **empty** are not written unless they already exist in the file. To remove a setting from the file, use the [File editor](file-editor.md).
{% endhint %}

## Forms vs the file editor

| Use **Configuration** when… | Use the **File editor** when… |
|---|---|
| You want to change a few well-known settings safely. | You need to edit tables, comments or many values at once. |
| You want each field validated with its real range. | The file has no form yet. |
| You are not familiar with the file format. | You need search/replace, compare or go to line. |

Both save to the same file, so you can switch between them. If someone changes the file while your form is open, reload the page before saving.

## Tips

- Files marked **high risk** (connection settings, ports, keys) can stop players from connecting if set wrong. Create a [backup](backups-and-restore.md) first and change one thing at a time.
- After saving, look at the GameServer **System Log** on the [Dashboard](server-dashboard.md#morpheus-gameserver-console) to confirm the reload succeeded.
- Use the **reload** button at the bottom of the page to refresh the list after support enables new forms.
