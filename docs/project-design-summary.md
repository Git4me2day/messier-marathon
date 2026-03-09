# Messier Marathon Observer's Log — Project Design Summary

---

## Mission Statement

The Messier Marathon Observer's Log is a self-contained, offline-capable web tool built for amateur astronomers pursuing the Astronomical League Messier Observing Program. Its purpose is to replace paper log sheets with a clean, distraction-free digital record that works in the field — including at dark sites with no internet connection — without requiring any app install, account, or subscription.

The guiding principle is **utility without compromise on atmosphere**. The interface is designed to feel like a well-made star atlas: precise, purposeful, and visually at home under a red flashlight. Every design decision, from the night-vision colour palette to the monospaced coordinate columns, serves the observer first.

---

## Development History

### Foundation
The project began as a single HTML file containing all 110 Messier objects in marathon observation order, sourced from the standard Astronomical League sequence used by observers at the spring new moon window. The data array stores each object's Messier number, type, constellation, magnitude, J2000.0 coordinates, and Pocket Sky Atlas page reference.

### Design System
A shared stylesheet (`mm-shared.css`) gives the project a consistent visual identity across all pages, built entirely on CSS custom properties that enable a full dark/light theme swap with a single attribute toggle on `<html>`.

Two complete themes were developed in parallel:

- **Dark / Night mode** — deep black background (`#030305`), red-spectrum text and accents for night-vision preservation, subtle starfield texture, red glow on headings.
- **Light / Day mode** — warm parchment atlas feel (`#f4ece0`), dark ink, maroon accents. Suited for planning sessions indoors.

Theme preference is saved to `localStorage` and restored on every page load without a flash of unstyled content.

Typography uses three typefaces, all self-hosted from local `fonts/` files to eliminate any dependency on Google Fonts or external CDNs:

- **Cinzel** — classical Roman capitals for headers and labels
- **Share Tech Mono** — monospaced data columns and coordinates
- **Crimson Pro** — italic body copy and prose pages

### Core Observation Log (`index.html`)
The main page provides the full 110-object table with:

- Marathon observation order (not Messier number order)
- Per-object checkbox and free-text field notes, both persisted in `localStorage`
- Automatic silent observation timestamp (ISO string) recorded when a checkbox is ticked; stored in `mm_times` and exposed only in the full record modal and CSV export
- Animated progress bar tracking objects observed out of 110
- Filter buttons: All / Galaxy / Open Cluster / Globular / Nebula / Unobserved
- Live search filtering by object number, type, or constellation
- Type colour-coded dot badges (11 types) with a collapsible legend panel
- Print stylesheet producing a clean black-on-white log sheet

### Image Lightbox
Clicking any object row opens a modal overlay with:

- Object image from local `images/M[n].jpg`
- Attribution overlay fetched from `images/M[n]-attrib.txt`
- Full object data in a right-hand sidebar
- Local finder chart button linking to `charts/M[n].png`
- Stellarium Web button (deep-linked by Messier name)
- Keyboard navigation (arrow keys, Escape), click-outside-to-dismiss, prev/next footer controls

### Log Menu
A single **☰ Log ▼** dropdown replaces discrete buttons, keeping the toolbar clean:

- ⎙ Print
- ⬇ Export CSV / ⬆ Import CSV
- ⬇ Export AL Records / ⬆ Import AL Records
- ⚙ Session Setup…
- ↺ Reset All Data…

### CSV Export / Import
Export produces a timestamped `YYYY-MM-DD-HHmmss-observing-log.csv` with all catalog fields, observed status, date observed (from the hidden timestamp), and field notes. All fields are quoted to handle embedded commas. Import parses by column header name (position-independent), detects conflicts, and presents an Overwrite / Merge choice before applying.

### AL Observation Record Modal
A small **✎** button at the right of every notes cell opens a full-screen modal with all fields required by the Astronomical League Messier Club program:

- When & Where: date, time, observing site / location
- Sky Conditions: seeing (Antoniadi 1–5), transparency (1–5), limiting magnitude
- Equipment: telescope / instrument, aperture (mm), eyepiece, magnification
- Observation: written description (required for AL certificate), sketch notes

The ✎ icon is outlined (dim) when no record exists and renders as a solid filled red button when a record has been saved, giving an at-a-glance status across the full table. Description pre-populates from the brief field note if a full record is being created for the first time.

### AL Records JSON Export / Import
Full observation records are stored in `mm_fullnotes` (localStorage) and can be backed up independently of the CSV log. Export produces a timestamped `YYYY-MM-DD-HHmmss-al-records.json` with a version field and ISO export timestamp for forward compatibility. Import merges incoming records with existing ones, showing new-vs-conflict counts before confirming.

### Session Setup
A modal accessed via the Log menu allows the observer to set common defaults once at the start of each night — location, sky conditions, and equipment — that auto-fill blank fields in every observation record opened during the session. Existing saved data is never overwritten; session values only fill gaps.

The **📍 Locate** button calls `navigator.geolocation` to get GPS coordinates from the device, then reverse-geocodes them to a human-readable place name via the OpenStreetMap Nominatim API (free, no API key required). Coordinates are stored in the session object alongside the place name. A pulsing **Session active** indicator pill appears in the toolbar while any session default is set. Session data is persisted in `mm_session` localStorage and survives page refreshes.

### Companion Pages
- **FAQ (`faq.html`)** — answers common questions about the marathon sequence, AL program requirements, how to use the log, and how localStorage works.
- **CSS Cheat Sheet (`docs/css-cheatsheet.md`)** — internal developer reference for all shared CSS classes, colour variables, layout patterns, and the theme toggle. Kept in `docs/` only; not deployed to the public site.

---

## localStorage Keys

| Key | Contents |
|-----|----------|
| `mm_checks` | Observed state per object (object keyed by M-id) |
| `mm_notes` | Brief field notes per object |
| `mm_times` | ISO observation timestamp per object |
| `mm_fullnotes` | Full AL record data per object (structured JSON) |
| `mm_session` | Session defaults: location, sky conditions, equipment, lat/lon |
| `mm_theme` | `"dark"` or `"light"` |

---

## Infrastructure

- No frameworks, no build tools, no server-side code
- Fonts served locally from `fonts/` — no external CDN calls at runtime
- Images and charts stored locally — works completely offline after first load
- GitHub Pages deployment via `.github/workflows/deploy.yml`; only public-facing files are published, developer docs and scripts are excluded
- Nominatim geolocation call is the only runtime network request (optional; gracefully skipped offline)

---

## Repository Structure (Current)

```
/
├── index.html                    ← Main observation log
├── faq.html                      ← Frequently asked questions
├── mm-shared.css                 ← Shared design system stylesheet
├── README.md                     ← GitHub repo overview and setup guide
├── create_attributions.sh        ← Generates SEDS attribution .txt files
├── generate_charts.py            ← Offline finder chart generator (matplotlib)
├── .gitignore
├── .github/
│   └── workflows/deploy.yml      ← GitHub Actions Pages deploy
├── fonts/                        ← Self-hosted typefaces (not tracked in git)
├── images/                       ← Object photos + attribution files (not tracked)
├── charts/                       ← Finder chart PNGs M1–M110 (not tracked)
└── docs/                         ← Internal developer documentation
    ├── project-design-summary.md
    └── css-cheatsheet.md
```

---

## Remaining Open Items

### Data Verification
All 110 object coordinates in the DATA array should be cross-checked against an authoritative source (e.g. SEDS Messier catalogue or SIMBAD) to confirm RA/Dec accuracy and PSA page numbers.

### Planned Future Features

**Session Planner** — a lightweight view showing which objects are observable on a given night based on date and observer latitude, highlighting the critical western and eastern horizon objects that define the marathon window.

**AL Certificate Export** — generate a formatted printable summary in the style clubs typically require for submission: observer name, date range, equipment, and a numbered observation list with descriptions.

**Multi-page object records** — dedicated per-object pages (`objects/m1.html` … `objects/m110.html`) for a richer record view with larger sketch area, image zoom, and a direct link from the lightbox. Would be generated from a template rather than hand-authored.

---

*Last updated: March 2026*
