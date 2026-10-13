# AI assistant

The **AI assistant** answers questions about your MuServer files and settings in plain language. It reads your server's files to explain what a setting does, where something is configured, and how to change it — and you stay in control of every change.

## Who can use it

The assistant appears only when **all** of these are true:

1. The assistant is enabled for the platform.
2. Your account has the **AI assistant** option turned on (support enables it for your account).

If any of these is missing, the button simply doesn't show. Ask support if you'd like access.

## Opening the assistant

1. Open any page of your server.
2. Click the floating **AI assistant** button in the bottom-right corner.
3. The panel opens on the right with the title **AI assistant** — *Questions and configuration for the MuServer files*.
4. Type your question in **Ask about a file or a setting…** and click **Send** (or press Enter).

When you have a file open in the [File editor](file-editor.md), the assistant knows it: the line **Context: &lt;file&gt;** appears above the input, and questions like "what does this line do?" refer to that file.

## What you can ask

The assistant is best at explaining and locating configuration. Examples:

- "Where do I change the experience rate?"
- "What does `MaxLevelCharacter` do and what is a safe value?"
- "Which file controls the Blood Castle schedule?"
- "Explain section 3 of this EventItemBag file."
- "Why would the GameServer say 'column count' on line 42 of this file?"
- "How do I make a box drop excellent items only for Dark Knights?"
- "Which reload module do I need after changing the NPC shop?"
- "What's the difference between a fixed point and an area in the spot files?"

While it works, the assistant shows what it is doing — **Reading &lt;file&gt;**, **Listing &lt;folder&gt;**, **Searching files: …**, **Searching key …**, **Checking the catalog for …** — so you can see which files the answer is based on.

{% hint style="info" %}
The assistant **only reads** files. It never saves, reloads or restarts anything. Apply its suggestions yourself in the [File editor](file-editor.md), [Configuration](configuration-forms.md) or a [visual editor](editors/README.md), where validation and backups protect you.
{% endhint %}

## Privacy

Sensitive values such as passwords, tokens, keys and IP addresses are masked before the assistant sees your files. Your conversation is kept only in your current browser session, separately for each server; it is not shared with other users.

## Controls

| Control | What it does |
|---|---|
| **Send** | Sends your question. |
| **Stop** | Stops an answer that is being generated. |
| **New chat** | Clears the conversation and starts again. |
| **Close** (✕) | Hides the panel; the conversation is kept while you navigate the same server. |

## Messages you may see

| Message | Meaning |
|---|---|
| **Thinking…** | The answer is being prepared. |
| **Too many questions in a row. Wait a few minutes.** | Usage limit reached; try again shortly. |
| **The assistant is busy. Try again in a moment.** | Many people are using it right now. |
| **The answer took too long. Try a more specific question.** | Narrow the question (mention the file or setting). |
| **The AI model is not available right now. Let the administrator know.** | Temporarily unavailable. |
| **The AI assistant is turned off.** | The assistant is not enabled for your account. |

## Tips for good answers

- Mention the **file name** or **setting name** when you know it.
- Open the file first so the assistant gets the context automatically.
- Ask one thing at a time.
- Always double-check values before saving — the assistant can make mistakes, and the editor's validation is the final word.
