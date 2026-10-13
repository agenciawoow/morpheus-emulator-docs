# Battle Pass editor

**Season, prizes, track, missions and calendar of the pass, with texts in every language, in one place.** The engine file, the season file and the language files are saved together.

For the meaning of every field in the files, see the MuServer reference: [Battle Pass](../../muserver/customs/battle-pass.md).

## How the Battle Pass works

1. **Turn on both switches** – `CustomBattlePassSwitch` in `GameServerInfo - Custom.dat` (server) and in the client settings (client, see [Client Hub → Client settings](../client-hub/client-settings.md)). With only one on, the button does not show.
2. **Pick the season** – the engine lists the seasons; only the **active** one is loaded. Each season has its own file with XP curve, prizes, track, missions and calendar.
3. **Build the prize catalog** – each prize has a number. It can be an item, Zen, coins, XP or a buff. Track, missions and calendar only point to that number.
4. **Fill the track** – one prize per level in the free row and another in the premium row. Mark milestones with the star.
5. **Create the missions** – daily, weekly and season. Title and description are typed per language; the message number is allocated automatically.
6. **Texts per language** – Portuguese (POR) is the reference. A translation whose `%d`/`%s` differ from POR is dropped by the server. Empty uses POR.
7. **Save and reload** – Save writes engine, season and languages together. Then **Reload custom**; if a text changed, also **Reload lang**. Nothing to publish to the client.

## Screen layout

- **Top bar**: Save → Reload bar, the **Season** selector (`index · name`, the active one marked **active**), re-read from disk and full screen.
- **Status panel** with the **Client** and **Server** switch chips. When everything is fine: **Pass ready: switches on and nothing pending**.
- **Sections** pane on the left: **General**, **Season**, **Prizes**, **Track**, **Missions**, **Calendar**, **Plans**, **XP sources**, **Advanced**.

{% hint style="info" %}
If you pick a season other than the active one, a note reminds you: *You are editing season N, which is not the active one.* Changes are saved, but players only see them when that season becomes active.
{% endhint %}

## General

| Field | Description |
|---|---|
| **Season live** | The active season. **"none" turns the pass off for players.** |
| **Daily** / **Weekly** | How many missions the player panel announces. Register exactly this many. |
| **Extra weeklies (Premium)** | Weeklies marked "Premium only". |
| **Daily XP cap** | 0 = no cap. |
| **Days to claim after the end** | After that, unclaimed prizes are gone. |
| **Day rollover hour (UTC)** | 0 to 23, server clock. |
| **Missing a day resets the calendar streak** | Off: the player continues where they stopped. |

New seasons are registered in **Advanced** mode (Seasons section). The engine supports a fixed maximum number of seasons.

## Season

| Field | Description |
|---|---|
| Season name (per language) | Name the player sees at the top of the pass, e.g. *Ascension of Kundun*. |
| **Start** / **End** | Dates in server time. Outside the period the pass does not show (**outside period** / **live** badge). |
| **Max level** | The track goes up to this level (max 200). |
| **XP curve** | XP needed per level, by level band (**From level**, **To level**, **XP per level**). Add bands with **Band**. The curve must cover every level — the editor shows `N of M levels covered` and **Total to finish the track**. |

## Prizes

The prize catalog. Search with **Search prize by name, number or kind**; the counter shows `N of max prizes`. Each prize shows where it is used (`track 3 · missions 1 · calendar 0`) or **unused**.

| Field | Description |
|---|---|
| **Number** | Track, missions and calendar point to this number. |
| **Kind** | **Item**, **Zen**, **WCoin (C)**, **WCoin (P)**, **Goblin Point**, **Experience**, **Buff**. |
| **Quantity** / **Value** | Amount delivered. Stackable items use one slot with the stack. |
| **Item delivered** | For items: **Pick item…**, **Level (+)**, **Durability** (0 = full; leave 0 for stackable items), **Time (seconds)** (0 = permanent; 86400 = 1 day). |
| Item options | **Show item options** for **Skill**, **Luck**, **Option (+)** (0–28), **Excellent** (bitmask 0–63), **Ancient**, **Harmony**, **380 option**, **Sockets** (255 = empty). |
| **Buff (aura)** | For buffs: **EffectID** and a required time (e.g. 2592000 = 30 days). |
| **Icon** | Empty = automatic. You can borrow another item's icon or use custom art. |
| **Note** | Comment saved in the file. |

{% hint style="warning" %}
Removing a prize that is still used shows *It is used in N place(s) of the track, missions or calendar; those places will point to nothing.* Replace those references first.
{% endhint %}

## Track

A grid with one row per level and two columns: **Free** and **Premium**. **Click a cell to pick the prize**; click the star to mark a **Milestone** (big card); **Clear** empties a cell. A meter shows how much of the definition size is used. Cells pointing to a deleted prize show `prize #N does not exist`.

## Missions

*Title and description per language; the message number is allocated automatically.* The header shows how many free text slots remain for this season.

| Field | Description |
|---|---|
| **Kind** | **Daily**, **Weekly**, **Season**. |
| **Condition** | **Kill monsters**, **Minutes online**, **Complete event**, **Reach level**, **Win PvP**, **Use item**, **Hold item**, **Reset**. Extra parameters (Map, Monster, Event, Item) appear depending on the condition; **any** means no restriction. |
| **Target** | How many (monsters, minutes, times, level, wins, uses, items, resets). |
| **Pass XP** | XP gained on claim. |
| **Premium owners only** | Counts as an extra weekly of the plan. |
| **Extra prize** | Optional, besides the XP. |
| **Title** / **Description** | One field per language, with a byte counter. |

## Calendar

28 login days. Pick a prize per **Day N**, mark milestones with the star, and use **Sort by day** — the server delivers the first unclaimed day **in file order**.

## Plans

Premium plans the player buys in the pass panel, paid with the coin configured in `Custom.dat`.

| Field | Description |
|---|---|
| **Kind** | **Premium (1)** or **Premium+ (2)**. |
| **Price** | In the configured coin. |
| **Levels granted** | Levels raised on purchase. |
| **XP bonus (%)** | Applied before the caps. |
| **Extra weeklies** | Display only. |
| **Plan name** | Per language. |

No plans = the pass only has the free row.

## XP sources

**Passive XP** the player earns by playing, outside missions: **Monster kill**, **Minute online**, **Event completed**, **Reset**, **Master reset**, each with **XP**, **Daily cap** and **Min level**. Also here: **Fixed icons** for non-item prizes and **Multipliers** (currently read but not applied by the server).

## Texts per language

Every text field shows a counter (`42/255 bytes`) and messages such as **required (reference)**, **placeholders differ from the reference** or **empty = uses POR**. Keep `%d` and `%s` identical to the Portuguese text, or the server drops the translation.

## Advanced

Raw tables per section of the engine and the season. Saving here is validated like visual mode and **does not touch** the language texts. Switching between Advanced and visual modes re-reads the files — save first.

## Saving and applying

1. Click **Save**. Message: *Saved. Reload the custom module on the GameServer to apply.* If texts changed: *Texts changed in …: reload custom and lang on the GameServer.*
2. Click **Reload custom (GameServer)** — marked **needed** after every save.
3. If texts changed, click **Reload lang (GameServer)** (marked **needed**).

There is nothing to publish to the client for the Battle Pass content.

{% hint style="info" %}
If the editor says *The engine does not exist at …*, your server is missing the Battle Pass engine file. The editor does not create it from scratch — contact support.
{% endhint %}
