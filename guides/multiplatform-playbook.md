# Multi-platform playbook

Morpheus Emulator runs **one server** for six ways of playing: Windows, Android, iPhone and iPad, macOS, the web browser and the mobile browser. Same account, same character, same party. This playbook explains how to use each platform as a channel: who plays there, how to reach them, and what to tune so they stay.

For the overall strategy, see the [Profitable server methodology](profitable-server-methodology.md).

{% hint style="info" %}
Player profiles and session lengths on this page are **typical patterns**, not measurements of your server. Check your own mix in the [Server dashboard](../dashboard/server-dashboard.md), which shows online players split by platform.
{% endhint %}

## Platforms at a glance

| Platform | Strength | Typical player | Typical session | Main role in your funnel |
|---|---|---|---|---|
| [Windows](#windows) | Full client in 32 and 64 bit, launcher that updates before entry | Veteran MU player, PvP and guild leader | Long evenings | End game, sieges, tournaments |
| [Android](#android) | The real game on the phone, with touch controls, updating itself when opened | Casual and returning players, students, commuters | Short, several per day | Volume and daily engagement |
| [iPhone and iPad](#iphone-and-ipad) | Native client at 120 fps on the iPhone 17 Pro | Players on premium devices | Short to medium | High-quality mobile experience |
| [macOS](#macos) | The same native client with its own Mac launcher | Mac owners shut out of Windows-only servers | Medium to long | An audience your competitors ignore |
| [Web browser](#web-browser) | Opens in a tab, nothing to install | Curious visitors, people on work or school computers | Short trial | Zero-friction trial |
| [Mobile browser](#mobile-browser) | The same tab with Android's touch controls | Visitors from social media on their phone | Very short trial | Instant trial from a link |

## How players move between platforms

The strongest pattern is not "pick a platform" but **start anywhere, stay everywhere**:

```
 Social post / Discord profile
            │
            ▼
   Browser tab (desktop or mobile)  ── try in seconds, no install
            │
            ├──► Installs Android or iPhone app ── plays daily in short sessions
            │
            └──► Installs Windows or macOS client ── joins sieges and tournaments
                                │
                                ▼
            Same account and character on every device
```

Make this movement easy:

- Always give the browser link first, then the install links.
- Tell players, in game and on Discord, that their character goes with them.
- Use the offline MU Helper so a player can start a hunt on the PC, close it, and check in from the phone later. See [Server features](../muserver/customs/server-features.md#mu-helper-offline).

---

## Windows

**Strengths.** The complete client in 32 and 64 bit. The launcher updates the game before entry, downloading only what changed and resuming if the connection drops. The interface uses modern panels while keeping MU's classic cursor.

**Typical player.** Long-time MU players, guild leaders, PvP competitors. They usually generate most of the competitive activity and a large share of spending.

**How to promote**

- MU communities, forums and server listing sites.
- Clips of tournament finals and Castle Siege.
- Discord Rich Presence: PC players often keep Discord open while playing.

**What to tune**

| Area | Recommendation | Where |
|---|---|---|
| Interface | Brand the launcher, login and loading screens | [UI Editor](../dashboard/client-hub/ui-editor.md) |
| Window title | Show character name, level or resets in the window title | [Client settings](../dashboard/client-hub/client-settings.md) |
| Competitive content | Tournament, sieges, rankings | [Morpheus Tournament](../muserver/events/morpheus-tournament.md), [Castle Siege](../muserver/events/castle-siege.md), [Ranking](../muserver/customs/ranking.md) |
| Fairness | Run the balance analysis regularly | [Balance Class](../muserver/customs/balance-class.md) |

---

## Android

{% embed url="https://www.youtube.com/watch?v=SwxIJgfAIU0" %}
Android and PC playing together on the same server.
{% endembed %}

{% embed url="https://www.youtube.com/watch?v=7Ek_AiOnG2g" %}
Super Camera 3D: the touch camera that makes the mobile game feel native.
{% endembed %}

**Strengths.** The full game, not a reduced version: on-screen joystick, skill ring, health and mana bars, minimap, quick potions, touch inventory and every game window, including chat, shop, map and warp. The app updates itself when opened. Party voice works on Android, so mobile parties can talk without another app.

**Typical player.** The largest audience in many regions: casual players, returning players who played MU years ago, people who play in breaks and on the move.

**How to promote**

- A Google Play listing (see [Store publishing considerations](#store-publishing-considerations)).
- Short vertical videos showing real gameplay on a phone.
- Messages in your community: "your PC character is already on your phone".
- Mobile browser link for people who are not ready to install yet.

**What to tune**

| Area | Recommendation | Where |
|---|---|---|
| Panels | Choose which windows use the modern panel and which use the classic native window | `InterfaceMobileSwitch` and the `*Webview` keys under `[Interface]` in the [Client settings reference](../muserver/client/client-settings-reference.md), edited in the **Mobile** tab of [Client settings](../dashboard/client-hub/client-settings.md) |
| Quick buttons | Auto Combo, Auto Attack, Auto Pick and Ctrl buttons | Same, `MobileBtn*` keys |
| Mobile HUD | Colours, images and layout of the touch interface | [UI Editor](../dashboard/client-hub/ui-editor.md) (Mobile HUD screen) |
| Session length | Daily missions, the 28-day login calendar and offline hunting suit short sessions | [Battle Pass](../muserver/customs/battle-pass.md), [Server features](../muserver/customs/server-features.md#mu-helper-offline) |
| Products | Small bundles and time-based products (VIP days, Battle Pass) fit mobile habits | [Monetization catalog blueprint](monetization-catalog.md) |

{% hint style="success" %}
Preview every mobile change before publishing: the UI Editor has **Mobile** and **Side by side** modes with Android, iPhone and Tablet sizes.
{% endhint %}

---

## iPhone and iPad

**Strengths.** A native client on Apple's graphics stack, running at a locked 120 frames per second on the iPhone 17 Pro, even in crowded combat with skill effects, and without the freeze when changing zones. It is the best-looking way to play MU on a phone.

**Typical player.** Players on premium devices who expect a polished experience. Often a valuable audience for cosmetics and season passes.

**How to promote**

- An App Store listing (see [Store publishing considerations](#store-publishing-considerations)).
- Highlight smoothness: screen recordings of crowded fights at high frame rate.
- iPad as a "bigger screen MU" for players who do not own a PC.

**What to tune**

| Area | Recommendation | Where |
|---|---|---|
| Panels and buttons | Same mobile keys as Android | [Client settings reference](../muserver/client/client-settings-reference.md) |
| Sign-in options | The Google sign-in button can be shown on all platforms, or hidden on iPhone and iPad only | `GoogleButtonRegisterType` in `[SocialLogin]`, [Client settings reference](../muserver/client/client-settings-reference.md) |
| Screenshots | The mobile camera can hide the interface for clean screenshots, useful for store pages and social media | In game |
| Preview | Check every screen at iPhone and Tablet sizes | [UI Editor](../dashboard/client-hub/ui-editor.md) |

---

## macOS

**Strengths.** The same native client as iPhone and iPad, with its own Mac launcher.

**Typical player.** People who wanted to play MU but own only a Mac. On most private servers they simply cannot join, so they are an audience with little competition.

**How to promote**

- Say it clearly: "playable on Mac" is a rare claim among MU servers.
- Mac and Apple communities, and players who already play on iPhone.

**What to tune**

| Area | Recommendation | Where |
|---|---|---|
| Content | Same end-game content as Windows: macOS players count as **PC** in the online list | [Server dashboard](../dashboard/server-dashboard.md) |
| Branding | Launcher and screens are shared with the other clients | [UI Editor](../dashboard/client-hub/ui-editor.md) |

---

## Web browser

{% embed url="https://www.youtube.com/watch?v=Z2CNeffaRm4&t=13s" %}
Playing MU Online in Google Chrome: login, character selection and gameplay with nothing to install.
{% endembed %}

**Strengths.** MU opens in a tab with nothing to install: login, character selection and the world, with audio and the same protection as the installed client. Google login is available. Party voice works in the browser too.

**Typical player.** Anyone who clicked a link: a friend's Discord profile, a video description, an ad. Also players on computers where they cannot install software.

**How to promote**

- Make the browser link the **primary call to action** everywhere: website, Discord, videos, Discord Rich Presence buttons.
- "Play now in your browser" removes the biggest barrier in MU acquisition: the download.

**What to tune**

| Area | Recommendation | Where |
|---|---|---|
| First impression | The loading and login screens are the first thing a visitor sees. Make them fast and clear | [UI Editor](../dashboard/client-hub/ui-editor.md) |
| Account creation | Google sign-in removes a step | `[SocialLogin]` in the [Client settings reference](../muserver/client/client-settings-reference.md) |
| First 15 minutes | Starting items and an easy early quest give an immediate sense of progress | [Start Item](../muserver/customs/start-item.md), [Custom Quest](../muserver/customs/custom-quest.md) |
| Conversion to install | Mention the installed clients in game texts and NPC dialogs | [Client files](../dashboard/client-hub/client-files.md) (game texts) |

{% hint style="info" %}
Track the **Web** share of online players in the [Server dashboard](../dashboard/server-dashboard.md). A large web share is a good sign for acquisition; the question to ask is how many of those players go on to install a client.
{% endhint %}

---

## Mobile browser

**Strengths.** The same browser client on a phone, with Android's touch controls.

**Typical player.** People arriving from social media on their phone who want to see the game before installing anything.

**How to promote**

- Links in social media bios and short videos.
- QR codes on banners and community posts.

**What to tune**

| Area | Recommendation | Where |
|---|---|---|
| Touch interface | Mobile and web share several panel settings (message boxes, master skill tree, event entry screens) | `*Webview` keys in the [Client settings reference](../muserver/client/client-settings-reference.md) |
| Next step | Invite players to install the Android or iPhone app for the best performance | Game texts in [Client files](../dashboard/client-hub/client-files.md) |

---

## One publish reaches every platform

You do not maintain six clients. Changes go through the [Client Hub](../dashboard/client-hub/README.md) once:

{% stepper %}
{% step %}
### Edit

Change the server list, game texts, items, client settings or screens in [Client files](../dashboard/client-hub/client-files.md), [Client settings](../dashboard/client-hub/client-settings.md) or the [UI Editor](../dashboard/client-hub/ui-editor.md). Shop editors add their own client files to the queue.
{% endstep %}

{% step %}
### Review

Open [Publishing updates](../dashboard/client-hub/publishing-updates.md) and check the pending queue. Group related changes into one update.
{% endstep %}

{% step %}
### Publish

Publish once. Players receive the new content automatically the next time they open the game, downloading only what changed.
{% endstep %}
{% endstepper %}

Because translations ship inside the client package, the seven in-game languages (English, Portuguese, Spanish, Chinese, Korean, Vietnamese and Thai) always match the game version on every platform. See [Multilingual server](../features.md#multilingual-server).

{% hint style="warning" %}
Players already in game keep the old files until they restart. Publish outside peak hours and announce important updates so players reopen the game.
{% endhint %}

## Store publishing considerations

Publishing on Google Play and the App Store puts your server in front of people who search the stores. Treat it as a project of its own. At a high level:

| Topic | What to prepare |
|---|---|
| Developer accounts | Your own accounts on each store, in your name or your company's name |
| Store listing | Name, short and long description, screenshots and a short video for phones and tablets. The mobile camera's hide-interface option helps with clean screenshots |
| Branding | Use your own server name, logo and art. Do not use names or art you do not have the rights to |
| Content rating | Fill in each store's rating questionnaire honestly (combat, player chat, voice chat, purchases) |
| Privacy | A privacy policy page describing what account data you keep |
| Purchases and digital goods | Read each store's current rules on selling coins, passes and other digital content before you design how players buy on mobile |
| Sign-in options | Check each store's current rules for third-party sign-in; the client lets you show or hide the Google button per platform |
| Review time | App updates go through store review. Plan releases ahead and keep day-to-day content changes in the Client Hub |

{% hint style="warning" %}
Store policies change over time and differ between regions. Always read the current official guidelines of each store before submitting, and before announcing a mobile launch date.
{% endhint %}

## Platform checklist

- [ ] Browser link tested on desktop and phone, used as the primary call to action.
- [ ] Discord Rich Presence on, showing the platform, with *Play* and *Discord* buttons.
- [ ] Mobile panels and quick buttons reviewed for Android and iPhone.
- [ ] Every client screen previewed in Desktop and Mobile modes before publishing.
- [ ] Store listings prepared for each mobile store you target.
- [ ] Players told, in game and on Discord, that their character is the same on every device.
- [ ] Platform mix reviewed weekly in the Server dashboard.
