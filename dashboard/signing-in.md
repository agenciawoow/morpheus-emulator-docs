# Signing in

This page explains how to sign in to the dashboard, how the two-factor code step works, and how to change the panel language.

## Signing in step by step

1. Open the dashboard address you received from your reseller or from support.
2. In **Username or e-mail**, type your login.
3. In **Password**, type your password.
4. Click **Sign in**. The button changes to **Signing in…** while the panel checks your credentials.
5. If your account has two-factor authentication (2FA) turned on, a new field appears: **Authenticator code**. Open Google Authenticator (or your equivalent app), type the current 6-digit code and click **Sign in** again.
6. You land on the server list, or directly on your server's dashboard if you only have one.

{% hint style="info" %}
The login page may be protected by reCAPTCHA. It is invisible — there is no checkbox to tick. If you see **reCAPTCHA is still loading — wait a second and try again.**, just wait a moment and click **Sign in** again.
{% endhint %}

## Two-factor authentication at login

When 2FA is active on your account, signing in always takes two steps: password first, then the code.

| Message | Meaning | What to do |
|---|---|---|
| **Authenticator code** field appears | Your password is correct; the panel now needs the code | Type the 6 digits shown in your app |
| **Invalid code. Try again.** | The code was wrong or expired | Codes change every 30 seconds. Wait for a fresh code and type it quickly |
| **Authenticator code invalid or expired.** | Same as above | Check that your phone's clock is set to automatic time |

{% hint style="warning" %}
**Lost your phone or reinstalled the authenticator app?** You can't remove 2FA yourself without a valid code. Ask your administrator or reseller to **reset 2FA** on your account. After the reset you sign in with just the password and can enable 2FA again from your profile.
{% endhint %}

To turn 2FA on or off, see [Account and security](account-and-security.md#two-factor-authentication).

## Changing the language

You can change the language at any time, before or after signing in:

1. Click the language button in the top bar (it shows the current language, for example `EN`).
2. Pick one of: **Português (Brasil)**, **English**, **Español**, **Tiếng Việt**, **简体中文**.

The whole panel switches immediately. When you are signed in, the choice is also saved to your account, so the next login opens in the same language on any computer.

{% hint style="info" %}
If you see **Could not save the language to your account. The change applies to this browser only.**, the language was changed for this browser but not stored in your account. Try again later.
{% endhint %}

On the very first visit (before any choice), the panel follows your browser's language.

## Signing out

Open the account menu (your e-mail in the top-right corner) and click **Log out**. The label changes to **Signing out…** and you return to the login page.

{% hint style="success" %}
Always sign out on shared or public computers. Your session also expires automatically after a period of inactivity.
{% endhint %}

## Troubleshooting

| Problem | Solution |
|---|---|
| **E-mail and password are required.** | Fill in both fields. |
| **Captcha invalid or expired.** / **Confirm the captcha to continue.** | Reload the login page and try again. Disable aggressive ad/script blockers for the panel address. |
| I forgot my password | Ask your administrator or reseller to set a new password for your account. |
| After login I see **No server available** | Your account has no server assigned yet. Contact your reseller or support. |
