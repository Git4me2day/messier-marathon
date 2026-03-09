# 🔭 Messier Marathon — Observer's Log

A beautifully designed, fully self-contained observation log for the **Messier Marathon** and the **Astronomical League Messier Observing Program**. Built for the field — red night-vision mode, offline-capable, no install, no account, no server.

![Night Mode](https://img.shields.io/badge/Night%20Mode-Red%20Vision-cc3322?style=flat-square)
![Objects](https://img.shields.io/badge/Objects-110%20Messier-gold?style=flat-square)
![AL Program](https://img.shields.io/badge/AL-Messier%20Club-8844aa?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## Features

- **Red night-vision mode** — deep black background with full red-spectrum text to preserve dark adaptation; toggle to warm parchment day mode for planning indoors
- **110 Messier objects** in optimal marathon search order, west to east
- **Per-object checkboxes and field notes** — persisted in `localStorage` across sessions
- **Automatic observation timestamp** — silently recorded when you check an object; never clutters the notes field
- **Progress bar** — live count of observed vs. total objects
- **Filter and search** — filter by Galaxy / Open Cluster / Globular / Nebula / Unobserved; search by M#, type, or constellation
- **Image lightbox** — click any M# to open the object photo, full data sidebar, local finder chart, and Stellarium Web link
- **Full AL observation records** — per-object form with all Astronomical League required fields; accessible via the ✎ button in the notes column
- **Session Setup** — set common defaults once per night and have them auto-fill every new record; includes device geolocation
- **Log menu** — Print, Export CSV, Import CSV, Export AL Records (JSON), Import AL Records (JSON), Session Setup, Reset All Data

---

## Quick Start

```
index.html          ← open this in any modern browser
faq.html            ← frequently asked questions
mm-shared.css       ← shared stylesheet (must be alongside index.html)
fonts/              ← self-hosted typefaces (see Fonts section below)
images/             ← optional object photos
charts/             ← optional local finder charts
```

No server, no build step, no dependencies.

---

## Fonts

Typography is self-hosted — no Google Fonts or CDN calls at runtime. Download the following from [Google Fonts](https://fonts.google.com) and place them in a `fonts/` folder:

| File | Family |
|------|--------|
| `Cinzel-Regular.ttf` | [Cinzel](https://fonts.google.com/specimen/Cinzel) |
| `Cinzel-SemiBold.ttf` | Cinzel |
| `Cinzel-Bold.ttf` | Cinzel |
| `ShareTechMono-Regular.ttf` | [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) |
| `CrimsonPro-Light.ttf` | [Crimson Pro](https://fonts.google.com/specimen/Crimson+Pro) |
| `CrimsonPro-Regular.ttf` | Crimson Pro |
| `CrimsonPro-LightItalic.ttf` | Crimson Pro |

---

## Adding Photos

Place JPEGs in an `images/` folder named with an uppercase M (`M1.jpg` … `M110.jpg`).

```bash
# Linux / macOS — wget
mkdir -p images && for i in $(seq 1 110); do
  wget -q --show-progress --tries=3 --timeout=15 \
    -O "images/M${i}.jpg" "http://www.messier.seds.org/Jpg/m${i}.jpg" \
    || echo "⚠ Failed: M${i}.jpg"
done
```

```bash
# macOS — curl
mkdir -p images && for i in $(seq 1 110); do
  curl -s -o "images/M${i}.jpg" "http://www.messier.seds.org/Jpg/m${i}.jpg" \
    && echo "✓ M${i}.jpg" || echo "⚠ Failed: M${i}.jpg"
done
```

Generate attribution text files: `chmod +x create_attributions.sh && ./create_attributions.sh`

---

## Finder Charts

Place PNGs in a `charts/` folder (`M1.png` … `M110.png`). Use `generate_charts.py` to produce red-on-black night-vision charts for all 110 objects:

```bash
pip install matplotlib numpy
python generate_charts.py
```

---

## Data Export and Import

All user data is managed via **☰ Log** in the toolbar.

**Observation Log (CSV)** — exports `YYYY-MM-DD-HHmmss-observing-log.csv` with marathon order, object, catalog data, observed status, observation timestamp, and field notes. On import you choose Overwrite or Merge.

**AL Records (JSON)** — exports `YYYY-MM-DD-HHmmss-al-records.json` with all full observation records. Import merges with existing records and shows a conflict count before confirming.

---

## AL Observation Records

Click **✎** in any object's notes cell to open the full record form:

- **When & Where** — date, time, observing site (with optional GPS → place name lookup)
- **Sky Conditions** — seeing (Antoniadi 1–5), transparency, limiting magnitude
- **Equipment** — telescope, aperture, eyepiece, magnification
- **Observation** — written description (required for AL certificate), sketch notes

The ✎ icon is outlined when empty and fills solid red when a record is saved.

---

## Session Setup

**☰ Log → Session Setup** sets defaults that auto-fill blank fields in every record opened during the session. The **📍 Locate** button gets GPS coordinates from the device and reverse-geocodes to a place name via OpenStreetMap. A pulsing **Session active** pill appears in the toolbar while defaults are active.

---

## localStorage Keys

| Key | Contents |
|-----|----------|
| `mm_checks` | Observed state per object |
| `mm_notes` | Brief field notes per object |
| `mm_times` | ISO timestamp of each observation |
| `mm_fullnotes` | Full AL record data per object |
| `mm_session` | Current session defaults |
| `mm_theme` | `"dark"` or `"light"` |

---

## GitHub Pages Deployment

1. Rename `messier_marathon.html` → `index.html`
2. Push to GitHub
3. **Settings → Pages → Source → GitHub Actions**

The included `.github/workflows/deploy.yml` publishes only public-facing files — developer docs, scripts, and the CSS cheat sheet are excluded from the live site.

---

## Browser Compatibility

Chrome, Firefox, Safari, and Edge (current versions). Requires `localStorage`. Geolocation requires HTTPS or `localhost`.

---

## Acknowledgements

- Object data: **Messier Catalog** (Charles Messier, 1771)
- Marathon order: standard AL / community observing sequence
- PSA references: *Pocket Sky Atlas*, Roger Sinnott (Sky & Telescope)
- Sky simulation: [Stellarium Web](https://stellarium-web.org)
- Reverse geocoding: [OpenStreetMap Nominatim](https://nominatim.openstreetmap.org)
- Object photos: [SEDS Messier Catalog](http://www.messier.seds.org) (optional, downloaded separately)

---

## License

MIT — free to use, share, and modify. Clear skies! 🌌
