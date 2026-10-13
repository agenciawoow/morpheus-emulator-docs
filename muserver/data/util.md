# Game Masters, notices, filters and messages

| File | What it controls |
|---|---|
| `Util/GameMaster.txt` | Which characters have GM / Admin powers |
| `Util/Notice.txt` | Automatic rotating notices |
| `Util/Filter.txt` | Words censored in chat |
| `Util/FilterRename.txt` | Words not allowed in new character names (rename command) |
| `LangConfig.txt` + `Lang/` | Server messages in each language |

Reset and experience tables (also in `Util/`) are on [Reset and experience](reset-and-experience.md).

---

## GameMaster.txt

### What it does

Gives GM powers to specific **characters**. The `GameMaster` column of [Command.txt](commands.md) and other GM-only features check this list.

### Where it lives

`MuServer/Data/Util/GameMaster.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Account` | `"text"` | Account login (quoted). |
| `Name` | `"text"` | Character name (quoted). |
| `Level` | 1 / 2 | `1` = Game Master, `2` = Administrator. A level-2 entry can use everything a level-1 entry can. |

Both the account **and** the character name must match (case-sensitive). Other characters on the same account are normal players.

### Example

```
//Account         Name         Level
"<account>"       "<gm-char>"  2
"<account2>"      "<helper>"   1
end
```

### Rules & pitfalls

- Treat this file as sensitive. Anyone listed with level 2 can `/reload`, create items, ban accounts and move players.
- After renaming a GM character, update the name here.

---

## Notice.txt

### What it does

A list of messages the server broadcasts to everyone, one after another, forever.

### Where it lives

`MuServer/Data/Util/Notice.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Message` | `"text"` | Text (quoted). |
| `Type` | 0 / 1 | `0` = golden notice at the top centre of the screen; `1` = blue system message in the chat. |
| `Count` | number | Repeat count for clients with animated notices. Keep `0`. |
| `Opacity` | number | Opacity for clients with animated notices. Keep `0`. |
| `Delay` | number | Display delay for clients with animated notices. Keep `0`. |
| `Red` `Green` `Blue` | 0–255 | Text colour for clients that support coloured notices. |
| `Speed` | number | Scroll speed for clients with animated notices. Keep `0`. |
| `RepeatTime` | seconds | Time until the **next** notice is sent. |

The server sends the first notice, waits its `RepeatTime`, sends the second, and so on; after the last one it starts over.

### Example (real line)

```
// Message                                          Type  Count  Opacity  Delay   Red  Green  Blue  Speed  RepeatTime
"Eventos durante o dia inteiro! Participe!"         0     0      0        0       255  255    255   0      180
end
```

A golden notice every 180 seconds (3 minutes).

### Rules & pitfalls

- Keep messages short; long lines are cut by the client.
- `RepeatTime 0` sends the next notice immediately, flooding the chat.

---

## Filter.txt

### What it does

Censors words in normal chat, whispers and `/post`: each listed word is replaced by `*`.

### Syntax

One quoted word per line (max 15 characters), up to 500 words, then `end`.

| Column | Values | Meaning |
|---|---|---|
| `Label` | `"text"` | Word to censor. Case-sensitive, matched anywhere inside the message. |

### Example

```
//Label
"badword"
"BADWORD"
end
```

Shipped empty.

---

## FilterRename.txt

### What it does

Blocks names (or parts of names) in the **character rename** command (`/mudarnome`). If the new name contains any listed text, the rename is refused.

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Label` | `"text"` | Text not allowed in the new name (max 10 characters). Case-sensitive. |

### Example (real lines)

```
//Label
"[gm]"
"[adm]"
"[staff]"
end
```

### Rules & pitfalls

- It does not affect character **creation**; creation is checked by the DataServer's `BadSyntax.txt`.
- Because it is case-sensitive, add common variants (`"[GM]"`, `"[gm]"`).

---

## LangConfig.txt and Lang/ (server messages)

`LangConfig.txt` lists the languages (`index`, Windows code page, bytes per UTF-8 character, name), and `Lang/NNN - Xxx_Message.txt` holds the server texts for each language (`index "text"`). Each player receives messages in their language; the default is `DefaultLanguageSystem` in `GameServerInfo - Common.dat`.

```
//index   CodePage    NumByteForOneCharUTF8
0         1252        2          "ENG"
1         1252        2          "POR"
```

Full guide: [Languages](../gameserver/languages.md). Reload with `/reload lang`.

{% hint style="info" %}
`Lang.bak-*` folders are backups made by the panel or by hand. The server only reads `Lang/`.
{% endhint %}

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md).

## Reload module

| Files | Reload |
|---|---|
| `GameMaster.txt`, `Notice.txt`, `Filter.txt`, `FilterRename.txt` | `/reload util` |
| `LangConfig.txt`, `Lang/` | `/reload lang` |
