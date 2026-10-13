# API tokens

The **API** page lets you generate **tokens** for your own automations and integrations — for example a script that restarts a service on a schedule, or a website that reads server status. Each token acts on your behalf with the same permissions as your account.

{% hint style="danger" %}
**Treat every token like a password.** Anyone who has the token can do what your account can do. Never paste a token in a chat, a screenshot or a public repository.
{% endhint %}

{% hint style="info" %}
Available to accounts allowed to manage tokens (roles `admin` and `reseller`). If you don't see **API** in the sidebar, your role doesn't include it — see [Roles and permissions](roles-and-permissions.md).
{% endhint %}

## Generating a token

1. Open **API** in the sidebar.
2. In **Generate a new token**, type a **Name** that tells you where it's used (e.g. `ci-deploy`, `mobile-integration`).
3. Click **Generate**.
4. The box **Token created — copy it now** shows the full value. Click **Copy** and store it in your password manager or in your application's configuration.
5. Click **Close**.

{% hint style="warning" %}
The full token is shown **only once**. If you lose it, revoke it and generate a new one.
{% endhint %}

## Active tokens

**Active tokens (N)** lists every token of your account:

| Information | Meaning |
|---|---|
| Name | The name you gave it. |
| **prefix** | The first characters of the token (`a1b2c3…`), so you can tell tokens apart without exposing them. |
| **used &lt;date&gt;** / **never used** | Last time the token was used. |
| **created &lt;date&gt;** | Creation date. |

## Revoking a token

1. Click **revoke** next to the token.
2. Confirm **Revoke token?** — *Whoever uses this token loses access immediately.*

Revoke tokens that are **never used**, that belonged to a former staff member's integration, or that you suspect have leaked.

## Using a token

The **How to use it** box on the page shows exactly how to send the token with each request (as a request header). Give that information to the developer of your integration.

## Good practices

- One token per integration, with a clear name — so you can revoke one without breaking the others.
- Review **Active tokens** regularly and revoke the unused ones.
- Token creation and revocation are recorded in [Logs and audit](logs-and-audit.md).
