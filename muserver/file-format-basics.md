# File format basics

Almost every MuServer file follows one of two formats. Learn the rules on this page once and you can read any of them.

| Format | Used by | Comment | Example |
|---|---|---|---|
| **INI** (`Key = Value`) | `GameServerInfo - *.dat`, every `*.ini`, `Data/Custom/CustomBalanceClass.dat` | `;` | `AddExperienceRate_AL0 = 200` |
| **Table script** (columns and sections) | Almost every `.txt` and most `.dat` in `Data/`, `ServerList.dat`, `MapServerInfo.dat` | `//` | `0  "Lorencia"  1  ...` |

## INI files

```ini
[GameServerInfo]

;==================================================
; Item Drop Settings
;==================================================
ItemDropTime = 30
ItemDropRate_AL0 = 10
```

| Rule | Detail |
|---|---|
| Section | Keys must be under the right `[Section]` header. In every `GameServerInfo - *.dat` file the only section is `[GameServerInfo]`. A key above it, or under another header, is ignored. |
| Comments | A line that starts with `;` is a comment. The `;====` banners only group keys for humans; the server does not care about them. |
| Spaces | Spaces around `=` are ignored. `Key=1`, `Key = 1` and `Key    =    1` are the same. |
| Case | Key names are not case sensitive. `EventTvtSwitch` and `EventTvTSwitch` are the same key. |
| Duplicates | If a key appears twice in the same section, only the **first** one counts. |
| Missing key | The server uses its built-in default. Each page in this guide lists those defaults. A missing key is often **0 = off**, not the value you expect. |
| Numbers | Whole numbers only (no `1.5`, no `1,000`). The server reads the leading digits and stops at the first other character. |
| Text values | Do **not** use quotes. Everything after `=` up to the end of the line is the value, including any `;` and text after it. |

{% hint style="warning" %}
Inline comments only work on **number** keys. `MaxLevelCharacter = 400 ; level cap` reads as `400`. But `CustomStoreJoBName = BLESS STORE ; shop title` makes the shop title `BLESS STORE ; shop title`. Put comments on their own line.
{% endhint %}

## Table script files

```
//==========================================================
//  Comment lines start with two slashes
//==========================================================
0
//Index  Name             Level  Rate
0        "Lorencia"       10     100
1        "Dungeon"        *      100     // a comment after the row
end

1
//Index  Value
0        5
end
```

### Sections and `end`

Most files are split into numbered **sections**. A section starts with its number alone on a line and ends with `end`. Inside it, each line is one row.

- Every section that is opened must be closed with `end`.
- The section number tells the server what kind of rows follow. Section `0` of one file has nothing to do with section `0` of another.
- Files with only one table usually still have `end` at the bottom, with or without a section number before the rows. Keep whatever the file already has.

### Columns

The server reads values **in order**, one after the other. Line breaks are just separators. This has two consequences:

- A row must have **exactly** the number of columns the file expects. One column too many or too few shifts every following value and silently breaks the rest of the file. In some files it stops the GameServer at startup.
- The `//Index  Name ...` line above the rows is a **column header** for humans. It is a comment: the server ignores it. Use it to count your columns.

### Comments

`//` starts a comment that runs to the end of the line. It can be on its own line or after a row. A single `/` is not a comment and will break the row.

{% hint style="info" %}
Leave at least one space before a comment that follows a number: write `2592000 // 30 days`, not `2592000//30 days`. Most files tolerate it, but not all.
{% endhint %}

### Quoted text

Text that may contain spaces goes between double quotes: `"Blood Castle"`, `"/move"`. Rules:

- Always close the quote. An open quote swallows the rest of the file as a single text.
- You cannot put a `"` inside a quoted text.
- Numbers are never quoted.

### `*` means "any" (-1)

In number columns, `*` is read as `-1`. Files use it to mean "any", "random", "no limit" or "not set", depending on the column. For example, in `MapManager.txt` a `*` in the `NonPK` column means "use the default from `GameServerInfo - Common.dat`", and in a spawn file a `*` direction means "random direction". You can write `-1` instead of `*`; they are the same.

## Item index: Section × 512 + Type

Items are identified by a **group** (also called section or category, 0 to 15) and a **type** (the item's number inside that group, 0 to 511). Most files write them as two columns, for example `14  13` for Jewel of Bless. Some files and some settings need a single **item index**:

```
ItemIndex = Group × 512 + Type
```

| Group | Contents | First index | Example |
|---|---|---|---|
| 0 | Swords | 0 | Kris = 0 × 512 + 0 = **0** |
| 1 | Axes | 512 | Small Axe = **512** |
| 2 | Maces and scepters | 1024 | Mace = **1024** |
| 3 | Spears | 1536 | Light Spear = **1536** |
| 4 | Bows and crossbows (and arrows, bolts) | 2048 | Short Bow = **2048** |
| 5 | Staffs, sticks and summoner books | 2560 | Skull Staff = **2560** |
| 6 | Shields | 3072 | Small Shield = **3072** |
| 7 | Helms | 3584 | Bronze Helm = **3584** |
| 8 | Armors | 4096 | Bronze Armor = **4096** |
| 9 | Pants | 4608 | Bronze Pants = **4608** |
| 10 | Gloves | 5120 | Bronze Gloves = **5120** |
| 11 | Boots | 5632 | Bronze Boots = **5632** |
| 12 | Wings, capes, orbs, Jewel of Chaos, seeds and spheres | 6144 | Jewel of Chaos (12, 15) = **6159** |
| 13 | Pets, rings, pendants, tickets and event items | 6656 | Guardian Angel = **6656** |
| 14 | Potions, jewels, boxes and consumables | 7168 | Jewel of Bless (14, 13) = **7181**, Jewel of Soul (14, 14) = **7182** |
| 15 | Skill scrolls and parchments | 7680 | Poison Scroll = **7680** |

The full list of groups and types is in `Data/Item/Item.txt`, one section per group. See [Items](data/items.md).

## Numbered files: `NNN - Name.txt`

Some folders hold one file per entry instead of one big file: `Data/MonsterSetBase/` (one file per map), `Data/EventItemBag/` (one file per bag), `Data/Shop/` (one file per shop), `Data/Lang/` (one file per language), `Data/Custom/BattlePass/` (one file per season), `ConnectServer/Data/Update/` (client patches).

The file name **must** start with exactly three digits, a space, a dash and a space:

```
000 - Lorencia.txt        OK       number 0
055 - Box of Kundun 5.txt OK       number 55
55 - Box of Kundun.txt    IGNORED  only two digits
055-Box of Kundun.txt     IGNORED  no " - " after the number
```

- The **number** is what links the file to the rest of the configuration (map number, bag number, shop number, language index). The name after ` - ` is free text for you.
- Two files with the same number: only one is loaded. Delete or rename the old one.

### Disabling a file with `_`

To turn a numbered file off without deleting it, put `_` at the start of its name: `_055 - Box of Kundun 5.txt`. The name no longer starts with three digits, so the server skips it. Remove the `_` to turn it back on.

{% hint style="warning" %}
A disabled bag, shop or spawn file behaves as if it did not exist: the bag drops nothing, the NPC opens an empty shop, the map has no monsters. That is usually what you want, but check what refers to it.
{% endhint %}

## Encodings

| Encoding | Files |
|---|---|
| **Windows-1252** (ANSI) | Almost everything: all `.ini` and `GameServerInfo - *.dat` files, `ServerList.dat`, `Command.txt`, `MapManager.txt`, `ItemDrop.txt`, `ItemValue.txt`, most of `Data/Custom/`, most of `Data/Event/`, `Data/Lang/000 - Eng_Message.txt` and the other non-UTF-8 language files |
| **UTF-8** | `Data/Custom/CustomMorpheusShop.txt`, `Data/Custom/CustomBattlePass.txt`, `Data/Custom/CustomCombo.txt`, `Data/Event/MorpheusTournament.dat`, `Data/Item/Item.txt`, `Data/Move/Move.txt`, `Data/Lang/001 - Por_Message.txt`, `Data/Lang/005 - Vie_Message.txt` |

Rules:

- **Keep the encoding the file already has.** Converting a Windows-1252 file to UTF-8 (or the reverse) turns accented letters into garbage in game.
- Language files in `Data/Lang/` are the exception: the server detects UTF-8 automatically and otherwise uses the code page set in `LangConfig.txt`. See [Languages](gameserver/languages.md).
- Never save a file with the byte `0xFF` (the character `ÿ` in Windows-1252). The script reader treats it as the end of the file and ignores everything after it.
- Line endings can be Windows (CRLF) or Unix (LF).

The control panel's [File editor](../dashboard/file-editor.md) handles the encoding for you.

## The standard banner

Most files start with the same header. In `.txt` and table `.dat` files it uses `//`; in INI files it uses `;`:

```
//========================================================================================
//  MORPHEUS EMULATOR - CONFIGURATION FILE
//----------------------------------------------------------------------------------------
//  File       : ServerList.dat
//  Version    : 1.0.0
//  Developer  : Hi Networks
//----------------------------------------------------------------------------------------
//  Description:
//  This file is part of the Morpheus Emulator configuration package.
//  Any incorrect modification may cause unexpected behavior or server issues.
//----------------------------------------------------------------------------------------
//  Copyright (c) Hi Networks. All rights reserved.
//========================================================================================
```

The banner is only a comment. The `File` line is not checked, so do not rely on it: for example the shipped `GameServerInfo - Common.dat` says `Character.dat` in its banner.

## Common mistakes

| Mistake | What happens | Fix |
|---|---|---|
| Missing `end` | The next section is read as rows of this one, or the file fails to load | Close every section with `end` |
| One column too many or too few | Every value after it shifts; drops, shops or spawns go wrong. Some files stop the GameServer at startup. | Count columns against the `//` header line |
| Unclosed quote | The rest of the file becomes one long text | Close every `"` |
| `Key = value ; comment` on a text key | The comment becomes part of the text | Put comments on their own line |
| Typo in a key name (`EventGvGMinGuild` instead of `EventGvGMinUsers`) | The key is ignored and the built-in default is used | Copy key names from this guide |
| Duplicate key | Only the first one is used | Search the file before adding a key |
| Wrong file name in a numbered folder | The file is ignored | Use `NNN - Name.txt` |
| Converting the encoding | Accents become `Ã©`, `�` or spaces | Keep the original encoding |
| Editing the wrong `Data` | Change has no effect | Game content is in root `MuServer/Data/`; settings are in `<Program>/Data/` |
| Forgetting to reload | Change has no effect until restart | Use the reload listed on each page |
