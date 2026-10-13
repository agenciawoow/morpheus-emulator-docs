# Languages

## What it does

Every message the GameServer sends to players (event announcements, command answers, errors, notices) comes from a numbered text in a language file. Morpheus keeps one file per language and sends each player the text in **the language chosen in their client**. Players whose client does not tell the server a language receive the server's default language.

## Where it lives

| File | Purpose |
|---|---|
| `Data/LangConfig.txt` | The list of languages: index, code page and name |
| `Data/Lang/NNN - Xxx_Message.txt` | The texts of each language (`NNN` = language index) |
| `GameServerInfo - Common.dat` `DefaultLanguageSystem` | The default language (see [Common](common.md#lang-system-settings)) |

Shipped files:

| Index | Language | File | Encoding |
|---|---|---|---|
| 0 | English | `000 - Eng_Message.txt` | Windows-1252 |
| 1 | Portuguese | `001 - Por_Message.txt` | UTF-8 |
| 2 | Spanish | `002 - Spn_Message.txt` | Windows-1252 |
| 3 | Chinese (Simplified) | `003 - Chs_Message.txt` | see LangConfig |
| 4 | Korean | not shipped | |
| 5 | Vietnamese | `005 - Vie_Message.txt` | UTF-8 |
| 6 | Thai | `006 - Tai_Message.txt` | see LangConfig |

## LangConfig.txt

### Syntax

One row per language, closed with `end`:

```
//index   CodePage    NumByteForOneCharUTF8    Name
0         1252        2                        "ENG"
1         1252        2                        "POR"
2         1252        2                        "SPN"
3         936         2                        "CHS"
4         949         2                        "KOR"
5         1252        2                        "VIE"
6         874         2                        "TAI"
end
```

| Column | Values | Description |
|---|---|---|
| `index` | 0 to 6 | Language number. Same number as `DefaultLanguageSystem` and as the `NNN` of the file. |
| `CodePage` | Windows code page | How to read the language file when it is **not** UTF-8: `1252` Western, `936` Simplified Chinese, `949` Korean, `874` Thai, `1258` Vietnamese, and so on. |
| `NumByteForOneCharUTF8` | number | Bytes per character used when converting texts. Keep the shipped value. |
| `Name` | quoted, 3 letters | Builds the file name: `"ENG"` → `000 - Eng_Message.txt`. |

If `LangConfig.txt` is missing, every language uses code page 1252 and the default names.

## Language files

### Syntax

One text per line: the text number, then the text in quotes.

```
0 "Blood Castle will close in %d minute(s)"
1 "Blood Castle has closed"
2 "Blood Castle %d will start in %d minute(s)"
32 "Free Account"
33 "Bronze Account"
34 "Silver Account"
35 "Gold Account"
37 ""
```

| Part | Rule |
|---|---|
| Number | The text index the server asks for. Do not renumber texts. |
| Text | In double quotes. `%d` is replaced by a number and `%s` by a word (name, item...), in order. |
| Empty text `""` | Means "not translated": the server uses the default language instead. |

### File name

`NNN - Xxx_Message.txt`, where `NNN` is the index and `Xxx` comes from the `Name` column. `NNN - Message.txt` is also accepted.

### Encoding

The server detects UTF-8 files automatically. Any other file is read with the code page of its language in `LangConfig.txt`. Both work; just keep each file in the encoding it already has.

## How a text is chosen

1. The player's language, if the text exists and is not empty there.
2. Otherwise the default language (`DefaultLanguageSystem`).
3. Otherwise Portuguese (`001 - Por_Message.txt`).

### Portuguese is the reference

`001 - Por_Message.txt` is the master file:

- It **must** exist. Without it (and without the old single-language `Data/Message.txt` from earlier versions) the GameServer does not start.
- Each translation is checked against it: if a text has a different set of `%d`/`%s` than the Portuguese text with the same number, that translation is **discarded** (logged) and the player gets the fallback. This prevents crashes from mismatched placeholders.

## Example

Translate text 7 into Spanish. In `002 - Spn_Message.txt`:

```
7 "No tienes nivel suficiente para entrar en Blood Castle"
```

Then `/reload lang`.

To make Spanish the default for players without a language, in `GameServerInfo - Common.dat`:

```ini
DefaultLanguageSystem = 2
```

## Adding a language

1. Pick a free index (for example 4 for Korean).
2. Check its row in `LangConfig.txt` (code page and name).
3. Copy `000 - Eng_Message.txt` to `004 - Kor_Message.txt` and translate the texts, keeping numbers and `%d`/`%s` unchanged.
4. `/reload lang`.

The client must also offer that language to the player. See [Client](../client/README.md).

## Rules and pitfalls

- **Keep the placeholders.** Same number and order of `%d` and `%s` as the Portuguese text, or the translation is dropped.
- **Close every quote.** An unclosed quote swallows the rest of the file. If a reload loads less than half of the previous Portuguese texts, the server rejects it and keeps the old texts (logged in red).
- **A broken file does not take the server down on reload.** The previous texts of that language stay in use and the error is logged. At startup, a missing or broken Portuguese file stops the GameServer.
- **Texts used by customs** (Battle Pass 950 and up, Chaos Mix announce 946/947, and others) live in these files too. When a custom page says "text N", it means this numbering.
- **Some keys of `GameServerInfo - *.dat` are text, not language indexes** (for example `CustomArenaText1`); those are sent as written. Others (`message_success` in Custom.dat) are language indexes. Each page says which.
- If a message appears as an empty box or blank line, the text is empty in every fallback language.

## Reload

`/reload lang` (or `/reload common`) re-reads `LangConfig.txt`, all files in `Data/Lang/` and `DefaultLanguageSystem`.
