# Custom Arena

Scheduled PvP arenas you design yourself: who may enter, how long it lasts, how many deaths eliminate a player, and what the winner gets.

## What it is

At the scheduled time the server announces that an arena is open. For a few minutes players can enter it through the move list (`/move <name>` or the **M** window). When the entry window closes the fight starts: every kill gives points, every death costs points. A player who reaches the death limit is sent out of the arena. When time runs out, or only one fighter is left, the server ranks everyone by score. The 1st place is announced to the whole server and can receive an item, and every participant sees their own position and score.

The shipped file has eight arenas: a free-for-all **Survivor** for every class and seven class-only duels (MG, DW, DK, RF, DL, Elf and Summoner).

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomArenaSwitch` | `0` | `1` = arenas follow their schedule. `0` = no arena runs, and kills score nothing. |
| `GameServerInfo - Custom.dat` | `CustomArenaVictimScoreDecrease` | `10` | Points lost by the player who dies. The score never goes below 0. |
| `GameServerInfo - Custom.dat` | `CustomArenaKillerScoreIncrease` | `100` | Points gained by the killer. |
| `GameServerInfo - Custom.dat` | `CustomArenaText1` .. `CustomArenaText20` | see below | Announcement texts. `%s` = arena name, `%d` = number. |
| `GameServer/Data/GameServerInfo - Character.dat` | `CustomArenaDamageRate` | `100` | Damage rate (%) applied to PvP inside the arena. |

The texts as shipped:

| Key | Text | When it is shown |
|---|---|---|
| `CustomArenaText1` | `[%s] will close in %d minute(s)` | Entry window countdown, to everyone |
| `CustomArenaText2` | `[%s] closed` | Entry window closed, to everyone |
| `CustomArenaText3` | `[%s] will start in %d minute(s)` | To the players inside, before the fight |
| `CustomArenaText4` | `[%s] started` | Fight started |
| `CustomArenaText5` | `[%s] finished` | Time is over |
| `CustomArenaText6` | `[%s] %s is the winner` | Winner, to everyone |
| `CustomArenaText7` | `[%s] You got in the %d position with %d points` | Each participant's result |
| `CustomArenaText8` | `You can not enter the arena` | Player is in a party, guild war, duel, personal shop or already in the arena map |
| `CustomArenaText9` | `This arena is not currently open` | Entry window is not open |
| `CustomArenaText10` | `Exceeded maximum number of players` | `MaxUser` reached |
| `CustomArenaText11`..`18` | `You must be at least Level %d` ... `You need to have a maximum of %d Master Resets` | Level, master level, reset and master reset limits |
| `CustomArenaText19` | `His class is not allowed in the arena` | Class or evolution not allowed |
| `CustomArenaText20` | `[%s] No inventory space: item reward not delivered` | Winner's inventory is full |

## Files & syntax

File: `Data/Custom/CustomArena.txt`. It has three sections. Up to 30 arenas (`Index` 0..29). Rows with an index outside that range are ignored and logged.

### Section 0: schedule

`Index Year Month Day DoW Hour Minute Second`, one row per start time. `Index` is the arena from section 1. The time columns follow the [schedule syntax](../events/schedule-syntax.md).

### Section 1: arena rules

| Column | Meaning | Values |
|---|---|---|
| `Index` | Arena number | 0..29 |
| `Name` | Name used in every announcement | Text in quotes |
| `AlarmTime` | Minutes the entry window stays open before the arena closes | Minutes |
| `StandTime` | Minutes between closing the entry and starting the fight | Minutes, `0` = start at once |
| `EventTime` | Fight duration | Minutes |
| `CloseTime` | Minutes the players stay inside after the result before being moved out | Minutes, `0` = move out at once |
| `StartGate` | Gate players enter through and respawn at while alive | Gate index from `Data/Move/Gate.txt`. Must be on map 40 (see Pitfalls) |
| `FinalGate` | Gate players are sent to when eliminated or when the arena ends | Gate index (shipped: `1`, Lorencia) |
| `MinUser` | Minimum players to start. With fewer, the arena is cancelled and goes back to waiting for its next time | Number, `*` = 1 |
| `MaxUser` | Maximum players | Number, capped at 100 |
| `MaxDeath` | Deaths that eliminate a player | Number, `*` = unlimited |
| `MinLevel` / `MaxLevel` | Character level range | Number, `*` = no limit |
| `MinMasterLevel` / `MaxMasterLevel` | Master level range | Number, `*` = no limit |
| `MinReset` / `MaxReset` | Reset range | Number, `*` = no limit |
| `MinMasterReset` / `MaxMasterReset` | Master reset range | Number, `*` = no limit |
| `DW` .. `ALQ` (15 columns) | Class permission, in class order DW, DK, FE, MG, DL, SU, RF, GL, RW, SL, GC, KUN, LEM, KIN, ALQ | `0` = not allowed, `1` = allowed at any evolution, `2` = needs 2nd evolution, `3` = needs 3rd evolution |

{% hint style="warning" %}
The header comment in the shipped file lists only 14 class columns (it stops at `KIN`), but the server reads **15**. The rows themselves have 15 values. Keep 15 values per row.
{% endhint %}

### Section 2: item reward for the winner

Only the 1st place receives these items, directly into the inventory. Several rows for the same arena are all given.

| Column | Meaning | Values |
|---|---|---|
| `Index` | Arena that gives this item | 0..29, `*` = every arena |
| `ItemIndex` | Item code | `Type * 512 + Index` |
| `Level` | Item level | 0..15 |
| `Dur` | Durability | 0..255, `0` = default. For stackable items, the quantity |
| `Skill` | Skill option | 0 / 1 |
| `Luck` | Luck option | 0 / 1 |
| `Option` | Additional option level | 0..7 |
| `ExcOp` | Excellent options bitmask | 0..63 |
| `SetOp` | Ancient option | 0..255 |
| `JoH` | Jewel of Harmony option | 0..255 |
| `OEx` | 380 option | 0..255 |
| `Socket1`..`Socket5` | Socket option per slot | `255` = empty. Only applied to socket items, up to the item's socket count |
| `BonusSocket` | Socket bonus option | `255` = none |
| `Duration` | Time-limited item | Seconds, `0` = permanent |
| `Class` | Only give to this class | Class number (0 = DW ... 14 = ALQ), `*` = any class |

Rows with invalid values (unknown item, level above 15, and so on) are ignored and logged in red.

## Example

```
0
//Index   Year   Month   Day   DoW   Hour   Minute   Second
0         *      *       *     *     22     10       0      // Survivor every day at 22:10
3         *      *       *     7     18     00       0      // BM x BM on Saturdays at 18:00
end

1
//Index Name        Alarm Stand Event Close StartGate FinalGate MinUser MaxUser MaxDeath MinLevel MaxLevel MinML MaxML MinReset MaxReset MinMR MaxMR DW DK FE MG DL SU RF GL RW SL GC KUN LEM KIN ALQ
0       "SURVIVOR"  5     0     7     0     450       1         4       100     *        0        400      *     *     85       *        *     *     1  1  1  1  1  1  1  1  1  1  1  1   1   1   1
3       "BMxBM"     5     0     5     0     453       1         4       100     *        0        400      *     *     *        *        *     *     0  1  0  0  0  0  0  0  0  0  0  0   0   0   0
end

2
//Index ItemIndex Level Dur Skill Luck Option ExcOp SetOp JoH OEx Socket1 Socket2 Socket3 Socket4 Socket5 BonusSocket Duration Class
0       7179      12    0   0     0    0      0     0     0   0   255     255     255     255     255     255         0        *     // Box of Kundun +5 for the Survivor winner
end
```

The Survivor arena opens for 5 minutes, needs at least 4 players with 85+ resets, and fights for 7 minutes. Arena 3 only accepts Dark Knights (any evolution, because the DK column is `1`). Write `3` in that column to accept only Blade Masters.

## Schedule

Start times use the shared format described in [Schedule syntax](../events/schedule-syntax.md). The time in section 0 is the moment the **entry window closes**: the window opens `AlarmTime` minutes before it.

```
0
//Index   Year   Month   Day   DoW   Hour   Minute   Second
0         *      *       *     *     10    10        0
0         *      *       *     *     14    10        0
1         *      *       *     7     17    00        0
end
```

Timeline of one run:

1. `AlarmTime` minutes before the scheduled time: entry opens, `CustomArenaText1` counts down every minute.
2. Scheduled time: entry closes (`Text2`), `StandTime` countdown starts (`Text3`).
3. Fight for `EventTime` minutes (`Text4`). It ends early when only one fighter is left.
4. Result (`Text6`, `Text7`), item reward, then players are moved to `FinalGate` after `CloseTime`.

The names shown in the in-game event time window (**H**) are not taken from this file: they come from section 2 of [`Data/Event/EventName.dat`](../events/event-name.md), one row per arena index.

```
2
//Key   Name
0       "Survivor"
1       "DM vs DM"
end
```

## Rewards

* **Item**: section 2 of `CustomArena.txt`, 1st place only, delivered to the inventory. Inventory space is checked item by item; an item that does not fit is not delivered and the winner sees `CustomArenaText20`.
* **Ranking**: every participant's score and position are saved to the database for the ranking windows.

Players enter through the move list. The shipped `Data/Move/Move.txt` has one entry per arena (indexes 54..61: `Survivor`, `DMxDM`, `GMxGM`, ...) pointing to gates 450..457, which are the `StartGate` values. Edit the price and level columns of those `Move.txt` rows to control the entry cost.

## Commands related

| Command | Who | What |
|---|---|---|
| `/startarena <index>` | Admin (`GameMaster = 2`, index 81 in `Data/Command.txt`) | Schedules arena `<index>` to close its entry 2 minutes from now, so entry opens immediately. |
| `/move <name>` | Player | Enters the arena while its entry window is open (needs the matching `Move.txt` row). |
| `/reload custom`, `/reload event` | Admin | See Reload module. |

## Pitfalls

* **`/startarena 0` does nothing.** The command rejects index 0, so the shipped Survivor arena (index 0) can only start from its schedule. Use index 1 or higher for arenas you want to start by hand, and never pass an index above 29.
* `/startarena` opens the arena even with `CustomArenaSwitch = 0`, but then kills score nothing and dead players do not respawn in the arena. Keep the switch on.
* The arena only works on **map 40** (the arena map). Entry rules are checked only when the gate leads to map 40, and players who leave map 40 are removed from the arena. A `StartGate` on another map lets anyone walk in without any check.
* A player in a party, a guild war, a duel or with a personal shop open cannot enter (`CustomArenaText8`). Tell players to leave their party first.
* If fewer than `MinUser` players are inside when the fight should start, the arena is cancelled silently and waits for its next scheduled time.
* Class columns: `0` blocks the class. A value of `2` or `3` also blocks characters below that evolution.
* The arena name in announcements comes from section 1; the name in the H window comes from `EventName.dat`. Change both when you rename an arena.
* Entering clears all of the player's buffs.

## Reload module

* `/reload custom` re-reads `CustomArena.txt` and the `CustomArena*` keys in `GameServerInfo - Custom.dat`.
* `/reload event` puts every arena back into its waiting state and recalculates the next start time. It is also what applies a changed `CustomArenaSwitch`. It interrupts an arena that is running.
* Safe order when changing the file: edit, `/reload custom`, then `/reload event` while no arena is running.
* `EventName.dat` (H window names) is reloaded by `/reload event`. `Move.txt` and `Gate.txt` are reloaded by `/reload move`.
