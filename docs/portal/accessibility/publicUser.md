# Public User

The Portal is a public web page and its **Home** and **Toolbox** sections can be visited by anyone, with no account required. To browse the digital archive itself, however, a signed-in session is required.

## What Public (Unsigned) Visitors Can Do

Without logging in, any visitor can:

- Open the [Home page](../manual/home.md) and read about the TEXTaiLES project.
- View the live artefact / use-case / tool counts in the statistics bar.
- Open the [Toolbox](../manual/toolbox.md) and follow links to each tool's documentation, GitHub repository and — where available — its live server.

## What Requires a Sign-in

The [Collections](../manual/collections.md) gallery and the individual [Artefact pages](../manual/artefact.md) are gated. A visitor who clicks *Collections* while unauthenticated is redirected to the Portal Login page, where they can:

- Sign in with a **Directus email + password** account, or
- Sign in via **EGI Check-In** — see [EGI Check-In Login](../integration/egi-login.md).

After a successful login the visitor is sent back to the page they originally requested.

## What Signed-in Public Users Can Do

Once signed in, a public user (without editor or admin privileges) can:

1. Browse all **8 use cases** and their published artefacts.
2. Open any published artefact to explore its full metadata.
3. Interact with the **3D model** through the built-in viewer — rotate, zoom, pan.
4. When no 3D model is available, browse the **robot scan images** captured for that artefact.
5. Use the **Annotate with THOTH** button to open the annotator on that artefact.

Public users do **not** see unpublished (draft) artefacts and do **not** see the *Add New Artefact* button. Those capabilities are described in [Admin & Editor Users](adminUser.md).
