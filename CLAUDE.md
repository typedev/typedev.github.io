# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitHub Pages static website (`typedev.github.io`) containing font development tools. The main application is a font comparison tool that allows drag-and-drop comparison of two font files.

## Architecture

### Font Tester (`/fonttester/index.html`)

A single-page application for comparing fonts side-by-side with these capabilities:

- **Font Loading**: Supports TTF, OTF, WOFF formats via drag-and-drop or file picker. Uses `opentype.js` library for font parsing.
- **Variable Font Support**: Extracts and exposes variation axes (weight, width, slant, etc.) and named instances from variable fonts via the fvar table.
- **OpenType Features**: Detects and allows toggling of GSUB/GPOS features (ligatures, stylistic sets, etc.).
- **Preview Modes**: Three size ranges (large/medium/small waterfall) plus a mixed preview that alternates fonts per word.
- **State**: Uses localStorage for theme (`theme`) and unit preference (`fontUnit`).

Key global state objects:
- `loadedFonts` / `fontData` / `fontObjects` - Font file data for left/right slots
- `variationAxes` / `variationSettings` / `namedInstances` - Variable font controls
- `activeFeatures` / `availableFeatures` - OpenType feature toggles

## Development

This is a static site with no build process. To develop:

1. Serve files locally with any static server (e.g., `python -m http.server`)
2. Open `fonttester/index.html` in browser
3. Deploy by pushing to the `master` branch (GitHub Pages)

External dependency: `opentype.js` loaded from CDN.
