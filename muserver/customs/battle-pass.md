# Battle Pass

A seasonal reward track: players earn pass XP by playing and completing missions, level up the pass and claim prizes on a free row and a Premium row.

## What it is

The player opens the Battle Pass from its button in the game HUD (also listed in the menu grid). The window shows:

* **Track**: one prize per pass level on the free row and on the Premium row. Big "milestone" squares every 10 levels. Claiming a level also claims every unclaimed level below it on the same row.
* **Missions**: daily, weekly and season missions (kill monsters, stay online, finish Blood Castle / Devil Square / Chaos Castle, win PvP, use or collect items, do resets). Each gives pass XP and optionally a prize.
* **Login calendar**: a 28-day calendar with a prize for each day, the same for everyone.
* **Plans**: Free, Premium and Premium+. Buying a plan unlocks the Premium row (holders get **both** rows), gives instant levels, extra pass XP, extra weekly missions and, if you configure it, multiplied prizes.

Players also gain pass XP passively, for example by killing monsters, staying online, finishing events and resetting, up to daily limits.

## Enable it

The Battle Pass has **two switches, and both must be on**. With only one of them on, the button does not appear.

| File | Key | Default (shipped) | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomBattlePassSwitch` | `1` (code default `0`) | Server half of the switch. |
| Client settings (Client Hub), section `[Custom]` | `CustomBattlePassSwitch` | `1` | Client half of the switch, published to players from the Client Hub. |
| Client settings (Client Hub) | `EnableMenuCustomBattlePass` | `1` | Shows the Battle Pass in the HUD menu grid. |
| `GameServerInfo - Custom.dat` | `CustomBattlePassPaymentType` | `1` | How plans are paid. `1` = in-game coins. `2` = your website wallet (credits). |
| `GameServerInfo - Custom.dat` | `CustomBattlePassCoinID` | `1` | Coin charged when `PaymentType = 1`: `1` = WCoinC, `2` = WCoinP, `3` = GoblinPoint. |
| `GameServerInfo - Custom.dat` | `CustomBattlePassWalletID` | `0` | Which website wallet is charged when `PaymentType = 2`. Ignored with `PaymentType = 1`. |
| `GameServer/Data/GameServerInfo - Common.dat` | `WriteBattlePassLog` | `1` | `1` = write the Battle Pass log (draws, claims, purchases) in the GameServer logs. |

There is no Zen payment option on purpose: plan purchases need a coin trail that can be audited.

{% hint style="danger" %}
The Battle Pass files are **loaded even when `CustomBattlePassSwitch = 0`**, so problems show in the log. The consequence: a malformed `CustomBattlePass.txt` or season file (a missing or extra column, a missing `end`) **stops the GameServer** at startup or on `/reload custom`, with players online, even with the switch off. Validate every edit on a test server first, and never add a column to an existing section.
{% endhint %}

Texts (plan names, mission titles, season name) are not in these files. They are `Data/Lang` indexes, translated per player language:

| Range | Used for |
|---|---|
| 950..999 | Engine texts: plan names (950 = Premium, 951 = Premium+, 952 = Free), notices, refusals |
| 1000 + (N-1)*100 .. +99 | Season N: season name, mission titles and descriptions. Season 1 = 1000..1099 |

## Files & syntax

| File | What it holds |
|---|---|
| `Data/Custom/CustomBattlePass.txt` | The **engine**: global settings, the list of seasons, plans, passive XP, multipliers, icons |
| `Data/Custom/BattlePass/NNN - <name>.txt` | One **season**: XP curve, prize catalog, track, missions, calendar. Shipped: `001 - Ascensao de Kundun.txt` (Rise of Kundun) |

Only the season set as `ActiveSeason` is loaded.

### Engine section 0: global (one row)

| Column | Meaning | Values |
|---|---|---|
| `ActiveSeason` | Season running now (`Index` from section 1) | Number, `0` = none |
| `DailyCount` | Free daily missions drawn per account per day from the season pool | Number, `0` = all |
| `WeeklyCount` | Free weekly missions drawn per account per week | Number, `0` = all |
| `PremiumExtraWeek` | Extra Premium-only weekly missions for Premium holders (added to the plan's `ExtraWeek`) | Number |
| `XpDailyCap` | Maximum pass XP per day from all sources, missions included | Number, `0` = no cap |
| `ClaimGraceDays` | Days after the season ends during which prizes can still be claimed | Days |
| `StreakResetOnMiss` | `1` = missing a day resets the login calendar streak | 0 / 1 |
| `ResetHourUtc` | Hour (UTC) when the day turns over for missions, caps and calendar | 0..23 |

The draw is per account and per period: the same account sees the same missions on every character, relog or restart, until the day or week turns. Premium daily missions are not drawn: Premium holders get all of them. Season missions are always visible.

### Engine section 1: seasons

| Column | Meaning | Values |
|---|---|---|
| `Index` | Season number | Number |
| `MaxLevel` | Highest pass level | 1..200 |
| `Start` / `End` | Season dates, server time | `"YYYY-MM-DD HH:MM"` in quotes |
| `File` | Season file, relative to `Data/Custom/BattlePass/` | Text in quotes |

Seasons with invalid dates, an end before the start or a `MaxLevel` out of range are ignored and logged. Up to 64 seasons.

### Engine section 2: Premium plans

| Column | Meaning | Values |
|---|---|---|
| `PlanId` | `1` = Premium, `2` = Premium+ (Free is always 0 and is not listed) | 1 / 2 |
| `Price` | Price in the unit of the payment method (coins, or wallet credits) | Number |
| `GiveLevel` | Pass levels given at purchase | Number |
| `XpBonus` | Extra pass XP, applied before the daily cap | % |
| `ExtraWeek` | Extra weekly missions | Number |
| `MsgId` | Lang index of the plan name | 950..999 |

### Engine section 3: passive XP sources

| Column | Meaning | Values |
|---|---|---|
| `Source` | `1` = monster kill, `2` = minute online, `3` = event finished, `4` = reset, `5` = master reset | 1..5 |
| `Param` | Monster class (source 1), event id (source 3), otherwise `*` | Number, `*` = any |
| `Xp` | Pass XP per occurrence | Number |
| `CapDay` | Daily cap of this source | Number, `0` = only the global cap |
| `MinLvl` | Minimum character level for this source to count | Level |

### Engine section 4: prize multiplier per plan

| Column | Meaning | Values |
|---|---|---|
| `PlanId` | Plan | 1 / 2 |
| `Kind` | Prize kind (same codes as season section 2) | 0..6 |
| `Mult` | Multiplier in hundredths: `200` = double, `150` = 1.5x | Number |

Applies to the track and missions, not the calendar. For items (`Kind 0`) it multiplies the quantity, never level or options. Missing rows = 100.

### Engine section 5: icons of non-item prizes

| Column | Meaning | Values |
|---|---|---|
| `Kind` | Prize kind 1..6 (Zen, WCoinC, WCoinP, GoblinPoint, Experience, Buff) | 1..6 |
| `IconG` / `IconS` | Icon folder and file used for that kind | Numbers |

### Season section 0: header

| Column | Meaning | Values |
|---|---|---|
| `MsgId` | Lang index of the season name | `1000 + (N-1)*100` |

### Season section 1: XP curve

| Column | Meaning | Values |
|---|---|---|
| `LvlMin` / `LvlMax` | Level band | Levels |
| `XpToNext` | XP to go from **each** level of the band to the next | Number |

The sum of the curve is the cost of finishing the pass. Season 1: 850,000 XP; with `XpDailyCap = 20000` a player hitting the cap every day finishes in about 43 days.

### Season section 2: prize catalog

| Column | Meaning | Values |
|---|---|---|
| `PrizeId` | Prize number used by the track, missions and calendar | Number |
| `Kind` | `0` = item, `1` = Zen, `2` = WCoinC, `3` = WCoinP, `4` = GoblinPoint, `5` = character experience, `6` = buff | 0..6 |
| `Section` / `Type` | Item group and index (`Kind 0`). For `Kind 6`, `Section` = effect id from `Data/Effect.txt` | Numbers, `*` when unused |
| `Qty` | How many items, or the amount of Zen / coins / experience | Number |
| `Level` | Item level | 0..15 |
| `Dur` | Durability per unit, `0` = full. Must be `0` on stackable items (there the durability is the quantity) | Number |
| `Skill` / `Luck` / `Option` | Item options 1, 2, 3 | 0/1, 0/1, 0..7 |
| `ExcOp` | Excellent bitmask | 0..63 |
| `SetOp` / `JoH` / `OEx` | Ancient, Harmony, 380 options | Numbers |
| `Sk1`..`Sk5` | Socket option per slot | `255` = empty |
| `Time` | Time-limited item or buff duration, counted from the claim | **Seconds**, `0` = permanent |
| `Icon` | `*` = default icon, or `"items/G/S"` / `"battlepass/name"` to override | Text |

Stackable items with `Qty` above 1 become one stack (split at the stack limit); non-stackable items take one slot each.

### Season section 3: track

| Column | Meaning | Values |
|---|---|---|
| `Level` | Pass level | 1..MaxLevel |
| `Row` | `0` = free row, `1` = Premium row | 0 / 1 |
| `PrizeId` | Prize from section 2 | Number |
| `Milestone` | `1` = big square on screen | 0 / 1 |

### Season section 4: missions (pool)

| Column | Meaning | Values |
|---|---|---|
| `Id` | Mission number | Number |
| `Kind` | `0` = daily, `1` = weekly, `2` = season | 0..2 |
| `Cond` | `1` kill monster (P1 = monster class, P2 = map), `2` minutes online, `3` finish event (P1 = event id), `4` reach level, `5` win PvP (P2 = map), `6` use item (P1/P2 = item group/index), `7` collect item (P1/P2 = item group/index), `8` reset | 1..8 |
| `P1` / `P2` | Parameters of the condition | Number, `*` = any |
| `Target` | Amount needed | Number |
| `Xp` | Pass XP given on claim | Number |
| `Prem` | `1` = Premium holders only | 0 / 1 |
| `PrizeId` | Extra prize, `0` = XP only | Number |
| `Msg` / `MsgD` | Lang index of the title and of the description | Season range |

Event ids for `Cond 3` and passive source 3: `0` = Blood Castle, `1` = Devil Square, `2` = Chaos Castle.

### Season section 5: login calendar

| Column | Meaning | Values |
|---|---|---|
| `Day` | Calendar day | 1..28 |
| `PrizeId` | Prize from section 2 | Number |
| `Milestone` | `1` = big prize | 0 / 1 |

## Example

Engine (`CustomBattlePass.txt`), as shipped:

```
0
//ActiveSeason	DailyCount	WeeklyCount	PremiumExtraWeek	XpDailyCap	ClaimGraceDays	StreakResetOnMiss	ResetHourUtc
  1		6		5		4			20000		1		1			3
end

1
//Index  MaxLevel  Start              	End                	File
  1	 50	   "2026-10-16 00:00"	"2026-12-31 23:59"	"001 - Ascensao de Kundun.txt"
end

2
//PlanId	Price	GiveLevel	XpBonus	ExtraWeek	MsgId
  1		5000	3		10	3		950     // Premium: 5000 WCoinC
  2		10000	6		20	4		951     // Premium+
end

3
//Source	Param	Xp	CapDay	MinLvl
  1		*	1000	10000	10      // any monster kill, from level 10
  2		*	20	2000	0       // per minute online
end

4
//PlanId	Kind	Mult
  2		1	200     // Premium+: double Zen
end
```

Season file, a few rows of each section:

```
1
//LvlMin  LvlMax  XpToNext
1         10      10000
11        25      15000
end

2
//PrizeId Kind Section Type Qty    Level Dur Skill Luck Option ExcOp SetOp JoH OEx Sk1 Sk2 Sk3 Sk4 Sk5 Time Icon
  1       1    *       *    50000  0     0   0     0    0      0     0     0   0   255 255 255 255 255 0    *    // 50,000 Zen
  5       0    14      13   3      0     0   0     0    0      0     0     0   0   255 255 255 255 255 0    *    // Jewel of Bless x3
end

3
//Level Row PrizeId Milestone
1       0   1       0       // level 1 free: 50k Zen
1       1   56      0       // level 1 Premium
end

4
//Id Kind Cond P1  P2 Target Xp    Prem PrizeId Msg  MsgD
  1  0    1    *   0  300    2000  0    1       1010 1011   // daily: kill 300 monsters in Lorencia
  5  0    3    0   *  1      2000  0    10      1014 1015   // daily: finish Blood Castle once
end
```

## Client settings to match

* Client settings `[Custom]` `CustomBattlePassSwitch = 1` and `EnableMenuCustomBattlePass = 1`, in [Client Hub → Client settings](../../dashboard/client-hub/client-settings.md). Changes reach players only once [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).
* Custom prize art (`Icon = "battlepass/<name>"`) must be shipped to players with the client update. A missing file shows a generic icon.

## Commands related

There is no player command: the pass opens from its button. Admin: `/reload custom` (see below).

## Pitfalls

* **A malformed file kills the GameServer even with the switch off** (see the danger box). Never add a column to an existing section; new fields always go in a new section.
* Both switches must be on (`GameServerInfo - Custom.dat` and the client settings). The client logs which one failed.
* When the season `End` date passes, the pass stops progressing until you set `ActiveSeason` to the next season and reload. The log says which registered season covers today's date.
* A season that has not started yet opens in the window but gives nothing until its `Start`.
* Check the XP curve against `XpDailyCap` and the season length before going live: the sum of the curve decides whether players can finish.
* Start passive sources with tight `CapDay` values. Without caps, a bot can finish the season in a day.
* `Time` is in **seconds** here (not minutes, as in some other custom files).
* `Dur` other than `0` on a stackable item is refused on load and the row is skipped.
* Only Blood Castle, Devil Square and Chaos Castle count as "event finished".
* `PaymentType = 2` charges your website wallet; set the correct `CustomBattlePassWalletID` and test a purchase before announcing it.
* Mission titles live in `Data/Lang`. Add each new index to every language file.

## Reload module

* `/reload custom` re-reads `CustomBattlePass.txt`, the active season file and the `CustomBattlePass*` keys. Configuration is reset and re-read; player progress is kept. A broken file at this point stops the GameServer, so test first.
* `/reload lang` re-reads the texts (indexes 950..999 and the season range).
* `/reload common` re-reads `WriteBattlePassLog`.
* Client settings changes have no hot reload: they must be [published to players from the Client Hub](../../dashboard/client-hub/publishing-updates.md).
