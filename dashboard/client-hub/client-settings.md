# Client settings

**Client settings** is the editor for your client's main configuration: the **client settings** (feature switches, interface, menus, mobile, Discord), the **connection settings** (license, server address, client version) and the **client custom files**. You change settings in forms, save, and **Publish update** builds the client configuration and puts it in the publish queue.

Open **Client Hub → Client settings**.

For the meaning of each setting, see the MuServer reference: [Client-side customs](../../muserver/client/README.md) and the [Client settings reference](../../muserver/client/client-settings-reference.md).

## Page layout

- **Tabs**: **Connection**, **Interface**, **Custom**, **Mobile**, **Discord**, plus **Custom loaders** and **Morpheus Shop (server)**.
- **Search box**: *Search option: name, key or initials (e.g. "rus" = RankUserSwitch)*. Results are shown across all tabs (`3 options found across all tabs`) with **open tab** links.
- **Action buttons**: **Reload settings**, **Save**, **Publish update**.
- Badges: **unsaved changes** and `N errors`.

## Tabs and groups

| Tab | Groups |
|---|---|
| **Connection** | **Connection**: the connection settings – license ID, customer name, IP address and port, client version and serial. |
| **Interface** | **Screen and interface**, **Inventory and warehouse**, **Client**, **Client features**, **Plugins**, **Maximum attack speed (per class)**, **Scenes, logo and mixes**. |
| **Custom** | **User rank**, **Custom menu**, **Windows**, **Personal store**, **Offline MU Helper**, **Extra warehouses per account type**, feature switches (Morpheus Shop, Battle Pass, map markers…), **Social login**. |
| **Mobile** | **Mobile interface (Android / iOS / web)**, mobile buttons, **WebView windows**. |
| **Discord** | **Discord**, **Discord application**, **Buttons (up to 2)**. |
| **Custom loaders** | The client custom files and client effect files by category (Items, Worlds/Others, Wings/Pets, Effects, Renders), edited as tables or text. See [Client custom files](../../muserver/client/custom-files.md). |
| **Morpheus Shop (server)** | The server shop catalog (prices, options, limits…) – the [Morpheus Shop editor](../editors/morpheus-shop.md) is the friendlier way to edit it. |

Hover a field to see its hint. Some examples:

| Field | Note |
|---|---|
| **Customer name (license)** | Unique server name tied to your client build. **Do NOT change it after distributing the client: old clients stop connecting.** |
| **License ID** | Must be identical to the one in the server configuration, otherwise the GameServer disconnects the player. |
| Window title format | Template for the client window title with variables (character name, level, resets, Zen, coins…). The editor lists the **Available variables** and **Examples**. |
| **Maximum attack speed (per class)** | Attack/magic speed cap in the client. Up to 65535 it is the cap itself; above 65535 it enables the class fast-attack timing. |
| **Extra warehouses per account type** | Only tells the client how many to draw. Keep it equal to the server command settings. |
| **Buttons (up to 2)** | An enabled Discord button needs both text and an `https://` link. |
| **WebView windows** | Checked = new WebView panel; unchecked = the usual native window. |

{% hint style="warning" %}
**Feature switches need both sides.** Options like `CustomMorpheusShopSwitch` or `CustomBattlePassSwitch` must be on here **and** in the server's `GameServerInfo - Custom.dat`. With only one side on, the feature doesn't appear.
{% endhint %}

## Validation

Invalid fields are marked in red with a message such as **Enter a whole number.**, **Out of range: 0 to 255.**, **Required.**, **Must have exactly 10 characters (has 8).**, **Too long: 40 bytes, the limit is 32. Accented letters count 2.**, **Use the x.xx.xx format (e.g. 1.04.05).** While errors exist, **Save** and **Publish update** are blocked (*Fix the 2 fields marked in red before saving.*). The editor jumps to the tab with the first error.

## Saving

Click **Save**. A confirmation message appears. Saving only stores the settings on your server — players don't see anything yet.

## Publishing the configuration

1. Click **Publish update**.
2. Confirm **Prepare a client configuration update?** with **Prepare**. If you had unsaved changes they are saved first.
3. The panel builds the client configuration. The **Update prepared** box shows the size of each generated file and the main executable check.
4. Open **Publish updates** (upload icon in the top bar) and publish — the configuration goes out together with any other pending file. See [Publishing updates](publishing-updates.md).

{% hint style="info" %}
If you see **main.exe self-check disabled**, the client executable was not found on the server, so the client can't verify its own integrity. Everything else works; contact support to enable it.
{% endhint %}

## Reloading and conflicts

- **Reload settings** reads the saved settings again and shows any manual changes (*Settings reloaded: the files had changes and the values were updated.* or *…no changes in the files.*). With unsaved changes you're asked **Reload and discard changes?** first.
- If the settings were changed on the server while you were editing, a banner offers the **reload** option (discard your edits) or **Overwrite with mine**. See [Validation and conflicts](../validation-and-conflicts.md#in-client-settings).

{% hint style="danger" %}
**Client configuration incomplete — Publish Update will not work.** A component needed to build the client configuration is missing on your server. Contact support.
{% endhint %}
