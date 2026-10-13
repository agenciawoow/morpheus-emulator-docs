# Dashboard FAQ

## Access and accounts

### I forgot my password. How do I get back in?

Ask your administrator or reseller to set a new password on your account (**Clients → edit → New password**). Then sign in and change it in **Edit profile**.

### I lost my phone with the authenticator app.

Ask your administrator or reseller to **reset 2FA** on your account. You can then sign in with just the password and enable 2FA again in [Account and security](account-and-security.md#two-factor-authentication).

### Some menu items are missing for me.

Menus depend on your role. For example, `operator` accounts only see the Dashboard and Morpheus Sender, and `client` accounts don't see **API**. See [Roles and permissions](roles-and-permissions.md).

### Can I give my staff access without sharing my password?

Yes. Ask your reseller or administrator to create a separate account for each staff member, with the `operator` role for people who only start/stop services, or `client` for people who edit configuration.

### How do I change the panel language?

Click the language button in the top bar (e.g. `EN`) and pick a language. The choice is saved to your account. See [Signing in](signing-in.md#changing-the-language).

## Server control

### What is the difference between Restart and Reload?

**Restart** closes the program and opens it again — players on that GameServer are disconnected. **Reload** makes the running program re-read some files without closing; nobody is disconnected. See [Services and reload](services-and-reload.md).

### I changed a file but nothing changed in game.

Saving only writes the file. Apply it with the right **Reload** module (for example **Reload Shop** after an NPC shop change) or a restart. If the file is a client file, you must also [publish](client-hub/publishing-updates.md) it.

### Which reload module do I need?

Use the table in [GameServer modules](services-and-reload.md#gameserver-modules). If unsure, **Reload All** covers most modules except **Item** and **Terrain**.

### Restart says "waiting for the port to be released…" for a long time.

That's normal: the operating system can keep the port busy for up to about 60 seconds after a program closes. The panel waits and then starts the service automatically.

### How do I shut down for maintenance without upsetting players?

Use **Stop all** with **Warn players and close in N minutes**, or the **1 min** / **3 min** / **5 min** shutdown buttons in **GameServer actions**. Players get a countdown and their characters are saved.

### Can I start an event right now?

Yes. In **Morpheus GameServer Events** on the dashboard, click **Start** on the event. It starts in about 5 seconds. See [Server dashboard](server-dashboard.md#starting-an-event-now).

### The dashboard says the server is offline.

The panel can't reach your server machine. Check that the machine is on and has internet access. The panel reconnects automatically when it's back; until then live data and file editing are unavailable.

## Editing files

### The editor won't let me save: "errors block saving".

The GameServer would not read the file correctly. Fix the lines marked in red (open the problems list with **Ctrl+Shift+M**) and save again. See [Validation and conflicts](validation-and-conflicts.md).

### What does "Save anyway" mean?

There are only warnings: the file will load, but something may not work as you expect. Review the listed lines; if they are intentional, click **Save anyway**.

### "The file changed on the server since you opened it."

Someone (or something) changed the file after you opened it. Copy your change, **reload** the file, re-apply your change and save. In the visual editors you can choose **Reload from server** or **Overwrite anyway**.

### Why can't I edit some files?

They are read-only: protected services (DataServer and JoinServer by default), binary files, or file types that are not editable text. You can still open, search and compare them.

### I broke a file. How do I undo it?

Open **view backups** in the [File editor](file-editor.md), click **compare** on a recent backup, tick only that file and **restore 1 file**. A safety backup of the current state is created first, so you can't lose anything. See [Backups and restore](backups-and-restore.md).

### Will the panel break accents in my files?

No. MuServer files are read and saved in the exact encoding the server expects, and pasted typographic characters (curly quotes, non-breaking spaces) are fixed automatically.

## Visual editors and client updates

### I changed the Cash Shop, but players still see the old prices.

After saving, you need both steps in the editor bar: **Reload** (server side) and **Publish to client** (players' shop scripts). Players get the client part the next time they open the launcher.

### My new EventItemBag box doesn't drop anything.

Check the **Trigger** tab: without a line in `EventItemBagManager.txt` the GameServer doesn't load the bag. Also check that no other bag with a lower number has the same trigger (**shadowed trigger**). Then use the **Chances** tab to confirm the bag gives items to the player's class, and **Reload EventItemBag**.

### Why doesn't the Morpheus Shop or Battle Pass button appear in game?

Both features need their switch **on in the server and in the client**. The status chips in the editor show **on** / **OFF** for each side. Turn the client switch on in [GetMainInfo](client-hub/getmaininfo.md) and publish.

### What's the difference between "Save" and "Publish"?

**Save** writes the file on your server. **Publish** sends client files to players' launchers. Server-only changes never need publishing; client changes always do.

### I cancelled a pending update. Is my edit undone?

No. Cancelling only removes the file from the publish queue; the edited file stays on your server. To revert it, edit it again or restore it from a backup.

### How long until players get an update?

Immediately after publishing, for anyone who opens the launcher. Players already in game get it the next time they restart through the launcher. Only changed files are downloaded.

## Other

### Who changed this file?

Open [Logs and audit](logs-and-audit.md) and type the file name in the top bar search box. Each entry shows who did it and when.

### Can the AI assistant change my files?

No. It only reads files and explains them. You apply any change yourself, with validation and backups. See [AI assistant](ai-assistant.md).

### How do I receive alerts on WhatsApp?

Your account must be **Premium** and have a phone number in your profile. Then choose the notification types in **Morpheus Sender**. See [Account and security](account-and-security.md#notification-preferences-morpheus-sender).

### How do I change my server's logo in the panel?

Click the round avatar at the top of the sidebar and upload a PNG, JPEG, WebP or SVG up to 256 KiB.
