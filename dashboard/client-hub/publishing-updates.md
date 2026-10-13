# Publishing updates

**Every file here was edited in the panel but has not been published yet. Publish to make the changes reach the players.** Open **Client Hub → Publish updates**, or click the upload icon in the top bar (it shows `N edited files awaiting publish`).

## The pending queue

The page lists every pending file of the server:

| Column | Meaning |
|---|---|
| **File** | The client file (for example `serverlist`, `text-eng`, `morph_pak`, cash shop scripts…). |
| **Edited at** | Last save time. |
| **By** | Who saved it. |
| **Saves** | How many times it was saved since the last publication. |
| **Actions** | **Cancel update** (removes it from the queue). |

The summary reads `3 pending file(s)`. If nothing is pending: **Nothing pending** — every edited file has already been published.

## Publishing step by step

1. Review the list. Make sure every file is one you intend to ship.
2. Click **Publish N files** (title **Publish updates**).
3. The confirmation lists the files that will be uploaded — *Players receive the update the next time they open the launcher.* You can untick files you don't want in this update (**unchecked**).
4. Click **Deploy**.
5. Wait for the **Deploy result**. It shows:

| Figure | Meaning |
|---|---|
| Prepared files (first figure) | Files packaged for this update. |
| **Reused** | Unchanged files reused from the previous version. |
| **Uploaded** | Files sent to your update hosting. |
| **Failed** | Files that could not be uploaded. |
| **Time** | How long it took. |

6. When it says **finished ✓** — *Players will pick the update up on their next launcher run.*

The new version appears in the **Release history** on [Client files](client-files.md).

{% hint style="warning" %}
**Deploy failed.** If some files fail to upload, the message says how many; **the pending ones stay in the queue**. Check your update hosting (FTP/cPanel credentials, disk space) and try again. Files **skipped (no source)** were not found on your server — re-save them in their editor.
{% endhint %}

### Rebake full.zip

The confirmation has a **Rebake full.zip** option. It rebuilds the full client package from scratch and uploads a large file (around 1.6 GB). **Leave it unchecked** for normal updates. Use it only on the first publication after a migration, or when support asks you to.

## Cancelling pending updates

- **Cancel update** on one row → **Cancel the "&lt;file&gt;" update?**
- **Cancel all** → **Cancel ALL updates for &lt;server&gt;?**

{% hint style="danger" %}
Cancelling **does not undo the edit**. The file on your server stays as it is; it only leaves the publication queue. To revert the content, edit it again or restore it from a [backup](../backups-and-restore.md). The next publication of that file will include whatever is on the server at that time.
{% endhint %}

## How players receive the update

- The launcher checks for updates every time it starts.
- It downloads **only what changed** since the player's version.
- If the download is interrupted, it **resumes** where it stopped.
- Players already in game keep the old files until they restart through the launcher.

## Tips

- Collect several edits and publish them together.
- Too many pending changes can block new edits (**Too many changes waiting to be published. Publish the pending ones before editing more files.**). Publish regularly.
- Publishing from a visual editor (**Publish to client** in Cash Shop or **Encode + Publish to client** in Morpheus Shop) publishes those specific files immediately, without going through this page.
