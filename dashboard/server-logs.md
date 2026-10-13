# Server logs

**Server logs** is a read-only browser over the MuServer log folders. Use it to read what the GameServer, ConnectServer, DataServer and the other programs wrote — today or on previous days — without connecting to the server machine. Content is read on demand directly from your server.

{% hint style="info" %}
{% endhint %}

## Reading a log

1. Open **Server logs** in the sidebar.
2. In **Folders** (left), pick the service, then navigate its log folders (`LOG` or `LOGS`, often organised by date). The breadcrumb (**root** / …) lets you go back up. Empty folders show **empty**.
3. Click a log file. Its text appears in **Content** (right). Before you pick a file you see **Select a file on the left to open it.**

## Which log to read

| Program | What you typically find |
|---|---|
| **GameServer** | Startup and reload results, errors loading files, player connections, item drops, GM commands, hack detections, chat logs (depending on your log settings). |
| **ConnectServer** | Client connections to the server list. |
| **JoinServer** | Account logins. |
| **DataServer** | Database operations and errors. |

## Troubleshooting with logs

| Situation | Where to look |
|---|---|
| A reload didn't apply | GameServer log of today: look for the module name (`Shop loaded successfully`) or a red error line naming the file. |
| GameServer closes at startup | GameServer log: the last lines usually name the file that failed to load. Then check it in the [File editor](file-editor.md). |
| Players can't log in | JoinServer and ConnectServer logs. |
| Suspected cheating | GameServer hack logs, or the **Hack Log** tab of the [live console](server-dashboard.md#morpheus-gameserver-console). |

{% hint style="success" %}
For **today's** GameServer output, the live console on the [Dashboard](server-dashboard.md#morpheus-gameserver-console) is faster: it has tabs for System, Global Message, Command, Hack and Connect logs, with search and auto-scroll.
{% endhint %}

## Notes

- Logs are read-only: you can't edit or delete them from the panel.
- Large log files can take a moment to open (**loading…**).
- Use your browser's find (**Ctrl+F**) to search inside the opened content.
