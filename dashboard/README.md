# Dashboard overview

The **Morpheus Emulator Dashboard** is the web control panel for your MU Online server. From any browser you can start and stop the server programs, watch players and events in real time, edit every configuration file safely, use visual editors for shops, spots, boxes and the Battle Pass, and publish client updates to your players.

Everything you do in the panel goes through validation, automatic backups and an audit trail. You never have to open a remote desktop session to change a `.txt` file again.

{% hint style="info" %}
The panel is available in **Português (Brasil)**, **English**, **Español**, **Tiếng Việt** and **简体中文**. This documentation uses the English labels. See [Signing in](signing-in.md#changing-the-language) to switch languages.
{% endhint %}

## What you can do

| Area | What it is for | Page |
|---|---|---|
| Dashboard | Live CPU / memory / disk, players online, service control, GameServer actions, events and live console | [Server dashboard](server-dashboard.md) |
| Service control | Start, stop, restart and hot-reload the MuServer programs | [Services and reload](services-and-reload.md) |
| Files | Full text editor for the server folders, with validation and compare | [File editor](file-editor.md) |
| Configuration | Ready-made forms for the main `.dat` / `.ini` settings | [Configuration forms](configuration-forms.md) |
| Editors | Visual editors: NPC Shop, Spot, Cash Shop (X), Morpheus Shop, Battle Pass, EventItemBag | [Visual editors](editors/README.md) |
| Client Hub | Client files, GetMainInfo, UI Editor and publishing updates to players | [Client Hub](client-hub/README.md) |
| Logs & Audit | Who changed what, and when | [Logs and audit](logs-and-audit.md) |
| Server logs | Read the MuServer log files from the browser | [Server logs](server-logs.md) |
| AI assistant | Ask questions about your files and settings | [AI assistant](ai-assistant.md) |

## First screen: picking a server

After you sign in:

- If your account has **only one server**, the panel opens that server's dashboard directly.
- If your account has **several servers**, you see a **Welcome** page with one card per server. Each card shows the server name, its ID, an **Online** / **Offline** badge and a summary such as `5 services · 42 editable files`. Click a card to open it.

If you see **No server available — Your account has no server assigned. Talk to an admin.**, your account exists but no server has been linked to it yet. Contact support.

## Screen layout

Every server page has the same three parts: the **sidebar** on the left, the **top bar**, and the page content.

### Sidebar

At the top of the sidebar you find the Morpheus Emulator logo (click it to return to the server list) and a server card with:

- **Server logo** – click the round avatar to upload your own logo (PNG, JPEG, WebP or SVG, up to 256 KiB).
- **Server name and ID**.
- **Connection status** – green **connected** when the panel can talk to your machine, red **offline** when it can't. When offline, live data and file editing are unavailable.

Below the card is the navigation menu.

| Menu item | Sub-items |
|---|---|
| **Dashboard** | – |
| **Files** | – |
| **Configuration** | – |
| **Licenses** | – |
| **Client Hub** | **Client Files**, **GetMainInfo**, **UI Editor**, **Publish updates** |
| **Editors** | **NPC Shop editor**, **Spot editor**, **Cash Shop (X)**, **Morpheus Shop**, **Battle Pass**, **EventItemBag Editor** |
| **Logs & Audit** | – |
| **Server logs** | – |
| **Morpheus Sender** | – |

At the bottom, the **Protected access** box reminds you that every change goes through schema validation, backup and audit.

{% hint style="info" %}
The **Licenses** page simply lists the license(s) assigned to your account with their status and expiry date. To change anything on a license, contact support.
{% endhint %}

### Top bar

From left to right:

| Element | What it does |
|---|---|
| **Search box** (`Filter services, files, logs...`) | Filters the current page as you type: services on the dashboard, entries on Logs & Audit, and so on. Click **✕** to clear. |
| **Language** (e.g. `EN`) | Switches the panel language. Your choice is saved to your account. |
| **Pending updates** (upload icon) | Shows how many edited client files are waiting to be published (`3 edited files awaiting publish`). Click it to open the [Publish updates](client-hub/publishing-updates.md) queue. |
| **Notifications** (bell) | Opens your notifications. A badge shows unread items. Use **Mark all as read** to clear it. |
| **Account menu** (your e-mail) | **Edit profile**, **Morpheus Sender** and **Log out**. A Premium badge appears if your account is Premium. |

### Status bar

At the top of the dashboard a thin status bar shows **Server:** (with a selector to jump to another server you manage), **License:** (**Active** or **Offline**) and **Updated Ns ago**, so you always know how fresh the data on screen is.

### AI assistant button

If the AI assistant is enabled for your account, a floating **AI assistant** button appears in the bottom-right corner of server pages. See [AI assistant](ai-assistant.md).

## Notifications

The bell collects messages for your account, each tagged with a type:

| Type | Typical content |
|---|---|
| **Urgent** | Critical incidents (server offline, security problems) |
| **Status** | Operational alerts (GameServer went down, MuServer restarted) |
| **Update** | New versions and configuration changes that affect you |
| **News** | Panel feature releases and announcements |

Items marked **automatic** were generated by the system (for example, a GameServer that stopped). Choose which types you receive — and whether they also go to WhatsApp — in [Account and security](account-and-security.md#notification-preferences-morpheus-sender).

## Where to go next

1. [Sign in and enable two-factor authentication](signing-in.md).
2. Get to know the [Server dashboard](server-dashboard.md).
3. Learn the safe editing workflow: [File editor](file-editor.md) → [Validation and conflicts](validation-and-conflicts.md) → [Backups and restore](backups-and-restore.md).
4. Use the [visual editors](editors/README.md) for the most common day-to-day changes.
5. Publish changes to players with the [Client Hub](client-hub/README.md).
