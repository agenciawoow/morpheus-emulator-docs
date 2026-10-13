# UI Editor

**Customize and preview the client screens — login, character select, shop and more — on PC and mobile.** The UI Editor lets you change colours, images, videos, texts and layout of the client's web-based screens, see the result live, and publish it to players.

Open **Client Hub → UI Editor**.

{% hint style="info" %}
If you see **Client screens not found on the server**, the screen files are not installed on your server yet. Contact support.
{% endhint %}

## Screens you can customise

| Screen | Description |
|---|---|
| **Launcher** | Update screen and Play button, before the game opens. |
| **Loading** | Progress bar while the game loads (one page on PC, another on mobile). |
| **Login** | Server list, login and sign-up. |
| **Character select** | Account characters, create and delete. Sits on top of the game 3D scene. |
| **Shop (Morpheus Shop)** | In-game item shop. |
| **Events** | Event and boss schedule. |
| **My account** | Account security: password, PID, lock and last login. |
| **Mobile HUD** | Touch interface on mobile: bars, buttons, joystick and windows. |

Pick a screen in the list. Badges show **unsaved** (you have edits) or **customized** (the screen differs from the original).

## Appearance tab

The easiest way to restyle a screen. Use **Search color or image…** to find what you want.

### Theme colors and sizes

*Theme variables of the screen. Changing here changes every place that uses the variable, and only the value changes in the file.*

1. Click a colour swatch (**Pick color**).
2. Choose the colour and **Opacity**.
3. Watch the preview update.

Variables marked **shared with other screens** change all screens that use them. If a screen has no theme variables, change colours in the **Code** tab.

### Images and videos

1. Find the image or video in the list.
2. Click **Replace** and choose a new file **of the same format** (WEBP for WEBP, PNG for PNG, MP4 for MP4…), ideally the same size.
3. The file is uploaded to the server (**Image replaced — waiting to be published.**). The image shows **Customized image**.
4. To go back, click **Restore** (**Restore the original image?**).

Limits: images up to **5 MB**, videos up to **25 MB**. A wrong format is refused: *Different format: this image is WEBP, the uploaded file is PNG. Convert it to WEBP before uploading.*

{% hint style="info" %}
Screen texts are changed in the **Code** tab. Translations are tied to the exact Portuguese text — if you change a text, update its translation too.
{% endhint %}

## Code tab

Edit the HTML, CSS and script files of the screen directly.

- Files marked **Shared: also used by other screens.** show a warning: the change applies to all of them.
- **Compare with original** shows original (left) and current (right) side by side; you can keep editing on the right. **Close comparison** to return.
- **Restore original** brings the file back to how it was before the first change made through the panel.
- **Full screen** / **Exit full screen**.
- Pasted invisible characters are removed automatically.

### Problems that prevent saving

The editor checks the files so a broken screen never reaches players. Errors are listed under **Problems that prevent saving**, for example:

| Problem | Meaning |
|---|---|
| **The "div" tag was opened and not closed.** | Broken HTML structure. |
| **HTML comment opened and never closed — the rest of the page disappears.** | Close the comment. |
| **Unclosed quoted text in the script — the whole screen stops working.** | Close the quote. |
| **"}" does not match the "(" opened on line 12.** | Brackets don't match. |
| **The game calls the function X in Y. Without it that part of the screen stops working — keep the function (you can change what it does inside).** | Don't delete functions the game uses. |
| **&lt;file&gt; does not exist on the server — the image/file will not show.** | A referenced file is missing. |
| **The text has the � character — a sign of corrupted text.** | Retype the accented text. |

Problems that were already in the file are marked **(already in the file — does not block saving)**.

## Preview

The preview runs the same files as the game, with sample data instead of your real server data. Edits show here **before saving**.

| Control | Options |
|---|---|
| Mode | **Desktop**, **Mobile** or **Side by side**. Some screens exist only on one platform (**This screen only exists on mobile.**). |
| Screen size | Desktop: **Full HD**, **Laptop**, **HD**. Mobile: **Android**, **iPhone**, **Tablet**. |
| **Preview language** | Portuguese, English, Spanish, Chinese, Korean, Vietnamese, Thai. |
| Background | **Background: game scene**, **Background: checkerboard (transparency)**, **Background: black**. On screens over the 3D scene you see *In the game, the 3D scene shows here*. |
| **Reload the preview** | Restarts the preview. |

### Preview console

Below the preview, the **Preview console** shows clicks and actions on the screen (**sent to game:** what the button would send to the game), script errors, missing files (**file not found:**) and external resources blocked in the preview (**external, blocked in the preview (loads normally in the game):**). Use **Clear** to empty it.

## Saving and publishing

1. Click **Save** (`Save (3 files)` when several files changed). If there are errors: *Fix the marked errors before saving — as it is, the screen would break in the game.*
2. Message: *3 files saved. Players get the change on the next Client Hub publish.* Click **Go to publish**.
3. Publish from [Publishing updates](publishing-updates.md).

**Reload from server** re-reads the files (discarding unsaved edits after **Discard unsaved changes?**). If a file was changed on the server meanwhile, choose **Reload** or **Keep mine** — see [Validation and conflicts](../validation-and-conflicts.md#in-the-ui-editor).

## Tips

- Start with the **Appearance** tab; use **Code** only for layout or text changes.
- Test both **Desktop** and **Mobile** before publishing.
- Keep image sizes small: big images slow down the launcher download for players.
