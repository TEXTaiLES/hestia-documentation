# Admin & Editor Users

The Portal supports several tiers of authenticated access. Roles are defined in Directus, and the Portal reads them on every request to decide what a user is allowed to see and do.

## Role Overview

| Role | Sign-in required | Extra capabilities |
|------|------------------|--------------------|
| **Public** (signed-in) | Yes | Browse Collections; open published artefacts; use *Annotate with THOTH* |
| **Editor** | Yes | All of the above **+** add new artefacts through the Portal |
| **Admin** | Yes | All of the above **+** see and open **unpublished** artefacts (drafts) |

*Public **unsigned** access is limited to the Home and Toolbox pages. Collections and artefact pages redirect unauthenticated visitors to the login page — see [Public User](publicUser.md) for details.*

## How to Sign In

Any of the following authenticate a user against Directus:

- **Email + password** — the classic form on the Portal Login page.
- **EGI Check-In** — the *Login via EGI* button on the same page, described in [EGI Check-In Login](../integration/egi-login.md). The EGI account must be registered in Directus with the same email address.

Sessions are shared across the whole `*.textailes.athenarc.gr` domain, so a single sign-in also grants access to any tool that uses the [Cookie-Based Authentication](../integration/directus-cookie.md) flow.

## Editor Capabilities

Editors are users whose Directus role grants **create** permission on the *artefacts* collection.

- On the Collections page, an **Add New Artefact** button appears in the top-right corner.
- Clicking it opens the [Add New Artefact form](../manual/add-artefact.md), where a full metadata record can be created and 3D / OBJ / thumbnail files can be uploaded.
- Newly created artefacts are stored as **drafts** (unpublished) until an admin promotes them.

## Admin Capabilities

Admins have unrestricted access. On top of everything an editor can do, they also:

- **See unpublished artefacts** in the Collections gallery, so drafts can be reviewed before publication.
- **Open any artefact page** directly, regardless of its published state.
- Control the *published* flag on individual artefacts from Directus.

## Annotating with THOTH

Any signed-in user can annotate a 3D artefact:

1. Open an artefact from the [Collections](../manual/collections.md) page.
2. Click **Annotate with THOTH** next to the 3D viewer.
3. The Portal prepares (or reuses) a THOTH scene tied to that artefact and opens the annotator in a new tab, already loaded with the model.
4. Annotations are saved back to the archive database automatically.
