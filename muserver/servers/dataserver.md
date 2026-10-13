# DataServer

## What it does

The DataServer is the only program that reads and writes game data in the SQL database: characters, inventories, warehouses, guilds, rankings, event results and so on. GameServers connect to it; players never do.

It also checks new character names (on creation and on rename) against a list of forbidden words.

## Where it lives

| File | Purpose |
|---|---|
| `DataServer/Data/DataServer.ini` | Database connection and listening port |
| `DataServer/Data/AllowableIpList.txt` | Which machines may connect (your GameServers) |
| `DataServer/Data/BadSyntax.txt` | Words not allowed in character names |

---

## DataServer.ini

INI format, section `[DataServerInfo]`.

| Key | Default | Values | Description |
|---|---|---|---|
| `DataServerPort` | 55960 | 1 to 65535 | TCP port GameServers connect to. Must equal `DataServerPort` in every `GameServerInfo - Common.dat`. |
| `AdvancedLog` | 0 | 0, 1 | `1` = write extra detail about every request to the DataServer log. Use only while troubleshooting; it makes the log grow fast. |
| `DataServerODBC` | (empty) | text | Name of the ODBC data source (DSN) that points to the database. Set up at installation. |

The file also holds the database login. It is created when the server is installed and you should not need to change it.

{% hint style="danger" %}
Never share `DataServer.ini` or post it in a support ticket without removing the database login first.
{% endhint %}

### Example

```ini
[DataServerInfo]
DataServerODBC = MuOnline
DataServerPort = 55960
AdvancedLog = 0
```

### Rules and pitfalls

- `DataServerODBC` is the **data source** name configured in Windows ODBC, not the database name.
- If the DataServer cannot open the database at startup, every GameServer will fail to load characters. Check the DataServer window first when players cannot enter.

### Reload

Restart the DataServer. Restart it only with all GameServers closed, or characters online at that moment may lose their last changes.

---

## AllowableIpList.txt

### What it does

Only machines listed here can connect to the DataServer. Connections from any other IP are refused immediately. This protects your database from outside access.

### Syntax

Section `0`, one quoted IP address per line, closed with `end`:

```
0
"127.0.0.1"
"192.0.2.10"
end
```

### Rules and pitfalls

- If every GameServer runs on the same machine as the DataServer, `"127.0.0.1"` is all you need (this is the shipped setting).
- Add the IP of every other machine that runs a GameServer. Use the address the DataServer sees, normally the private/LAN IP.
- Exact match only. No ranges, no wildcards.
- An empty list blocks every GameServer.

### Reload

Restart the DataServer.

---

## BadSyntax.txt

### What it does

A list of words that cannot appear anywhere in a character name. It is checked when a character is created, when a name is checked, and when a character is renamed. The server already rejects names with invalid characters; this list is for words you want to forbid (offensive words, "GM", "ADMIN", your server's staff tags, and so on).

### Syntax

One quoted word per line, closed with `end`:

```
"GM"
"ADMIN"
"Staff"
end
```

The shipped file has an empty list (only a comment and `end`).

### Rules and pitfalls

- The check is a **substring** match: `"GM"` blocks `GMaster`, `KingGM` and `xGMx`.
- The check is **case sensitive**: `"GM"` does not block `gm` or `Gm`. Add each variant you want to block.
- Names that already exist are not affected.

### Reload

Restart the DataServer.
