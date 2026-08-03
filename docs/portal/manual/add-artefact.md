# Add a New Artefact

Users whose Directus role grants **create** permission on the *artefacts* collection can add new artefacts directly from the Portal, without going through the Directus admin app.

## Where to Find It

On the [Collections](collections.md) page, an **Add New Artefact** button appears in the top-right of the gallery for authorised users. Clicking it opens the *Add New Artefact* form.

Users without the required permission never see the button, and requesting the form URL directly returns a *401 Unauthorised* page.

## The Form

The form is organised in cards, each grouping related fields. Only the **Title** field is required; every other field is optional and can be filled later by editing the artefact in Directus.

### Heritage Asset Information

Title (required), description, date/timespan, dimensions, owner, category of textile, keywords (comma-separated), inventory number and origin.

### Digital Asset Information

- **GLTF file** — upload a `.gltf` or `.glb` model.
- **OBJ file** — upload a `.obj` model.
- **Robot Scan ID** — paste the `scan_id` UUID of an existing robot image batch to link the artefact to its captured images.
- **Thumbnail** — upload a `.png`, `.jpg` or `.jpeg` preview image used on the Collections gallery.
- Digitization methods, digitization actor and resolution.

Each file input uses a custom **Browse…** control that shows the chosen filename next to it.

### Identification, Condition, Preventive & Interventive Conservation

Mirror the corresponding read-only sections on the [Artefact page](artefact.md) — accession number, reference, material analyzed, object status, temperature/humidity, mount, conservation date (with a date picker), cleaning yes/no, foreign materials, and so on.

### Additional Information

Use case, collection, time period, creator, sensor, location and source — the same "legacy" fields shown at the bottom of the artefact detail page.

## Saving

Clicking **Save Artefact** submits the form:

1. Any uploaded files (GLTF, OBJ, thumbnail) are stored in Directus and linked to the new artefact record.
2. On success, a green confirmation banner appears and the browser is redirected to the newly created artefact's detail page.
3. On failure (validation error, upload failure, missing permission), a red banner shows the error message and the form remains editable.

A **Cancel** link at the bottom returns to Collections without saving.

## After Creation

New artefacts are created as **drafts** — they will only be visible to public users once an admin toggles the *published* flag on the record.
