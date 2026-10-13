# Backups and restore

The dashboard protects your files in two ways:

1. **Automatic safety copy on every save** – each time you save a file, the previous version is kept next to it. Copies older than 7 days are cleaned up automatically.
2. **Folder backups** – a complete ZIP of a service folder (for example the whole `GameServer` folder) that you create on demand and can restore entirely or file by file.

The visual editors (Cash Shop, Morpheus Shop, Battle Pass, EventItemBag) also keep a copy of every file they rewrite in the backup history.

## Creating a backup

1. Open **Files** in the sidebar.
2. Navigate into the service you want to back up (for example `GameServer`). The service is the first folder level.
3. Click **create backup** at the top of the tree.
4. Confirm **Create a backup of '&lt;service&gt;'?** with **Create backup**. It can take a few seconds depending on the folder size.

{% hint style="success" %}
Create a backup **before** any big change: a new season, a mass price change, replacing many files, or before letting a new staff member edit files.
{% endhint %}

## Viewing backups

Click **view backups** at the top of the tree. The **Backups** window lists every backup of that service:

| Column | Meaning |
|---|---|
| **ID** | Backup identifier (contains the date and time). |
| **When** | Creation date and time. |
| **By** | Who created it (a user, or "safety" backups created automatically before a restore). |
| **Files** | Number of files in the backup. |
| **Size** | ZIP size. |
| **Actions** | **restore everything** and **compare**. |

Every backup is a complete ZIP with a checksum for each file. Restoring replaces the current files — a safety backup is generated automatically first.

## Restoring everything

1. In **Backups**, click **restore everything** on the backup you want.
2. Read **Restore backup '&lt;id&gt;'?** — every file in the backup will be restored, and current files that exist in the backup are overwritten.
3. Click **Restore**.

{% hint style="info" %}
Before restoring, the panel **always** creates a safety backup of the current state. If the restore was a mistake, simply restore that safety backup.
{% endhint %}

{% hint style="warning" %}
Files that exist today but were **not** in the backup are left untouched (they are not deleted).
{% endhint %}

## Comparing with a backup and restoring selected files

Use this when you only want to undo some changes.

1. In **Backups**, click **compare** on a backup. The **Compare with backup** window opens (**comparing…**).
2. Files are grouped:

| Group | Meaning | Restorable? |
|---|---|---|
| **Modified — different from the backup** | The file changed since the backup. | Yes – restoring overwrites the current content. |
| **Removed — they were in the backup and were deleted afterwards** | The file no longer exists. | Yes – restoring re-creates the file. |
| **Added — they exist today and were not in the backup** | New files. | No – informational only. |
| **identical** | Same as in the backup. | Not needed. |

3. Tick the files to restore. By default everything that differs comes pre-selected. Use **select the differences** or **clear selection** to change it quickly. The counter shows `3 of 12 selected`.
4. Click **restore N files** and confirm. A safety backup of the current state is created first.

### Restoring a single file

1. Open **compare** on the backup.
2. Click **clear selection**.
3. Tick only the file you want.
4. Click **restore 1 file**.

## After restoring

Restored files are on disk, but the running programs still use what they loaded before. Apply the restore with:

- the right [hot reload module](services-and-reload.md#hot-reload), or
- a **Restart** of the service.

If the restored files are client files, publish them again through the [Client Hub](client-hub/publishing-updates.md).

## Good practices

- Create a backup before and after major updates, and label the moment in your own notes (the ID contains the date and time).
- Use **compare** regularly to see what changed since the last known-good state.
- Backups are per service: back up `GameServer` and `GameServerCS` separately if you change both.
- Every backup, comparison and restore is recorded in [Logs and audit](logs-and-audit.md).
