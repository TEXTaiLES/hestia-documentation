# Collections

The Collections page is the entry point to the digital archive of artefacts.

## Login Required

Collections require a signed-in session. Users who are not authenticated are redirected to the [Login page](../integration/directus-login.md), where they can sign in with a Directus email/password or via [EGI Check-In](../integration/egi-login.md). After a successful login they land back on the Collections page.

## Use Cases Grid

The landing view of the page is a grid of **8 use case cards**, each representing a different archaeological collection. Every card shows:

- A preview image of the collection
- The name of the use case
- The number of artefacts currently in that use case

The 8 use cases are:

1. Greek Ancient Textiles
2. Textile Collection from Pompeii
3. Greek Bronze Age clay sealings
4. Imprints on human plaster casts from Pompeii
5. Benaki Museum collection
6. Turku Cathedral Museum collection
7. Opera Theatre Archive in Rome
8. Textile Museum St. Gallen collection

An additional **All Use Cases** card lists every artefact regardless of collection.

## Browsing Artefacts

Selecting a use case card opens the gallery view for that collection. Each artefact is presented as a card showing:

- Its **thumbnail** (or a "No thumbnail" placeholder)
- The **title** of the artefact
- The **use case number**
- The **collection** and, when available, the **time period**

Clicking a card opens the [Artefact detail page](artefact.md).

A **Back to Collections** link returns to the use case grid.

## Visibility of Unpublished Artefacts

Each artefact has a **published** flag. Public and editor users only see artefacts that have been published. **Admin** users see every artefact — including drafts — so they can review work in progress before publishing.

## Add New Artefact (Editors and Admins)

Users whose Directus role grants **create** permission on the *artefacts* collection see an **Add New Artefact** button in the top-right of the Collections page. It opens the [Add Artefact form](add-artefact.md).
