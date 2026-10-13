# Rank User

Military-style ranks ("patents") earned by resets, master resets or level, shown over the character's head, with an optional coin prize each time a player climbs a rank.

## What it is

Every character falls into one rank band of your table (for example "Recruit" for 0-50 resets, "Soldier" for 51-100...). The rank title is shown over the character (and in the character info panel) together with resets, master resets and VIP status, depending on the client settings. When a reset, master reset or level-up moves the character **up** to a new rank, the server can:

- announce it to everyone ("Name achieved rank Soldier!"),
- tell the player ("You achieved rank Soldier!"),
- give the coins configured for the new rank.

Prizes are paid only on the transition to a higher rank. Skipping several ranks at once pays only the rank reached.

The rank is also used as a requirement by [Custom Quest](custom-quest.md) and [NPC Quest](npc-customs.md#npc-quest) (`RankUser` column).

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomRankUserSwitch` | `1` | `1` = rank info is sent to clients. `0` = no rank display. |
| same | `CustomRankUserType` | `0` | What the bands measure: `0` = resets, `1` = master resets, `2` = level + master level. |
| same | `CustomRankUserRewardSwitch` | `1` | `1` = notices and coin prize on rank-up. `0` = no rank-up rewards or notices. |
| same | `CustomRankUserNoticeToAll` | `1` | `1` = announce rank-ups to the whole server. |
| same | `CustomRankUserNoticeToUser` | `1` | `1` = private notice to the player. |
| `Tools/MainInfo/MainInfo.ini` (client) | `RankUserSwitch` | `4` | Display mode: `0` show all, `1` hide stats, `2` hide logo, `3` hide stats and logo, `4` logo on top, `5` disabled. |
| same | `RankUserShowOverHead`, `RankUserOnlyOnSafeZone`, `RankUserShowReset`, `RankUserShowMasterReset`, `RankUserNeedAltKey` | `1 1 1 1 0` | Client display options. |

## Files & syntax

File: `Data/Custom/CustomRankUser.txt`. Section `0`, closed by `end`. Up to 50 ranks.

| Column | Meaning | Values |
|---|---|---|
| RankIndex | Rank number | `0, 1, 2...` continuous, lowest rank first |
| Name | Rank title, in quotes | |
| ResetMin | Lowest value of the band | value measured by `CustomRankUserType` |
| ResetMax | Highest value of the band | `-1` = no upper limit (use on the last rank) |
| Cash | WCoinC prize for reaching this rank | |
| Gold | WCoinP prize | |
| PcPoint | GoblinPoint prize | |

Despite the column names, `ResetMin`/`ResetMax` measure whatever `CustomRankUserType` selects (resets, master resets or level + master level).

## Example

```
0
//RankIndex  Name          ResetMin  ResetMax  WC  WP  GP
  0          "Recruit"     0         50        0   0   100
  1          "Soldier"     51        100       0   0   100
  2          "Warrior"     101       150       0   0   100
  3          "General"     151       -1        0   0   500   // last rank: no upper limit
end
```

The shipped file has 11 ranks (0-50, 51-100 ... 500+) with 100 GoblinPoint each.

## Rewards

The WC / WP / GP of the **new** rank, given when the character climbs into it (needs `CustomRankUserRewardSwitch = 1`).

## Commands related

None. Ranks update automatically on reset, master reset and level-up.

## Pitfalls

- RankIndex must start at `0` with no gaps, in ascending order of bands. The server stops at the number of rows loaded, so a gap hides the higher ranks.
- Bands must not overlap; the first matching row wins.
- The last rank must have `ResetMax = -1`, otherwise characters above it have no rank (rank `-1`), and `RankUser` quest requirements fail for them.
- With `CustomRankUserType = 2` the value is **level + master level**, so set bands accordingly (for example 400-800). Since a reset lowers the level, the character drops ranks and climbs them again after every reset, and **the prize is paid again on each climb**. Use Type 2 with zero prizes, or use Type 0/1.
- Changing `CustomRankUserType` on a live server re-maps everyone's rank, but prizes are paid only on the next climb.

## Reload module

`/reload custom` (admin) re-reads `CustomRankUser.txt` and the `CustomRankUser*` keys. Client display keys in `MainInfo.ini` need a client rebuild.
