# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitHub Pages static website (`typedev.github.io`) containing font development tools. The main application is a font comparison tool that allows drag-and-drop comparison of two font files.

## Architecture

### Font Tester (`/fonttester/index.html`)

A single-page application for comparing fonts side-by-side with these capabilities:

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

## Development

This is a static site with no build process. To develop:

1. Serve files locally with any static server (e.g., `python -m http.server`)
2. Open `fonttester/index.html` in browser
3. Deploy by pushing to the `master` branch (GitHub Pages)

External dependencies (loaded from CDN):
- `opentype.js` - Font parsing
- `woff2-encoder` - WOFF2 decompression (lazy-loaded on first WOFF2 file)
