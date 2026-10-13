# Roles and permissions

Every account has exactly one **role**. The role decides which menus you see and what you can do. Your role is shown in the sidebar (**Role**) and on your profile (**Role:** · *permissions and menus come from this role — to change it, talk to an admin.*).

## The four roles

| Role | Typical user | In one sentence |
|---|---|---|
| `admin` | Platform administrators | **full access** — every feature, every server. |
| `reseller` | Official resellers | **manages clients/servers** — creates customer accounts and servers, and operates their customers' servers. |
| `client` | Server owner | **only its own server** — operates and configures the server it owns. |
| `operator` | Server owner's technical staff | **only start/stop/restart** — controls services and watches the dashboard, without seeing files or licenses. |

## Permission matrix

| Permission | What it unlocks | admin | reseller | client | operator |
|---|---|:-:|:-:|:-:|:-:|
| View dashboard | **Dashboard**: metrics, services table, events, live console | ✅ | ✅ | ✅ | ✅ |
| Control services | Start / Stop / Restart / Reload, **Start all** / **Stop all**, GameServer actions, event start, reload buttons in editors | ✅ | ✅ | ✅ | ✅ |
| Read files | **Files**, **Configuration**, **Editors**, **Client Hub** (read) | ✅ | ✅ | ✅ | ❌ |
| Write files | Save files, backups and restores, publish client updates | ✅ | ✅ | ✅ | ❌ |
| Read logs | **Logs & Audit**, **Server logs** | ✅ | ✅ | ✅ | ❌ |
| View licenses | **Licenses** page, GameServer stats card on the dashboard | ✅ | ✅ | ✅ | ❌ |
| Manage API tokens | **API** page | ✅ | ✅ | ❌ | ❌ |
| Manage users | Create and edit customer accounts | ✅ | ✅ | ❌ | ❌ |
| Manage servers | Register and edit servers | ✅ | ✅ | ❌ | ❌ |
| AI assistant | **AI assistant** button | ✅ | when enabled for the user | when enabled for the user | ❌ |
| Admin Center | Platform-wide administration | ✅ | – | ❌ | ❌ |

{% hint style="info" %}
**Morpheus Sender**, **Edit profile** and two-factor authentication are available to everyone.
{% endhint %}

## What each role sees in the sidebar

| Menu item | admin | reseller | client | operator |
|---|:-:|:-:|:-:|:-:|
| **Dashboard** | ✅ | ✅ | ✅ | ✅ |
| **Files** | ✅ | ✅ | ✅ | – |
| **Configuration** | ✅ | ✅ | ✅ | – |
| **Licenses** | ✅ | ✅ | ✅ | – |
| **Client Hub** | ✅ | ✅ | ✅ | – |
| **Editors** | ✅ | ✅ | ✅ | – |
| **API** | ✅ | ✅ | – | – |
| **Logs & Audit** | ✅ | ✅ | ✅ | – |
| **Server logs** | ✅ | ✅ | ✅ | – |
| **Morpheus Sender** | ✅ | ✅ | ✅ | ✅ |

Items you can't use are hidden, not just disabled. If you open a page you can't use (for example from an old bookmark), the panel refuses the action.

## Which servers you see

- **client** and **operator** accounts see only the server(s) assigned to them. With a single server, the panel opens it directly after login.
- **reseller** accounts see the servers of their customers, and always start at the server list.
- **admin** accounts see every server.

## Choosing roles for your team

| Person | Suggested role |
|---|---|
| You, the server owner | `client` |
| A trusted co-admin who edits configuration | `client` (ask your reseller to create a separate account) |
| A staff member who only restarts the server at night | `operator` |
| A developer integrating a website | `client` account plus an [API token](api-tokens.md) created by a role that can manage tokens |

{% hint style="success" %}
Follow the principle of least privilege: give each person the smallest role that lets them do their job, and their **own** account so the [audit log](logs-and-audit.md) shows who did what. Enable [two-factor authentication](account-and-security.md#two-factor-authentication) on every account that can edit files.
{% endhint %}

## Changing a role

Roles are changed by an administrator or by your reseller. You can't change your own role.
