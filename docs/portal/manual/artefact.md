# Artefact Page

The Artefact page is the detailed view of a single artefact, opened from any card in the [Collections](collections.md) gallery.

The page has three main areas:

1. A **3D viewer** (or a robot-scan image gallery if no 3D model is available)
2. Action buttons (currently: **Annotate with THOTH**)
3. Full **metadata** organised in themed sections

## 3D Model Viewer

When the artefact has an attached 3D asset, it is displayed with the [`<model-viewer>`](https://modelviewer.dev/) component. Users can:

- **Rotate** the model with left-click / touch drag
- **Zoom** with scroll / pinch
- **Pan** with right-click / two-finger drag

The viewer supports GLB / glTF files as well as OBJ models (including multi-file OBJs with linked material and texture files).

If the artefact has **no 3D model** but was captured by the robotic imaging system, the page shows a gallery of the corresponding **robot scan images** instead, sorted chronologically.

## Annotate with THOTH

Signed-in users see an **Annotate with THOTH** button next to the viewer. Clicking it:

1. Prepares (or retrieves) a THOTH scene for the artefact.
2. Opens the [THOTH annotator](https://thoth.textailes.athenarc.gr) in a new tab, already loaded with the artefact's 3D model.
3. Any annotations saved in THOTH are automatically written back to the archive database.

While the scene is being prepared, the button shows a loading spinner.

## Metadata Sections

Artefact metadata is organised in themed panels. The first two are always visible; the rest are grouped under a collapsible **Additional Information** panel that expands on click.

### Heritage Asset (always visible)

Title, description, date/timespan, dimensions, owner, category of textile, keywords, inventory number and origin.

### Digital Asset (always visible)

Digitization methods, digitization actor and resolution.

### Additional Information (collapsible)

Expanding this panel reveals three deeply detailed sections:

- **Conservation** — identification, condition assessment, preventive conservation (temperature, humidity, container, mount, result link) and interventive conservation history.
- **Analysis** — identification, non-destructive analysis (method, instrument, aim, result) and destructive analysis (sample data, method, instrument, result).
- **Documentation** — identification, provenance, group/subgroup, publication references, and a **Technological Analysis** subsection covering:
    - **Primary Structure** — material, type of weave, weave count and paired **Thread A / Thread B** data (diameter, ply, twist, angle, method).
    - **Decoration** — type, specifics, material and decoration thread details.

Below these, a **Legacy Additional Information** block preserves fields from the original schema: use case, collection, time period, creator, sensor and location.

## Draft vs Published

Every artefact has a **published** flag. Non-admin users only reach published artefacts; requesting an unpublished artefact returns a *not found* response. **Admin** users can open any artefact regardless of state, so they can review drafts before making them public.
