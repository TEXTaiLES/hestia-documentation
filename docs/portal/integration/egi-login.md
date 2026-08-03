# EGI Check-In Login

Besides the classic email + password form, the Portal offers **Single Sign-On via [EGI Check-In](https://aai.egi.eu/)** — the federated authentication service of the European Grid Infrastructure. Users who already have an EGI, ORCID, institutional or social identity linked to EGI can sign in to the archive without creating a separate password.

## Where to Find It

On the Portal Login page a horizontal separator (**OR**) sits between the email/password form and the EGI button:

> **Login via EGI** — a button styled as an outlined key icon; clicking it starts the EGI sign-in flow.

The EGI option appears everywhere the login page is shown — for example, when an unauthenticated user tries to open [Collections](../manual/collections.md) or an [Artefact page](../manual/artefact.md).

## What Happens When You Click "Login via EGI"

1. The Portal redirects the browser to the **EGI authorization page**.
2. The user picks an identity provider (EGI account, ORCID, institutional login, etc.) and authenticates there.
3. EGI redirects back to the Portal with a short-lived authorization code.
4. The Portal exchanges that code for an EGI access token and asks EGI for the user's email.
5. The Portal looks that email up in Directus:
    - If a user with that email **exists and is active**, a Portal session is created and the browser is redirected to the page the user originally wanted to visit.
    - If the user **does not exist** or is **inactive**, the flow ends on the archive page with a message code such as `EGI_USER_NOT_REGISTERED` or `EGI_USER_INACTIVE`.

The whole round-trip is designed to feel like a normal login: the user sees the EGI screen, then lands back on the page they were trying to reach.

## Prerequisites

To sign in via EGI a user needs:

- A working EGI Check-In identity (EGI, ORCID, institutional login, …).
- A Directus user account **registered with the same email address** returned by EGI. Contact a portal administrator to have your email added if it is not present yet.

## Session Behaviour

Signing in via EGI creates the **same kind of Portal session** as an email/password login. That means:

- The session is stored in a cookie shared across the whole `*.textailes.athenarc.gr` domain.
- Any TEXTaiLES tool that uses the [Cookie-Based Authentication](directus-cookie.md) flow will pick the session up automatically — no second login prompt.
- The session lasts 7 days, after which the user will be asked to sign in again.

## Signing Out

The regular logout action (in the user menu of the top navigation bar) clears the Portal session cookie. It does **not** sign the user out of EGI itself; that can be done from EGI's own account page.

## Common Error Messages

When something goes wrong, the Portal redirects with a `?reason=EGI_*` query parameter. The most common ones are:

| Reason | Meaning |
|--------|---------|
| `EGI_LOGIN_INIT_FAILED` | The Portal could not build the request to EGI. Usually a server-side configuration issue. |
| `EGI_USER_NOT_REGISTERED` | Authentication at EGI succeeded, but no Directus user exists with that email. |
| `EGI_USER_INACTIVE` | The Directus user exists but is disabled. |
| `EGI_STATE_MISMATCH` | Security check failed on the callback (possible cross-site or expired session). Try again. |
| `EGI_MISSING_PARAMS` | The callback was missing expected parameters. Restart the login. |
| `EGI_CALLBACK_ERROR` | An unexpected error occurred while finalising the sign-in. |

If you keep hitting the same error, contact a portal administrator with the reason code shown in the URL.
