# Messier Marathon Observer's Log — Feature Guide

*A complete reference to everything the site can do.*

---

## What Is This?

The Messier Marathon Observer's Log is a free, browser-based tool for amateur astronomers observing the 110 objects in the Messier catalogue. It serves two purposes:

1. **Marathon night** — a fast, red-light-friendly checklist for the annual Messier Marathon, where observers attempt all 110 objects in a single night.
2. **AL Messier Club program** — a structured observation record for earning the Astronomical League's Messier Observing Program certificate.

Nothing is installed. Nothing is sent to a server. All data lives in your browser.

---

## Night Vision Mode

The site opens in **dark / night mode** by default: a near-black background with all text and controls rendered in shades of red. Red light does not constrict the pupil or bleach rhodopsin the way white light does, so your eyes stay dark-adapted while you navigate the log.

A pill-shaped toggle in the top-right corner switches between:

- 🌙 **Dark mode** — red-on-black, for use at the telescope
- ☀ **Light mode** — dark ink on warm parchment, for planning indoors

Your preference is remembered between visits.

---

## The Main Table

The centrepiece of the site is a 110-row table listing every Messier object in **marathon search order** — the sequence that gives the best statistical chance of observing all 110 in one night, beginning with western objects visible just after dusk and ending with eastern objects visible just before dawn. This sequence is different from Messier number order.

Each row shows:

| Column | What it tells you |
|--------|-------------------|
| **#** | Position in the marathon sequence (1–110) |
| **Object** | Messier designation — click to open the image lightbox |
| **Type** | Object type with a colour-coded dot (11 types) |
| **Con** | Constellation (IAU 3-letter abbreviation) |
| **Mag** | Visual magnitude |
| **RA / Dec** | J2000.0 coordinates |
| **PSA Pg** | Page in *Pocket Sky Atlas* (Sinnott) |
| **✓** | Observation checkbox |
| **Field Notes** | Brief inline notes field + full-record button |

A colour-coded legend panel below the toolbar identifies all 11 object types: spiral galaxy, elliptical galaxy, lenticular galaxy, irregular galaxy, open cluster, globular cluster, diffuse nebula, planetary nebula, supernova remnant, star cloud, and double star / asterism.

---

## Progress Bar

A bar at the top of the table shows your running count of observed objects (e.g. **47 / 110**). It fills and updates in real time as you check objects.

---

## Filtering and Search

Two controls above the table narrow what you see without losing your place:

**Filter buttons** — click to show only one category:
All · Galaxy · Open Cluster · Globular · Nebula · Unobserved

**Search box** — type any fragment to filter by Messier number, object type, or constellation abbreviation. Clears instantly with the × button.

Filters and search combine — e.g. "Globular" filter + "Sgr" in the search shows only globular clusters in Sagittarius.

---

## Checkboxes and Field Notes

Every object has a **checkbox** and a **notes field** in the same row.

- Check the box when you've seen the object. The row dims slightly and the progress bar advances.
- Type anything in the notes field — time of observation, eyepiece used, sky conditions, a quick impression.
- Both are saved automatically to browser storage and survive page refreshes, browser restarts, and computer reboots. You never need to click Save.
- When you check an object, an **observation timestamp** is silently recorded in the background. It appears in the full observation record and in CSV exports, but never clutters the table.

---

## Image Lightbox

Click any object's **M-number** in the table to open the lightbox modal. It shows:

**Left panel**
- Object photograph (from a local `images/` folder; a placeholder is shown if the image is absent)
- Image credit attribution (loaded from a paired text file)

**Right sidebar**
- Full catalog data: type, constellation, magnitude, RA, Dec, PSA page
- **🗺 Finder Chart** button — opens a local chart image from `charts/M[n].png`
- **🔭 Stellarium Web** button — opens the free online sky simulator centred on this object

**Footer**
- ← Prev / position indicator / Next → to step through all 110 objects
- Keyboard shortcuts: ← → to navigate, Escape or click outside to close

---

## Full AL Observation Records

For each object, a **✎** button sits at the right edge of the notes cell.

- **Outlined** (dim) — no full record saved yet
- **Filled red** — a full record has been saved for this object

Click it to open the full observation record form. This captures everything the Astronomical League requires for the Messier Club observing certificate:

**When & Where**
- Date of observation
- Time (local)
- Observing site / location (can be filled automatically — see Session Setup)

**Sky Conditions**
- Seeing on the Antoniadi scale (1 Perfect → 5 Very bad)
- Transparency (1 Very poor → 5 Excellent)
- Limiting magnitude

**Equipment**
- Telescope or instrument description
- Aperture in millimetres
- Eyepiece
- Magnification

**Observation**
- Written description of what you saw — size, shape, brightness, structure, whether averted vision was needed, etc. This field is required for the AL certificate.
- Sketch notes — orientation, field stars noted, anything relevant to a sketch made at the eyepiece

If you have brief notes already typed in the table, they are pre-filled into the description field when you open an empty record, so you are not starting from scratch.

The hidden observation timestamp is displayed at the bottom of the modal for reference.

All full records are saved automatically to browser storage under the key `mm_fullnotes`.

---

## Session Setup

At the start of each observing session, open **☰ Log → Session Setup** to enter shared defaults that will auto-fill blank fields in every observation record you open that night.

Fields you can set as session defaults:

- Observing site / location
- Seeing
- Transparency
- Limiting magnitude
- Telescope / instrument
- Aperture
- Eyepiece
- Magnification

**How auto-fill works:** when you open an object's full record, any field that already has saved data is left exactly as-is. Only blank fields are filled from the session. This means you can freely override the default for any individual object without affecting the session.

**📍 Locate button** — requests your GPS position from the device (browser permission required; works over HTTPS or localhost). Once coordinates are obtained, the site calls OpenStreetMap's free reverse-geocoding service to convert them to a human-readable place name, which is filled into the Site field if it is empty. The raw coordinates are also stored in the session for reference.

A pulsing **Session active** indicator appears in the toolbar whenever any session default is set, so you always know the auto-fill is running. Use **↺ Clear Session** in the modal to remove all defaults without affecting any saved records.

Session data is stored in `mm_session` localStorage and survives page refreshes.

---

## The Log Menu

A single **☰ Log ▼** button in the toolbar opens a dropdown with all data management options:

| Item | What it does |
|------|--------------|
| ⎙ Print | Opens the browser print dialog with a clean black-on-white stylesheet applied — no red backgrounds, no wasted ink |
| ⬇ Export CSV | Saves a complete observation log as a CSV file |
| ⬆ Import CSV | Loads a previously exported CSV back in |
| ⬇ Export AL Records | Saves all full observation records as a JSON file |
| ⬆ Import AL Records | Loads a previously exported JSON file back in |
| ⚙ Session Setup… | Opens the session defaults modal |
| ↺ Reset All Data… | Clears everything with a confirmation dialog |

---

## CSV Export and Import

**Export** produces a file named `YYYY-MM-DD-HHmmss-observing-log.csv` containing one row per Messier object with these columns:

Marathon Order · Object · Type · Constellation · Magnitude · RA (J2000) · Dec (J2000) · PSA Page · Observed · Date Observed · Field Notes

The Date Observed column is populated from the hidden observation timestamp — it shows the exact moment you ticked the checkbox.

**Import** reads a CSV exported from this page (or any CSV with matching column headers — column order doesn't matter). Before applying the data, you are shown:

- How many objects are in the file
- Which objects would overwrite existing observations or notes (conflicts)

You then choose:
- **Cancel** — do nothing
- **Merge** — import only fills in blanks; existing data is untouched
- **Overwrite** — incoming data replaces everything

---

## AL Records JSON Export and Import

**Export** produces a file named `YYYY-MM-DD-HHmmss-al-records.json` containing all of your full observation records in a structured format, with a version number and export timestamp for forward compatibility.

**Import** reads a previously exported JSON file. You are shown how many records are in the file and how many would overwrite existing records, then asked to confirm. Incoming records are merged with (and overwrite) existing ones. After import, the ✎ button indicators refresh immediately across the whole table.

This is the recommended way to back up your full records, move them between devices, or hand them off to someone else.

---

## Resetting Data

**☰ Log → Reset All Data…** shows a count of how many observations and notes you have saved before asking you to confirm. Confirmed reset clears all six `mm_*` localStorage keys:

`mm_checks` · `mm_notes` · `mm_times` · `mm_fullnotes` · `mm_session` · `mm_theme`

This is permanent and cannot be undone — export your data first if you want to keep it.

---

## Browser Storage

All user data is stored in your browser's **localStorage** — a small private database that persists between sessions on the same device and browser. Nothing is transmitted anywhere.

| Key | Contents |
|-----|----------|
| `mm_checks` | Which objects you have observed |
| `mm_notes` | Your brief field notes |
| `mm_times` | Timestamps of each observation |
| `mm_fullnotes` | Your full AL observation records |
| `mm_session` | Current session defaults |
| `mm_theme` | Dark or light mode preference |

Data is tied to the specific browser and device. To move data to another device, use Export / Import.

Clearing your browser's site data or using private/incognito mode will erase localStorage. Export your data regularly if it matters to you.

---

## Printing

**☰ Log → Print** applies a print stylesheet that:

- Renders everything in black ink on white paper
- Hides the toolbar, filters, search, and modal overlays
- Preserves the full table including any notes you have typed
- Fits cleanly on standard paper sizes

Use your browser's print-to-PDF option to save a snapshot of your log.

---

## Works Offline

After the page has loaded once, the core tool — table, checkboxes, notes, full record forms, lightbox, and all data management — works entirely without an internet connection. The only features that require network access are:

- **📍 Locate** (geolocation place-name lookup via OpenStreetMap)
- **🔭 Stellarium Web** (external link)

Local finder charts, object photos, and all saved data work completely offline.

---

*Messier Marathon Observer's Log — clear skies!*
