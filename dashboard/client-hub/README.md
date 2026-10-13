# Client Hub

The **Client Hub** is where you change what is inside your players' game client and deliver those changes to them. You edit the client files in the browser, review them in a queue, and publish one update that the players' launcher downloads automatically the next time they open the game.

For what each client file controls, see the MuServer reference: [Client files](../../muserver/client/README.md).

## Sidebar entries

| Entry | Page | Use it to |
|---|---|---|
| **Client Files** | [Client files](client-files.md) | Edit the server list, game texts, items, `/move` list, tooltips, mix recipes, and upload other client files. |
| **GetMainInfo** | [GetMainInfo](getmaininfo.md) | Edit the client configuration (connection, interface, custom menus, mobile, Discord) and build it. |
| **UI Editor** | [UI Editor](ui-editor.md) | Customise and preview the client screens (launcher, login, character select, shop…) on PC and mobile. |
| **Publish updates** | [Publishing updates](publishing-updates.md) | Review the queue of edited files and publish them to players. |

{% hint style="info" %}
{% endhint %}

## How a client change reaches players

```
Edit a file  →  Save  →  added to the pending queue  →  Publish  →  players' launcher downloads it
```

1. **Edit and save** in Client Files, GetMainInfo, the UI Editor or a visual editor (Cash Shop, Morpheus Shop). Saving writes the file on your server and marks it as **pending**.
2. **Review** the queue in **Publish updates**. The upload icon in the top bar shows how many files are waiting (`3 edited files awaiting publish`).
3. **Publish**. The panel packages the changed files and uploads them to your update hosting.
4. **Players update automatically.** The next time a player opens the launcher, it downloads only what changed. If a player's connection drops, the download resumes from where it stopped.

{% hint style="warning" %}
Saving is **not** publishing. Until you publish, players keep the previous version. Many edits can be collected in the queue and published together in one update.
{% endhint %}

## Server-side vs client-side changes

Many features have two halves. Make sure you update both:

| Change | Server side | Client side |
|---|---|---|
| Server list at login | ConnectServer `ServerList.dat` + **Reload ServerList** | `ServerList.bmd` via Client Files + publish |
| Item attributes | Server item tables + **Reload Item** / restart | `Item.bmd` via Client Files + publish |
| `/move` list | `Move.txt` / `Gate.txt` + **Reload Move** | MoveReq via Client Files + publish |
| Mix recipes | Server mix configuration | Mix recipes via Client Files + publish |
| Cash Shop | Saved together by the [Cash Shop editor](../editors/cash-shop.md) + **Reload cashshop** | **Publish to client** in the same editor |
| Morpheus Shop item list | [Morpheus Shop editor](../editors/morpheus-shop.md) | **Encode + Publish to client** |
| Feature switches (Morpheus Shop, Battle Pass…) | `GameServerInfo - Custom.dat` | GetMainInfo + publish |

## Tips

- Group related changes and publish once, instead of publishing after every small edit.
- Publish outside peak hours: players who are already in game get the update on their next launcher start.
- Every save and publish is recorded in [Logs and audit](../logs-and-audit.md).
