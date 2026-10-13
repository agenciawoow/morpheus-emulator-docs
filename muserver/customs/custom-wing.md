# Custom Wing

Add new wings and capes with their own defense, damage increase/absorption and options, and optionally make them possible results of the 3rd-wing Chaos Goblin mix.

## What it is

Custom wings behave like original wings: they add defense, increase the damage the wearer deals, reduce the damage they take, can have the additional option (Jewel of Life), excellent-style options and luck, and accept the jewels that original wings accept. The player sees the new model on the character and the stats in the item tooltip. If you list them in `CustomWingMix.txt`, they can also come out of the 3rd-level wing mix.

## Enable it

There is no on/off key for custom wings: a row in `CustomWing.txt` (server and client) creates the wing. The wing mix has these related keys:

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Common.dat` | `DisableWingMixDL` | `0` | `1` = Dark Lord wings/capes are removed from the wing mixes (Cape of Lord from the 2nd-wing mix, Cape of Emperor from the 3rd-wing mix). |
| same | `DisableWingMixSU` | `0` | `1` = Summoner wings are removed from the 1st, 2nd and 3rd-wing mixes (Talisman of Wing for Summoner gives Satan wings instead). |
| same | `DisableWingMixRF` | `0` | `1` = Rage Fighter capes are removed from the 2nd and 3rd-wing mixes. |

## Files & syntax

### `Data/Custom/CustomWing.txt` (server) and `MainInfo/Local/CustomWing.txt` (client)

Both files use the same columns and **must contain the same rows**: the server uses the stats, the client uses the stats for the tooltip and the model columns to draw the wing. A single list (no section number), closed by `end` or end of file.

| Column | Meaning | Values |
|---|---|---|
| # | Item group | `12` for wings |
| Index | Item index in the group | e.g. `130` |
| DefA | Defense added per item level: `Defense += DefA x level` | e.g. `4` |
| IncDmgA | Damage dealt, in %, at +0 | e.g. `139` = 139% |
| IncDmgB | Extra damage % per item level: total = `IncDmgA + level x IncDmgB` | e.g. `2` |
| DecDmgA | Damage taken, in %, at +0 | e.g. `61` = takes 61% (absorbs 39%) |
| DecDmgB | Less damage taken per item level: total = `DecDmgA - level x DecDmgB` | e.g. `2` |
| Opt1, Val1 | Additional option (Jewel of Life) and its value per step | option code, value |
| Opt2, Val2 | Alternative additional option, used when the item has the 5th excellent bit (16) | |
| Opt3, Val3 | Alternative additional option, used when the item has the 6th excellent bit (32) | |
| NewOpt1, NewVal1 | Option given by excellent bit 1 | option code, value |
| NewOpt2, NewVal2 | Option given by excellent bit 2 | |
| NewOpt3, NewVal3 | Option given by excellent bit 4 | |
| NewOpt4, NewVal4 | Option given by excellent bit 8 | |
| ModelType | Client only: `0` = attached as wings, other = attached like a cape (Dark Lord / Rage Fighter style) | `0`/`1` |
| ModelName | Client only: model file name, in quotes (textures in `Data/Item/Texture/`) | `"angel_wing"` |

The server reads `ModelType` and `ModelName` but ignores them.

Option codes listed in the file header:

| Code | Option |
|---|---|
| 80 | Additional damage +%d |
| 81 | Additional wizardry damage +%d |
| 82 | Additional defense rate +%d |
| 83 | Additional defense +%d |
| 84 | Luck (Jewel of Soul success +25%) |
| 85 | Automatic HP recovery %d% |
| 102 | Ignore opponent's defense %d% |
| 109 | Return %d% of the enemy's attack |
| 110 | Complete life recovery %d% rate |
| 111 | Complete mana recovery %d% rate |
| 113 | Additional curse spell +%d |

### `Data/Custom/CustomWingMix.txt` (server)

Extra results for the 3rd-wing mix (Chaos Goblin). Section `0`, closed by `end`. Maximum 50 rows.

| Column | Meaning |
|---|---|
| Category | Item group of the extra result (usually `12`) |
| Index | Item index |

Each row is added to the random result list of a successful 3rd-wing mix, with the same weight as the original 3rd wings.

## Example

`CustomWing.txt` (identical on server and client):

```
//#  Index DefA IncDmgA IncDmgB DecDmgA DecDmgB Opt1 Val1 Opt2 Val2 Opt3 Val3 NewOpt1 NewVal1 NewOpt2 NewVal2 NewOpt3 NewVal3 NewOpt4 NewVal4 ModelType ModelName
12   133   4    139     2       61      2       80   4    85   1    83   4    110     5       111     5       109     5       102     5       0         "angel_wing"
12   134   4    139     2       61      2       80   4    85   1    83   4    110     5       111     5       109     5       102     5       0         "devil_wing"
end
```

At +15 the wing above gives `+60` defense, deals `139 + 30 = 169%` damage and takes `61 - 30 = 31%` of incoming damage.

`CustomWingMix.txt`:

```
0
//Category Index
  12       133      // Angel wing can come out of the 3rd-wing mix
  12       134
end
```

## Commands related

None.

## Pitfalls

- Server and client `CustomWing.txt` must list the same wings with the same stats. A wing missing on the server behaves like a plain item (no wing stats); a wing missing on the client shows no model/tooltip. In the shipped package the client file has one extra row (`12 161`) and a different model on `12 160`; align them before going live.
- `DecDmgA - level x DecDmgB` must stay positive at +15, and `IncDmgA + level x IncDmgB` should stay above 100, or the wing becomes a penalty.
- The item must also exist in `Data/Item/Item.txt` (server) and in the client item list; `CustomWing.txt` only adds wing behavior.
- `Disable*` keys only affect the mixes; existing wings keep working.
- The client file is part of the client package: changes require repacking and redistributing the client. See [client custom files](../client/custom-files.md).

## Reload module

- `CustomWing.txt` and `CustomWingMix.txt` (server): `/reload custom`. Items already equipped get the new values the next time they are loaded (relog or re-equip).
- `DisableWingMix*`: `/reload common`.
- Client `MainInfo/Local/CustomWing.txt`: no hot reload; rebuild and redistribute the client.
