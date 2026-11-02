# OCCULAR

**Frame metadata check for oblique camera counts (OCC).**

OCCULAR is a single-file, browser-based quality-control tool for aerial survey imagery. Drop in a flight folder and it reads each frame's EXIF metadata locally, checks camera settings against a configurable spec, and flags frames that break it. Nothing is uploaded; files never leave the machine.

Built at Save the Elephants as part of the Aerial Survey Reform work, to catch camera-setting problems (motion-blur-risk shutter speeds, ISO drift, a changed aperture, a body or lens swap, dropped triggers, missing GPS) before a sortie's images go into counting.

## Run it

Open `index.html` in any modern browser (Chrome or Edge recommended for folder selection). No install, no build step.

> **Connectivity:** the metadata reader (exifr) and map library (Leaflet) currently load from a CDN, so the first run needs an internet connection. See *Known issues*.

## What it does

- **Configurable checks:** each rule has a test, value, tolerance, an "if tag missing" behaviour, and a severity (fail / check).
- **Starting profiles:** Locked manual · Auto-ISO tolerant · Fully automatic camera · Inventory only. Profiles can be saved and reloaded as JSON.
- **Normalised fields across camera brands:** shutter, ISO, aperture, focal length, white balance, exposure mode, exposure compensation, image size, camera model, lens, GPS fix, trigger gap.
- **Drift tests** against the flight median (e.g. ISO within ±1.5 stops, trigger gap under 1.8× the median interval).
- **Any raw tag** the camera wrote can be added as a check. *Deep read* also parses XMP and IPTC.
- **Views:** verdict summary, shutter distribution against spec, results per check, contact sheet (embedded previews), sortable table, GPS track map, and a pre-flight sign-off checklist for settings that standard EXIF doesn't record (Long Exposure NR, stabilisation, pod lock, clock sync, card, apron test burst).
- **Exports:** full CSV, failures-only CSV, every-tag CSV. Pre-flight attestation, operator, sortie ID and the active checks are written into the CSV header.

## Repository layout

| Path | Contents |
|---|---|
| `index.html` | Current version (v2), with rule editor and profiles |
| `legacy/occular-v1.html` | First version, kept for reference |

## Known issues

- Capture times are displayed in UTC after being read as local time, so on an EAT (UTC+3) machine they show 3 hours early. Gaps and durations are unaffected.
- The tool needs a CDN connection to load exifr. The offline fallback described in the error message is not implemented yet.
- The trigger-gap check flags slow gaps (dropped triggers) but not near-zero gaps (double fires).
- Metadata only: there is no image sharpness / blur scoring yet.

## Roadmap

- Inline exifr for true offline use
- Fix the capture-time display
- Laplacian-variance sharpness score from embedded previews
- GPX clock-offset estimation and geotagging for frames without GPS

## Credits

Built by Yuka Gliday (Save the Elephants) through AI-assisted development. Uses [exifr](https://github.com/MikeKovarik/exifr) and [Leaflet](https://leafletjs.com/).

## Licence

MIT, see [LICENSE](LICENSE). Free to use, adapt and share with attribution.
