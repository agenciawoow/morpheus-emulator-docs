# Death Message

Taunts and kill announcements: monsters "speak" when they kill a player, and the server can announce who killed whom.

## What it is

Three related features:

| Feature | What the player sees |
|---|---|
| Custom death message | When a monster kills a player, a chat bubble appears over the monster with the text you set for that monster, for example `I'm Golden Monster You Are Dead`. Only the killed player sees it. |
| Player kill announcement | When a player kills another player, every connected player gets a message: `<killer> killed <victim>`. |
| Monster kill announcement | When a monster kills a player, every connected player gets a message: `<monster> killed <player>`. |

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomDeathMessageSwitch` | `1` | `1` = monsters say their line from `CustomDeathMessage.txt` when they kill a player. |
| `GameServer/Data/GameServerInfo - Common.dat` | `DieUserSwitch` | `1` | `1` = announce player-vs-player kills to the whole server. |
| `GameServerInfo - Common.dat` | `DieMonsterSwitch` | `0` | `1` = announce players killed by monsters to the whole server. |

The kill announcements use `Data/Lang` texts, translated per player language: index **515** (player kill) and **516** (monster kill), both `%s killed %s` in English (killer, then victim).

## Files & syntax

File: `Data/Custom/CustomDeathMessage.txt`. One row per monster, then `end`. No sections.

| Column | Meaning | Values |
|---|---|---|
| `index` | Monster class | Monster index from `Data/Monster/Monster.txt` |
| `text` | What the monster says over its head | Text in quotes, up to 127 characters |

A monster with no row says nothing.

## Example

```
//index text
18  "Hihihih I'm Very Strong Monster"        // Gorgon
43  "I'm Golden Monster You Are Dead"        // Golden Budge Dragon
275 "Kneel before Kundun!"                    // Kundun
end
```

The shipped file has about 580 rows with a few generic taunts. Translate them or write your own; this text is not translated per player.

## Commands related

| Command | Who | What |
|---|---|---|
| `/reload custom` | Admin | Re-reads `CustomDeathMessage.txt` and `CustomDeathMessageSwitch`. |
| `/reload common` | Admin | Re-reads `DieUserSwitch` and `DieMonsterSwitch`. |
| `/reload lang` | Admin | Re-reads the Lang texts 515 / 516. |

## Pitfalls

* `DieMonsterSwitch = 1` on a busy server floods the chat: every death to a monster is sent to everyone. Leave it at `0` unless the server is small.
* Kills inside the Morpheus Tournament (Colosseum) are never announced, whatever the switches say.
* The death message text is a chat bubble, limited by the chat size. Keep it short.
* Keep the text under 128 characters: a longer value can stop the GameServer while the file loads.

## Reload module

* `/reload custom` for `CustomDeathMessage.txt` and `CustomDeathMessageSwitch`.
* `/reload common` for `DieUserSwitch` / `DieMonsterSwitch`.
* `/reload lang` for the announcement texts.
