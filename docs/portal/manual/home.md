# Home

The Home page is the landing page of the Portal. It introduces visitors to the TEXTaiLES project and gives quick access to the two main sections of the archive.

## Overview

The page opens with a **hero banner** ("Digital TEXTaiLES Archive — Preserving Cultural Heritage Through 3D Digitization") followed by a short *About TEXTaiLES* section describing the project's mission.

## Feature Cards

Two large cards act as the main entry points into the archive:

### Explore Collections

Takes the user to the **Collections** page, where all digitized artefacts are grouped into use cases (archaeological collections). Browsing collections requires an authenticated session — see [Public User](../accessibility/publicUser.md) for details.

### Digital Toolbox

Takes the user to the **Toolbox** page, which lists every tool of the TEXTaiLES suite (robotic imaging, 3D reconstruction, annotation, AI-powered detection/segmentation/classification). Each tool exposes links to its documentation, GitHub repository and, where available, a live server.

## Statistics Bar

At the bottom of the page a live stats bar reports:

- **Digitized Artefacts** — the total number is read from the database on every page load, so it is always up to date.
- **Use Cases** — 8 archaeological collections currently represented in the archive.
- **Our Tools** — 8 tools available in the toolbox.

## Navigation

The top navigation bar is shown on every page and gives access to **Home**, **Collections** and **Toolbox**. When a user is signed in, the navbar also exposes the user menu; otherwise it shows a **Login** entry.
