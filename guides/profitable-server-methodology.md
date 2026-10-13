# Profitable server methodology

A sustainable MU server is not the one with the highest rates or the biggest launch. It is the one where players **arrive easily, stay for months, and pay because they want to**, not because they have to. This guide is a method in eight phases for getting there with Morpheus Emulator.

{% hint style="info" %}
Every number on this page (rates, prices, percentages, targets) is an **illustrative example**. It shows the reasoning, not a guaranteed result. Your community, region and season will produce different numbers. Measure your own and adjust.
{% endhint %}

## The core idea: one server, every screen

Most MU servers compete for the same small group of PC players. Morpheus lets you compete for a much larger audience, because the **same server, the same account and the same character** are playable on:

| Platform | What it means for your business |
|---|---|
| Windows (32 and 64 bit, with launcher) | Your core, highest-engagement players |
| Android | The largest audience in most regions, playing in short sessions |
| iPhone and iPad | Native client at 120 fps on recent devices: a premium experience on Apple hardware |
| macOS | Players who could never join a Windows-only server |
| Web browser | No install at all: the cheapest trial you can offer |
| Mobile browser | The same tab, with touch controls |

A player on a phone joins the same party as a player on a PC. That changes the economics of a server: you can acquire players where it is cheap (web, mobile), keep them with systems designed for busy people (offline helper, Battle Pass, calendar), and let them move to the platform that suits them best without losing anything.

See the [Feature overview](../features.md#one-server-every-platform) and the [Multi-platform playbook](multiplatform-playbook.md).

## The eight phases at a glance

| # | Phase | Key question | Main tools |
|---|---|---|---|
| 1 | [Positioning](#phase-1-positioning) | Who is this server for? | Rates, resets, [Balance Class](../muserver/customs/balance-class.md) |
| 2 | [Launch plan](#phase-2-launch-plan) | How do we open with momentum? | Closed beta, [Client Hub](../dashboard/client-hub/README.md), [backups](../dashboard/backups-and-restore.md) |
| 3 | [Acquisition per platform](#phase-3-acquisition-per-platform) | Where do new players come from? | Web client, mobile apps, Discord Rich Presence |
| 4 | [Retention](#phase-4-retention) | Why do they come back tomorrow? | [Battle Pass](../muserver/customs/battle-pass.md), [events](../muserver/events/README.md), [Morpheus Tournament](../muserver/events/morpheus-tournament.md), offline helper |
| 5 | [Monetization design](#phase-5-monetization-design) | What do we sell, and what never? | [Morpheus Shop](../muserver/customs/morpheus-shop.md), [Cash Shop](../muserver/data/cash-shop.md), [Buy VIP](../muserver/customs/buy-vip.md) |
| 6 | [Economy health and balance](#phase-6-economy-health-and-balance) | Is the game still fair after 3 months? | [EventItemBag editor](../dashboard/editors/event-item-bag.md), [Balance Class](../muserver/customs/balance-class.md) |
| 7 | [Operations](#phase-7-operations) | Can we change things without hurting players? | [Hot reload](../dashboard/services-and-reload.md#hot-reload), [audit](../dashboard/logs-and-audit.md), notifications |
| 8 | [KPIs and routine](#phase-8-kpis-and-routine) | Is it working? | [Server dashboard](../dashboard/server-dashboard.md), your own database and sales data |

---

## Phase 1: Positioning

Before you touch a single rate, decide **who** you are building for. Every later decision (rates, events, store, platforms to promote) follows from this.

### Choose the server type

| Type | Typical experience rate (illustrative) | Pace | Who it attracts | Monetization fits best |
|---|---|---|---|---|
| Low rate | Low, long progression | Months to reach the end game | Nostalgic, patient, often adult players | Convenience, VIP, cosmetics, Battle Pass |
| Mid rate | Moderate | Weeks to reach the end game | The broadest audience | Balanced mix of everything |
| High rate | High, fast resets | Days to reach the end game | Players who want PvP and action now | Cosmetics, seasons, tournament entries, short passes |

The experience and drop rates live in [GameServerInfo - Common](../muserver/gameserver/common.md) (for example `AddExperienceRate_AL0..3` and `ItemDropRate_AL0..3`, one value per account level), and resets in [Reset, Master Reset and experience tables](../muserver/data/reset-and-experience.md).

### Choose the play focus

| Focus | What to emphasise | Morpheus systems that support it |
|---|---|---|
| PvP | Fair fights, competitions, rankings | [Balance Class](../muserver/customs/balance-class.md) (single PvP formula), [Morpheus Tournament](../muserver/events/morpheus-tournament.md), [PvP 1x1](../muserver/events/pvp-1x1.md), [GvG](../muserver/events/gvg-event.md), [TvT](../muserver/events/tvt-event.md), [Castle Siege](../muserver/events/castle-siege.md) |
| PvM | Progression, bosses, loot | [Custom Monster](../muserver/customs/custom-monster.md), [Invasions](../muserver/events/invasions.md), [EventItemBag](../dashboard/editors/event-item-bag.md), [Custom Quest](../muserver/customs/custom-quest.md) |
| Hybrid | Both, with separated spaces | Non-PvP server slots in the [server list](../dashboard/client-hub/client-files.md#server-list-serverlistbmd), events on a timetable |

### Choose the season of play

Decide how long a "season" of your server lasts before something resets or renews. A defined season gives players a reason to start (fresh start), a reason to stay (season goals), and a reason to return (next season).

| Season model | Description | Good for |
|---|---|---|
| Endless | One world forever, renewed by content and Battle Pass seasons | Low and mid rate |
| Seasonal content | Same world, new Battle Pass season, events and store rotation every few weeks | Most servers |
| Seasonal world | A new server or full restart every few months | High rate, PvP-focused |

The [Battle Pass](../muserver/customs/battle-pass.md) is built for seasons: each season has its own start and end dates, track, missions and 28-day calendar.

### Choose the target audience

| Audience | Mobile-first casual | PC hardcore |
|---|---|---|
| Session length | Short, several per day | Long evenings |
| What they value | Progress while away, simple goals, daily rewards | Competition, rare loot, rankings |
| Key systems | Offline MU Helper, Battle Pass calendar, daily missions, party voice | Tournament, rankings, Castle Siege, Balance Class reports |
| Platforms to promote first | Android, iPhone, mobile browser | Windows, macOS |
| Rates | Mid or low, with generous offline progression | Any, depending on the PvP focus |

{% hint style="success" %}
You do not have to pick one audience forever. A common winning pattern is **mobile-first acquisition with a PC end game**: casual players discover the server on the phone, and the most engaged ones move to the PC client for sieges and tournaments, keeping the same character.
{% endhint %}

### Positioning checklist

- [ ] One sentence describing the server (for example: *"Mid-rate PvP season of 90 days, playable on phone and PC"*).
- [ ] Server type, play focus and season length decided.
- [ ] Primary audience and the two platforms you will promote first.
- [ ] Experience, drop and reset settings drafted to match the pace you promise.
- [ ] A clear statement of what will **never** be sold (see [Phase 5](#phase-5-monetization-design)).

---

## Phase 2: Launch plan

A launch is a single chance to concentrate attention. Plan it as a sequence, not a date.

{% stepper %}
{% step %}
### Weeks -6 to -4: build and brand

- Configure rates, resets, events and starting items ([Start Item](../muserver/customs/start-item.md)).
- Brand what players see: the server list and game texts in [Client files](../dashboard/client-hub/client-files.md), and the launcher, login, loading and shop screens in the [UI Editor](../dashboard/client-hub/ui-editor.md).
- Set up Discord presence and its buttons in [Client settings](../dashboard/client-hub/client-settings.md) (Discord tab), so every early player advertises your server.
- Create a full [backup](../dashboard/backups-and-restore.md) of the finished configuration.
{% endstep %}

{% step %}
### Weeks -4 to -2: closed beta

- Invite a small group across **all** the platforms you will promote. Ask each tester which device they use.
- Run every scheduled event at least once. Check rewards in the [EventItemBag editor](../dashboard/editors/event-item-bag.md) **Chances** tab.
- Run the [Balance Class](../muserver/customs/balance-class.md) analysis and read the report before players do.
- Test a Battle Pass season end to end: XP curve, daily cap, claims, Premium purchase.
- Watch the [Server dashboard](../dashboard/server-dashboard.md) for the platform split of online players.
{% endstep %}

{% step %}
### Weeks -2 to 0: pre-launch

- Publish the final client update from [Publishing updates](../dashboard/client-hub/publishing-updates.md) a few days before launch, so launchers are already updated on day one.
- Announce the launch date, the season length and the "never sold" list.
- Open pre-registration in your community channels; prepare a launch-week event calendar.
- Wipe beta characters if you promised a fresh start.
{% endstep %}

{% step %}
### Launch day

- Lead with the **web browser** link: zero friction, anyone can join in one click.
- Have staff online in game and on Discord.
- Keep the panel open on the dashboard: services, online players by platform, live console.
- Start a well-known event (Blood Castle, Devil Square) at peak time from the panel to create a first shared moment.
{% endstep %}

{% step %}
### Days 1 to 30: stabilise and hook

- Daily: fix issues with [hot reload](../dashboard/services-and-reload.md#hot-reload) instead of restarts.
- Week 1: run a [Bonus Manager](../muserver/events/bonus-manager.md) experience weekend to help late joiners catch up.
- Week 2: first [Morpheus Tournament](../muserver/events/morpheus-tournament.md) edition with a visible prize.
- Week 3 to 4: first limited-time store offer tied to an event (see the [Monetization catalog blueprint](monetization-catalog.md#limited-time-offers-tied-to-events)).
- Day 30: review the [KPIs](#phase-8-kpis-and-routine) and decide what to change for month two.
{% endstep %}
{% endstepper %}

---

## Phase 3: Acquisition per platform

Each platform plays a different role in bringing players in. Think of them as a funnel.

```
 Discover            Try                     Commit                       Stay
 ─────────           ─────────               ─────────                    ─────────
 Discord profile  →  Web browser tab     →   Install Android / iPhone /  → Same account,
 Content creators    (nothing to install)    Windows / macOS client        any device
 Referrals           Mobile browser
```

### The web browser as a zero-friction trial

The browser client is the cheapest front door you have: a link opens the game in a tab, with login, character selection and the full world. Use it as the default call to action in every ad, video description and post.

- Put the browser link first in your Discord, website and social media.
- Once players are hooked, invite them to install the full client for their device. They keep the same account and character.
- Use the Google sign-in option where it fits your audience ([Client settings reference](../muserver/client/client-settings-reference.md), `[SocialLogin]`), to remove the account-creation step.

### Mobile apps and store presence

Mobile stores are discovery channels in themselves. A presence on Google Play and the App Store makes your server searchable by people who have never heard of MU private servers. Store publishing is covered at a high level in the [Multi-platform playbook](multiplatform-playbook.md#store-publishing-considerations).

### Discord Rich Presence: free marketing

Every connected player shows their character and your server's name on their Discord profile, visible to their whole friend list. Turn it on and configure it in [Client settings](../dashboard/client-hub/client-settings.md) (Discord tab). Recommended:

| Setting | Why |
|---|---|
| Show level and resets | Progress is a conversation starter |
| Show the platform (PC or Mobile) | Tells friends they can play from the phone too |
| Two buttons (for example *Play in browser* and *Join Discord*) | Turns a profile view into a click |

See [Discord Rich Presence](../features.md#discord-rich-presence) and the `[DiscordPresence]` keys in the [Client settings reference](../muserver/client/client-settings-reference.md).

### Content creators

- Give creators a reason to stream: a [Morpheus Tournament](../muserver/events/morpheus-tournament.md) final, a Castle Siege, a boss invasion.
- The mobile camera can hide the interface for clean screenshots and clips.
- Offer creators a cosmetic or a code-free in-game reward, never power items.

### Referrals

Morpheus does not need a dedicated referral system for this to work. A simple manual programme is enough at first:

| Step | How |
|---|---|
| Player brings a friend | Friend mentions the referrer when they reach a level milestone |
| Staff verifies | Check both characters in the online player list and rankings |
| Reward both | Deliver a cosmetic or coins with the moderation commands (`/gift`, `/reward`) listed in [Commands](../muserver/data/commands.md) |

### Acquisition checklist

- [ ] Browser link is the primary call to action everywhere.
- [ ] Discord Rich Presence on, with buttons.
- [ ] Store pages ready for the mobile platforms you promote.
- [ ] One creator-friendly event per week.
- [ ] Referral reward defined and announced.

---

## Phase 4: Retention

Acquisition fills the server; retention keeps it full. Most revenue comes from players who stay for months, so this phase deserves the most attention.

### Retention levers and what they solve

| Lever | Problem it solves | Docs |
|---|---|---|
| Battle Pass seasons | "I have nothing to aim for" | [Battle Pass](../muserver/customs/battle-pass.md), [editor](../dashboard/editors/battle-pass.md) |
| 28-day login calendar | "I forgot to log in" | [Battle Pass](../muserver/customs/battle-pass.md#season-section-5-login-calendar) |
| Daily and weekly missions | "I don't know what to do today" | [Battle Pass](../muserver/customs/battle-pass.md#season-section-4-missions-pool) |
| Events calendar | "Nothing happens on this server" | [Events](../muserver/events/README.md), Event Time window in [Server features](../muserver/customs/server-features.md#event-time-window) |
| Morpheus Tournament | "There is no place to prove myself" | [Morpheus Tournament](../muserver/events/morpheus-tournament.md) |
| Offline MU Helper | "I can't leave my PC on all day" | [Server features](../muserver/customs/server-features.md#mu-helper-offline) |
| Offline store | "I have no time to sell my items" | [Server features](../muserver/customs/server-features.md#custom-store-offline-store) |
| Rankings and Top lists | "Nobody sees my progress" | [Ranking](../muserver/customs/ranking.md), [Custom Top](../muserver/customs/custom-top.md), [Rank User](../muserver/customs/rank-user.md) |
| Party voice | "Playing alone is boring" | [Server features](../muserver/customs/server-features.md#party-voice) |
| Online rewards | "Being online is not worth it" | [Server features](../muserver/customs/server-features.md#reward-online-users) |

### Battle Pass seasons

The Battle Pass is your retention engine and a monetization product at the same time.

- **Match the season to the curve.** The shipped season needs 850,000 pass XP; with a daily cap of 20,000 a player hitting the cap every day finishes in about 43 days. Design the curve so a regular player finishes the free track a little before the season ends.
- **Cap passive XP.** Start every passive source with a tight daily cap so bots or offline farming cannot finish the season in a day.
- **Make the free track worth it.** A free track that feels empty pushes players away instead of towards Premium.
- **Use the claim grace days** so players who miss the last day are not punished.
- **Plan the next season before the current one ends.** When the end date passes, the pass stops progressing until you activate the next season.

### Events calendar

Build a weekly rhythm players can memorise. Example (illustrative):

| Day | Prime-time anchor | Why |
|---|---|---|
| Monday | [Blood Castle](../muserver/events/blood-castle.md) + [Devil Square](../muserver/events/devil-square.md) | Classic, counts for Battle Pass missions |
| Tuesday | [Quiz](../muserver/customs/quiz.md) + [Online Lottery](../muserver/customs/online-lottery.md) | Low effort, rewards being online |
| Wednesday | [Invasions](../muserver/events/invasions.md) | PvM group content |
| Thursday | [TvT](../muserver/events/tvt-event.md) or [GvG](../muserver/events/gvg-event.md) | Team PvP |
| Friday | [Morpheus Tournament](../muserver/events/morpheus-tournament.md) | Weekly competitive highlight |
| Saturday | [Castle Siege](../muserver/events/castle-siege.md) or [King of MU](../muserver/events/king-of-mu.md) | Guild content |
| Sunday | [Bonus Manager](../muserver/events/bonus-manager.md) experience window + [Event Drop](../muserver/customs/event-drop.md) | Catch-up day for casual players |

Players see the countdown to every scheduled event in the Event Time window (H key). Schedules use one common format: see [Schedule syntax](../muserver/events/schedule-syntax.md).

### Morpheus Tournament

The Colosseum runs itself: registration, draw, judging, ranking, history and prizes. That makes it a reliable weekly highlight without staff effort.

- Charge an **entry item** to give the slot weight and keep no-shows out.
- Pay prizes in coins and items for the champion, the runner-up and each match won.
- Keep the Ranking and History tabs on: a visible history is a reason to keep competing.
- Betting ships off. If you enable it, use a coin rather than Zen, as the tournament page recommends.

### Offline MU Helper for adult and mobile players

This is the feature that best retains players who cannot leave a device running. Use the per-account-level settings to make it fair:

| Setting | Free (illustrative) | VIP (illustrative) |
|---|---|---|
| Maximum offline time | A few hours | Longer, or unlimited |
| Experience rate while offline | Lower than online | Slightly higher, still below online |
| Potion and buff delays | Default | Faster, within the documented minimums |

{% hint style="warning" %}
Keep offline progress **below** active play. If being offline is as good as playing, players stop logging in, and an empty world loses everyone.
{% endhint %}

### Retention checklist

- [ ] Battle Pass season live, curve tested against the daily cap.
- [ ] Weekly event rhythm published and visible in the Event Time window.
- [ ] Weekly tournament scheduled, with prizes announced.
- [ ] Offline helper limits set per account level.
- [ ] Rankings visible in game and shared on Discord.

---

## Phase 5: Monetization design

Monetization is **designed**, not added. The goal is that paying feels like a good deal, and not paying still feels like a fair game.

### The three in-game coins

Morpheus stores three coins per account: **WCoinC**, **WCoinP** and **GoblinPoint**. The store, VIP, Battle Pass and tournament systems can each charge or pay in any of them. Give each coin a clear role. A recommended split:

| Coin | Suggested role | How players get it | What it buys |
|---|---|---|---|
| WCoinC | **Premium coin** | Bought from you (through your own sales channel) | VIP, Battle Pass Premium, store items, inventory extension |
| WCoinP | **Event coin** | Tournament prizes, Battle Pass rewards, event rewards | A selected part of the store, VIP plans, tournament entries |
| GoblinPoint | **Earned coin** | Online rewards, boss kills, missions | Consumables and a limited set of store items |

How each coin can be **earned in game**:

| Source | Coins | Docs |
|---|---|---|
| Online rewards (paid at an interval, per account level) | Any of the three | [Server features](../muserver/customs/server-features.md#reward-online-users) |
| Boss kills (top-damage player) | Any of the three | [Custom Monster](../muserver/customs/custom-monster.md) |
| Battle Pass prizes (track, missions, calendar) | Any of the three | [Battle Pass](../muserver/customs/battle-pass.md#season-section-2-prize-catalog) |
| Tournament prizes | Any of the three | [Morpheus Tournament](../muserver/events/morpheus-tournament.md#sections-7-8-9-prizes) |
| Cash Shop purchase bonus | GoblinPoint | [Cash Shop editor](../dashboard/editors/cash-shop.md#package-details) |
| Exchange commands | Converts between coins at your rate | [Exchange Coin](../muserver/customs/exchange-coin.md) |
| Player-to-player sales in coin stores | Any of the three | [Server features](../muserver/customs/server-features.md#custom-store-offline-store) |

{% hint style="success" %}
Letting non-paying players **earn** some premium value (through events, missions and coin stores where paying players sell to them) is what keeps a free player population large. That population is what paying players want to play with.
{% endhint %}

### Product families

| Family | Example | Morpheus system |
|---|---|---|
| VIP tiers | VIP 1 / 2 / 3 for 7 or 30 days | [Buy VIP](../muserver/customs/buy-vip.md) (End key window) |
| Season pass | Premium and Premium+ plans | [Battle Pass](../muserver/customs/battle-pass.md#engine-section-2-premium-plans) |
| Item builder | Items built within your limits, for 1, 7, 30 days or permanent | [Morpheus Shop](../muserver/customs/morpheus-shop.md) |
| Packages | Bundles, pets, seals, time-limited items | [Cash Shop](../muserver/data/cash-shop.md) (X key) |
| Convenience | Inventory extension rows | [Server features](../muserver/customs/server-features.md#buy-inventory-extension) |

The full catalog, with illustrative price ladders, is in the [Monetization catalog blueprint](monetization-catalog.md).

### Convenience versus pay-to-win guardrails

| Safe to sell (convenience, time, style) | Sell with limits | Do not sell |
|---|---|---|
| Inventory extension | Item builder with **capped** options | The best item in the game at maximum options |
| VIP time (offline helper hours, Bot Buffer access) | Experience seals and buffs (time-limited) | Items that decide PvP on their own |
| Cosmetics, pets for looks, wings with modest stats | Battle Pass Premium with strong items **late** in the track | Tournament wins, rankings or titles |
| Battle Pass Premium | Jewels in small, limited bundles | Anything that can only be obtained by paying and gives power |

Your guardrails in Morpheus:

- **Morpheus Shop limits** (Section 2): maximum level, luck, skill, additional option, +380, excellent count, sockets and harmony per item range. This is how you sell a convenience item without selling the top of the end game.
- **Balance Class item bonus cap** (`MaxItemBonus`): limits how much gear can add in PvP, so bought gear cannot decide a fight alone.
- **Time-limited products**: 1, 7 and 30-day versions keep revenue recurring and avoid permanent power creep.

### Monetization checklist

- [ ] Each coin has one clear role, written down and announced.
- [ ] Every product is classified: convenience, cosmetic, boost, season pass or VIP.
- [ ] The "never sold" list is public.
- [ ] Morpheus Shop limits reviewed for every item range you sell.
- [ ] At least one way to earn each coin in game.

---

## Phase 6: Economy health and balance

An economy that inflates or a class that dominates will drain your server slowly. Check both on a schedule, not when complaints arrive.

### Drop rates and boxes

| Lever | Where | What to watch |
|---|---|---|
| Global drop rate per account level | [GameServerInfo - Common](../muserver/gameserver/common.md) | Jewel prices between players falling fast = too much supply |
| Item drop rules | [Item drop](../muserver/data/item-drop.md) | Excellent and ancient items becoming common |
| Boxes, boss drops, event prizes | [EventItemBag editor](../dashboard/editors/event-item-bag.md) | Real chance per class |
| Temporary bonuses | [Bonus Manager](../muserver/events/bonus-manager.md) | Use for events, not as a permanent rate |

Use the EventItemBag editor's **Chances** tab before every change to a box. It calculates the chance of each result with the same algorithm as the GameServer, per class and evolution, and **Simulate 1000 openings** confirms it. Publish the chances of paid boxes to your community: transparency builds trust.

### Answering "class X is overpowered"

The [Balance Class](../muserver/customs/balance-class.md) analysis tool simulates all 225 class matchups with your server's real formulas and produces a report with the full win-rate matrix and proposed changes.

{% stepper %}
{% step %}
### Collect the complaint

Note which matchup, which gear level and which content (duel, siege, tournament).
{% endstep %}

{% step %}
### Run the analysis

It runs in analysis mode by default and makes nothing change until you confirm. Read the win-rate matrix for that matchup.
{% endstep %}

{% step %}
### Decide and communicate

If the matchup is outside the healthy band (45% to 55% by default), apply a small adjustment and announce it with the report. If it is inside, share the report: the complaint becomes a conversation about data.
{% endstep %}

{% step %}
### Apply safely

The tool makes an automatic backup before any change. Reload the `custom` module when few players are fighting, as the item bonus is recalculated per player afterwards.
{% endstep %}
{% endstepper %}

### Inflation control

| Symptom | Likely cause | Response |
|---|---|---|
| Zen worthless, jewels everywhere | Drop too high or too many bonus windows | Lower drop rates, shorten bonus windows, add jewel sinks (mixes, [Custom Mix](../muserver/customs/custom-mix.md)) |
| Coins piling up unused | Earning sources too generous, few things to buy | Lower online rewards, add new store items, add tournament entry costs |
| Everyone at maximum level | Rates too high for the season length | New Battle Pass season, reset or master reset content |
| Store items appear in player shops at low prices | Too many sold or dropped | Reduce availability, prefer time-limited versions |

### Resets as a progression valve

Resets and master resets let you extend progression without new maps. Tune them in [Reset, Master Reset and experience tables](../muserver/data/reset-and-experience.md). The HP bonus per master reset ([Server features](../muserver/customs/server-features.md#hp-bonus)) and rank titles ([Rank User](../muserver/customs/rank-user.md)) make each reset feel like an achievement.

---

## Phase 7: Operations

Players forgive a lot, but not lost items or a server that is down at prime time. Good operations protect revenue directly.

| Practice | How Morpheus supports it | Docs |
|---|---|---|
| Change without downtime | Hot reload of items, monsters, shops, Cash Shop, customs, languages and EventItemBag: nobody is disconnected | [Services and reload](../dashboard/services-and-reload.md) |
| Never lose a configuration | Automatic copy on every save, folder backups with compare and selective restore | [Backups and restore](../dashboard/backups-and-restore.md) |
| Know who changed what | Append-only activity log | [Logs and audit](../dashboard/logs-and-audit.md) |
| Ship client updates to every platform at once | One Client Hub publish; players download only what changed when they open the game | [Publishing updates](../dashboard/client-hub/publishing-updates.md) |
| Be told when something breaks | Panel notifications by type (urgent, status, updates, news) | [Account and security](../dashboard/account-and-security.md#notification-preferences-morpheus-sender) |
| Read server logs from anywhere | Per-service logs in the browser | [Server logs](../dashboard/server-logs.md) |
| Edit safely | Validation, conflict protection and all-or-nothing saves | [Validation and conflicts](../dashboard/validation-and-conflicts.md) |

### Change management rules

1. **Back up before big changes**: a new season, a mass price change, a new set of boxes.
2. **One change at a time**, then reload, then check the console.
3. **Reload between events**: `/reload event` resets running events, so pick a quiet moment.
4. **Test risky files first.** A malformed Battle Pass file can stop the GameServer even with the system switched off.
5. **Group client changes** and publish them together, outside peak hours.
6. **Communicate**: post every player-facing change in your patch notes channel.

{% hint style="info" %}
Remember which changes need a client publish. Prices in the Morpheus Shop only need a GameServer reload, but adding or removing an item from its list, Cash Shop changes, Buy VIP plans and feature switches also need a client update. See [Server-side vs client-side changes](../dashboard/client-hub/README.md#server-side-vs-client-side-changes).
{% endhint %}

---

## Phase 8: KPIs and routine

What you do not measure, you cannot improve. Keep a small set of numbers and look at them on a fixed schedule.

### The KPIs that matter

| KPI | Definition | Where to get it | Healthy direction |
|---|---|---|---|
| Online now, by platform | Players connected, split PC / Mobile / Web | [Server dashboard](../dashboard/server-dashboard.md) | Stable or rising peaks |
| DAU / MAU | Unique accounts active per day / per month | Your account database (logins) | Ratio rising means stickier |
| Retention D1 / D7 / D30 | % of new accounts that come back after 1, 7 and 30 days | Your account database | D1 is your first-impression score |
| Conversion rate | % of active accounts that bought anything in the period | Your sales records and the Cash Shop log | Rising slowly, not spiking |
| ARPPU | Revenue divided by paying accounts | Your sales records | Stable; a spike can mean pay-to-win pressure |
| Platform mix | Share of players on each platform | Online players by platform | Matches the audience you chose |
| Battle Pass completion | % of Premium holders reaching the last level | Battle Pass log | Most Premium holders should finish |

Illustrative example of reading the numbers together:

| Observation | Possible reading | Action to consider |
|---|---|---|
| High web share, low D7 | Trials work, but players do not move to an installed client | Push the app install inside the game and on Discord |
| High mobile share, low conversion | Products do not fit short sessions | Offer offline helper time, Battle Pass, small bundles |
| High conversion, falling DAU | Paying players dominate, free players leave | Review the "never sold" list and Balance Class report |
| Tournament registrations falling | Same winners every week | Add level or reset brackets, rotate prizes |

{% hint style="warning" %}
Avoid optimising one number in isolation. Raising prices may raise ARPPU this month and lower DAU for the next three.
{% endhint %}

### Daily routine (10 to 15 minutes)

- [ ] [Server dashboard](../dashboard/server-dashboard.md): all services running, online count by platform, no unusual CPU or memory.
- [ ] Notifications bell: anything urgent or status-related since yesterday.
- [ ] [Server logs](../dashboard/server-logs.md): errors in the GameServer log, failed purchases in the Cash Shop log.
- [ ] Tournament or event results: unpaid prizes for offline winners (paid by hand from the log).
- [ ] Community: top questions and complaints.

### Weekly routine (1 hour)

- [ ] Review DAU, new accounts, conversions and revenue against last week.
- [ ] [Logs and audit](../dashboard/logs-and-audit.md): who changed what.
- [ ] Next week's events and limited offer prepared.
- [ ] Battle Pass: are players progressing at the expected pace?
- [ ] Publish pending client changes together in [Publishing updates](../dashboard/client-hub/publishing-updates.md).
- [ ] Create a folder [backup](../dashboard/backups-and-restore.md) of the GameServer.

### Monthly routine (half a day)

- [ ] D1 / D7 / D30 retention for the month's new accounts, by platform where possible.
- [ ] Run the [Balance Class](../muserver/customs/balance-class.md) analysis and publish the summary.
- [ ] Review drops and boxes in the [EventItemBag editor](../dashboard/editors/event-item-bag.md) Chances tab.
- [ ] Store review: best and worst sellers, adjust with bulk price adjustment in the [Cash Shop](../dashboard/editors/cash-shop.md) and [Morpheus Shop](../dashboard/editors/morpheus-shop.md) editors.
- [ ] Prepare the next Battle Pass season (curve, track, missions, calendar, plans).
- [ ] Decide one improvement for next month and announce it.

## Where to go next

- [Multi-platform playbook](multiplatform-playbook.md): turn each platform into a channel.
- [Monetization catalog blueprint](monetization-catalog.md): build the store.
- [Getting started](../getting-started.md): if you have not configured your server yet.
