# Morpheus Emulator

Welcome to the official documentation of **Morpheus Emulator** — a complete MU Online server
with 40+ custom systems, 32 events, cross-platform play (Windows, Android, iPhone/iPad, macOS and
the browser) and a web control panel that lets you run everything without touching the machine.

{% hint style="info" %}
New here? Start with [Getting started](getting-started.md). It takes you from your first sign-in
to your first configuration change and your first event in a few minutes.
{% endhint %}

## What you will find here

| Section | What it covers |
|---|---|
| [Getting started](getting-started.md) | What is in the package and your first steps |
| [Feature overview](features.md) | Everything Morpheus Emulator delivers |
| [Changelog](CHANGELOG.md) | Main customs, features and the version history |
| [Dashboard](dashboard/README.md) | How to use every screen of the control panel and its visual editors |
| [MuServer](muserver/README.md) | How to configure every server, every file and every setting, with syntax and examples |
| [Events](muserver/events/README.md) | All classic and custom events: enabling, scheduling and rewards |
| [Customs](muserver/customs/README.md) | Every custom system and how to configure it |
| [Client customs](muserver/client/README.md) | Client-side customs, managed in the Client Hub and published to players |
| [Troubleshooting](troubleshooting.md) | Common problems and how to fix them |

## How to read the configuration pages

Every configuration page follows the same structure:

1. **What it does** — the feature from the player's point of view.
2. **Where it lives** — the file and the switch that enables it.
3. **Syntax** — every column or key, with its values and meaning.
4. **Example** — real lines taken from the shipped files.
5. **Pitfalls** — the mistakes that make the server ignore a file or behave unexpectedly.
6. **Reload** — how to apply the change without restarting the server.

{% hint style="warning" %}
Before editing any server file, read [File format basics](muserver/file-format-basics.md).
A missing `end` line or a wrong number of columns is the most common reason a change "does nothing".
{% endhint %}

## Quick links

- [Hot reload: which module reloads which file](dashboard/services-and-reload.md)
- [GameServerInfo - Common (main server settings)](muserver/gameserver/common.md)
- [EventItemBag — boxes, boss drops and event rewards](muserver/data/event-item-bag.md)
- [Chat commands](muserver/data/commands.md)
- [Visual editors in the panel](dashboard/editors/README.md)
