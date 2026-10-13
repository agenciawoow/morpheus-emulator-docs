# Logs and audit

**Logs & Audit** is the append-only history of the critical actions on your server: who started or stopped a service, who saved which file, who created or restored a backup, and more. Entries cannot be edited or deleted afterwards, so you always have a trustworthy record of what happened.

{% hint style="info" %}
Requires log access (roles `admin`, `reseller`, `client`). See [Roles and permissions](roles-and-permissions.md).
{% endhint %}

## Reading the audit list

Open **Logs & Audit** in the sidebar. The **Audit** card shows the most recent entries (up to the last 500), newest first, and a header like `128 entries` with a chip per category (`files · 54`, `service · 30`, `backup · 4`…).

Each entry shows:

| Part | Meaning |
|---|---|
| Category chip | The first part of the action, e.g. `files`, `service`, `editor`, `backup`, `token`, `auth`, `user`. |
| Action | What happened, e.g. `files.write`, `service.start`, `backup.restore`. |
| Actor | The account (e-mail) that did it. |
| **target** | What it was done to: a file path, a service name, a backup ID… |
| Time | When it happened. |
| Risk pill | **Low risk** for normal actions; **High risk** when the action was **rejected** (for example a save blocked by validation or a permission). |

### Common actions

| Action | Meaning |
|---|---|
| `files.write` | A file was saved (File editor or Configuration). |
| `editor.doc.write` | A visual editor saved its files. |
| `service.start` / `service.stop` / `service.restart` / `service.reload` | Service control from the dashboard. |
| `backup.create` / `backup.restore` / `backup.diff` | Backups created, restored or compared. |
| `server.logo.*` | Server logo changed or removed. |
| `token.create` / `token.revoke` | API tokens created or revoked. |
| `auth.login` | A sign-in to the panel. |
| `user.*` | Account changes (profile, 2FA reset, AI access). |
| `clienthub.pending.cancel` | A pending client update was cancelled. |

## Searching

Type in the **top bar search box** (`Filter services, files, logs...`). The list is filtered as you type — by action, actor, target or any text in the entry — and the header shows **filtering "&lt;text&gt;"**. If nothing matches: **No entry matches "&lt;text&gt;".**

Examples:

- `GameServer/Data/Custom` – every change in the custom folder.
- `restore` – every restore.
- `staff@` – everything done by a staff account.

## Tamper protection

The **Hash chain** card explains how the audit protects itself: every entry carries a fingerprint computed from the previous entry plus its own content. Changing or removing a single line breaks the chain, which makes tampering detectable.

## Using the audit day to day

- **After an incident**: search for the file or service involved to see who changed what, then compare or restore from [Backups and restore](backups-and-restore.md).
- **Staff review**: filter by a staff member's e-mail to review their changes.
- **Before a big update**: check that no unexpected changes happened since the last backup.

{% hint style="success" %}
Give each person their own account. Shared accounts make the audit useless, because every action appears under the same name.
{% endhint %}

## Related pages

- [Server logs](server-logs.md) – the log files written by the MuServer programs themselves (game events, connections, hacks).
- [Server dashboard](server-dashboard.md#morpheus-gameserver-console) – live GameServer console.
