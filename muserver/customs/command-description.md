# Command Description

Information commands: you invent a chat command, and typing it shows a fixed text to the player.

## What it is

The player types a command such as `/rules` or `/discord` in chat and receives the text you wrote for it as a notice on screen. Use it for server rules, website and Discord addresses, event hours, or how-to tips.

## Enable it

There is no on/off key. Every row in `CustomCommandDescription.txt` is a working command. The shipped file has only a commented example, so no command is active.

## Files & syntax

File: `Data/Custom/CustomCommandDescription.txt`. One row per command, then `end`. No sections.

| Column | Meaning | Values |
|---|---|---|
| `ID` | Row number | Any unique number |
| `Command` | The command the player types, including the `/` | Text in quotes, up to 31 characters. Not case sensitive |
| `String` | Text shown to the player | Text in quotes, up to 127 characters. `%s` is replaced by the player's name |

## Example

```
//       #ID     #Command	  #String
	0	"/rules"	"%s, no bots, no insults, no scams. Read more on our website."
	1	"/discord"	"Join our Discord: <your-discord-invite>"
	2	"/events"	"Blood Castle every 2 hours. Custom Arena at 22:10."
end
```

Typing `/rules` as player Hero shows: `Hero, no bots, no insults, no scams. Read more on our website.`

## Client side

Nothing is required on the client for the command to work. To also list it in the client's command window, add a row to `Tools/MainInfo/Local/CustomCommand.txt` (see [Client custom files](../client/custom-files.md)) and redistribute the client.

## Commands related

| Command | Who | What |
|---|---|---|
| Any command you add here | Player | Shows its text. |
| `/reload custom` | Admin | Re-reads the file. |

## Pitfalls

* The command must not exist in `Data/Command.txt` or `Data/Custom/CustomMove.txt`. Those are checked first, so a duplicate name never shows your text.
* Only the first word is compared: `/rules anything` also shows the `/rules` text.
* Respect the limits: 31 characters for the command and 127 for the text. A longer value is not cut, it can stop the GameServer while the file loads. Split long texts into several commands.
* `%s` is the only placeholder. Do not use `%d` or a second `%s`: there is no value for them and the text shows garbage.

## Reload module

`/reload custom` re-reads `Data/Custom/CustomCommandDescription.txt`. New commands work immediately, no client change is needed.
