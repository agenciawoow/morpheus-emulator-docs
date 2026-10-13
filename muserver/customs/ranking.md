# Ranking (F8 window)

The in-game ranking window: tabs such as General, Resets, Level, Blood Castle, Devil Square, with the top players read live from the database.

## What it is

The player opens the ranking window (F8 or the Ranking button of the custom menu). Each tab is one ranking; selecting it shows the list of characters with their class, VIP status and score. Clicking a character shows their equipment. The data comes straight from the database, so it is always current.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomRankingSwitch` | `1` | `1` = the server answers ranking requests. `0` = the window stays empty. |
| Client settings (Client Hub) | `EnableRankingButton` | `1` | Shows the Ranking button in the custom menu. |
| same | `EnableWindowsRankTop` | `1` | Enables the ranking window. |

## Files & syntax

File: `Data/Custom/CustomRanking.txt`. Section `0`, closed by `end`. Up to 10 rankings (tabs).

| Column | Meaning | Values |
|---|---|---|
| RankIndex | Tab number; also the `@type` passed to the database procedure | `0..9`, continuous |
| Name | Tab title, in quotes | `"Reset"` |
| Coluna 1 | Header of the name column, in quotes | `"Character:"` |
| Score | Header of the score column, in quotes | `"Resets"` |

This file only defines **labels**. The rows of each tab come from the SQL stored procedure `WZ_CustomRanking`, called with the tab number (`EXEC WZ_CustomRanking <RankIndex>`). For each tab the procedure must return the columns `VALUE1` (character name), `VALUE2` (score), `VALUE3` (class code) and `VALUE4` (VIP level).

## Example

```
0
//RankIndex  Name            Column1        Score
  0          "General"       "Character:"   "Master Resets"
  1          "Reset"         "Character:"   "Resets"
  2          "Level"         "Character:"   "Level"
  3          "Blood Castle"  "Character:"   "Points"
  4          "Devil Square"  "Character:"   "Points"
  5          "Power"         "Character:"   "Power"
end
```

The shipped column headers are in Portuguese (`"Personagem:"`, `"Pontos"`); translate them to your players' language.

## Commands related

None. The window is opened from the client (F8 / Ranking button).

## Pitfalls

- Adding a row here does not create a ranking: `WZ_CustomRanking` must also return data for that index. A tab without a matching branch in the procedure shows an empty list.
- RankIndex must start at `0` with no gaps; the client asks for tabs `0` to count-1.
- More than 10 rows are ignored.
- Long ranking lists are cut to what fits in one packet (about 4 KB); keep the procedure to a top 50 or so.

## Reload module

`/reload custom` (admin) re-reads `CustomRanking.txt` and `CustomRankingSwitch`. Changes to the stored procedure apply immediately (they are on the database). Client keys must be [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).
