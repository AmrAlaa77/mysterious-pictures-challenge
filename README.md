# Mysterious Pictures Challenge

Static, single-page facilitator + participant app. No backend, no database.

## Files
- `index.html` — the whole app (facilitator UI + participant router based on `?v=TOKEN`)
- `qrcode.js` — vendored MIT "qrcode-generator" library (Kazuhiko Arase), generates QR codes fully client-side, no network calls
- `images/*.jpg` — the 30 pictures, filenames are the opaque tokens (same tokens used in participant URLs)
- `mapping.json` — human-readable record of token → sequence position → image file (for reference/restoration only; the live app has this baked into `index.html`)
- `tokens.json` — the raw ordered token list (position 1..30)
- `build_index.js` — regenerates `index.html` from `tokens.json`. Re-run with `node build_index.js` if you ever need to change the base URL or rebuild.

## How it works
- Facilitator opens the root URL with no query string: `https://AmrAlaa77.github.io/mysterious-pictures-challenge/`
- Participant scans a QR code that opens `https://AmrAlaa77.github.io/mysterious-pictures-challenge/?v=<token>` — the token is a permanent, opaque, random 8-character id that reveals nothing about picture order. The page detects `?v=` and renders **only** that one image full-screen on black, nothing else.
- The token → picture mapping is fixed forever in `index.html` (baked in at build time). Shuffling on the facilitator screen only changes which QR *card* appears in which visual slot (and its A/B/C/D label) — it never changes which token points to which picture.
- Selecting "N pictures" always uses positions 1..N (the most-zoomed-in picture through the Nth), never a random subset.

## Redeploying / regenerating
If you ever need to change the GitHub Pages URL, edit `BASE_URL` in `build_index.js` and run:
```
node build_index.js
```
Do NOT change `tokens.json` unless you intend to invalidate all previously printed/shared QR codes — the tokens are meant to be permanent.

## Hosting
Deployed as a static site via GitHub Pages from the repository root (or `/docs`, depending on how you configured Pages). No server, database, or paid service required.
