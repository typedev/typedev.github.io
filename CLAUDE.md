# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitHub Pages static website (`typedev.github.io`) hosting browser-based font
development tools. Every tool is client-side: font files are parsed, shaped and rebuilt
in the browser and never uploaded. The set grows — nothing in the site should hardcode
how many tools there are.

## Layout

One directory per tool, each served as `index.html`:

| Path | Tool | Edited here? |
| --- | --- | --- |
| `index.html` | Landing page listing every tool | yes |
| `fonttester/` | Font Tester | yes |
| `panose/` | PANOSE Editor | yes |
| `family-inspector/` | Font Family Inspector | yes |
| `fea-proof/` | OpenType Features Proof | no — deploy target of `typedev/fea-proof` |
| `rangeproof/` | Range Proof | no — deploy target of `typedev/range-proof` |
| `ot-edit/` | OT Tables Compare & Patch | no — deploy target of `typedev/OT-tables-online` |

The three deploy targets are build output pushed by each source repo's `deploy.sh`.
Never hand-edit them here — changes will be overwritten on the next deploy.

`fonttester/panose.html` and `fonttester/font-family-inspector.html` are redirect stubs
left behind when those two tools moved out of `fonttester/`.

When adding a tool, give it its own directory with an `index.html` and add a card to
the landing page (`index.html`) in the matching section — Proofing or Metadata.

## Architecture

### Font Tester (`/fonttester/index.html`)

A hinting proof: renders two fonts side by side from 72 pt down to 8 pt so TrueType and
PostScript hinting can be read at text sizes. Hinting only shows where the platform
applies it — Windows in full, Linux in part, macOS not at all — so the tool is only
meaningful on Windows or Linux. Capabilities:

- **Font Loading**: Supports TTF, OTF, WOFF, WOFF2 formats via drag-and-drop or file picker. Uses `opentype.js` for font parsing and `woff2-encoder` for WOFF2 decompression (lazy-loaded).
- **Variable Font Support**: Extracts and exposes variation axes (weight, width, slant, etc.) and named instances from variable fonts via the fvar table.
- **OpenType Features**: Detects and allows toggling of GSUB/GPOS features (ligatures, stylistic sets, etc.).
- **Preview Modes**: Five size ranges (XXL 72-57pt, XL 56-33pt, Large 32-21pt, Medium 25-14pt, Small 18-8pt) plus a mixed preview that alternates fonts per word.
- **State**: Uses localStorage for theme (`theme`) and unit preference (`fontUnit`).

Key global state objects:
- `loadedFonts` / `fontData` / `fontObjects` - Font file data for left/right slots
- `variationAxes` / `variationSettings` / `namedInstances` - Variable font controls
- `activeFeatures` / `availableFeatures` - OpenType feature toggles
- `woff2Decompress` - Lazy-loaded WOFF2 decompressor

### Landing page (`/index.html`)

Self-contained, no build step. Fonts come from Google Fonts: Recursive (variable —
headings via `MONO 0`, mono labels via `MONO 1`) and Newsreader (prose). Palette
tokens live in `:root`; blue `--guide` marks structure and links, amber `--mark`
marks "found or changed" — the same meaning those colors carry inside the tools.
Each card holds a CSS-only miniature of that tool's own output.

### PANOSE Editor (`/panose/index.html`)

Reads and rewrites the ten PANOSE bytes in `OS/2`. Batch-loads a family, downloads
modified binaries, exports the family's PANOSE scheme.

### Font Family Inspector (`/family-inspector/index.html`)

Groups dropped files by Typographic Family (name ID 16, falling back to ID 1) and
sub-groups by RIBBI family, then validates each face: RIBBI-ness of ID 2,
`fsSelection` / `macStyle` bit agreement, `USE_TYPO_METRICS`, presence of ID 16/17,
presence of `STAT`, and weight class against the declared style. Exports CSV.

## Development

This is a static site with no build process. To develop:

1. Serve files locally with any static server (e.g. `python3 -m http.server 8000`)
2. Open `http://localhost:8000/` in a browser
3. Deploy by pushing to the `master` branch (GitHub Pages)

External dependencies (loaded from CDN by the hand-written pages):
- `opentype.js` - Font parsing (Font Tester, PANOSE Editor, Font Family Inspector)
- `woff2-encoder` - WOFF2 decompression (Font Tester, lazy-loaded on first WOFF2 file)
- Google Fonts - Recursive and Newsreader (landing page only)
