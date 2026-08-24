# typedev.github.io

Type design tools that run in your browser. Every tool is a static page — parsing, shaping and
rebuilding happen client-side, so no font file is ever uploaded.

**Live:** [typedev.github.io](https://typedev.github.io/)

## Proofing — what the font draws

| Tool | What it does | Formats |
| --- | --- | --- |
| [Font Tester](https://typedev.github.io/fonttester/) | Renders two fonts side by side from 72 pt down to 8 pt to proof how their TrueType or PostScript hinting rasterizes, with variable axes and OpenType features switchable at every size. **Only Windows applies hinting in full and Linux in part — proof there; macOS ignores it.** | OTF · TTF · WOFF · WOFF2 |
| [OpenType Features Proof](https://typedev.github.io/fea-proof/) | Shows what each GSUB/GPOS feature actually does, proofed on real words in the right language. Per-language `locl` inventories, feature combinations, mark/mkmk anchor explorer. ([source](https://github.com/typedev/fea-proof)) | OTF · TTF · WOFF · WOFF2 |
| [Range Proof](https://typedev.github.io/rangeproof/) | Coverage of 61 curated Unicode blocks: missing assigned characters, codepoints mapped beyond the standard, and everything else in the file — down to glyphs no codepoint reaches. ([source](https://github.com/typedev/range-proof)) | OTF · TTF · WOFF · WOFF2 |

## Metadata — what the font declares

| Tool | What it does | Formats |
| --- | --- | --- |
| [OT Tables Compare & Patch](https://typedev.github.io/ot-edit/) | Lines up `head`, `name`, `OS/2`, `STAT` and `avar` from any number of fonts and highlights what differs. Edit values and download rebuilt binaries, or export a JSON patch for the CLI. ([source](https://github.com/typedev/OT-tables-online)) | OTF · TTF |
| [PANOSE Editor](https://typedev.github.io/panose/) | Reads the ten PANOSE digits out of `OS/2`, spells out what each one classifies, and writes changes back into the font. | OTF · TTF |
| [Font Family Inspector](https://typedev.github.io/family-inspector/) | Groups files into families and RIBBI style slots, then flags the faults that break style linking — `fsSelection`, `macStyle`, missing typographic names, absent `STAT`. | OTF · TTF · WOFF · WOFF2 |

## Layout

```
index.html                              landing page
fonttester/index.html                   Font Tester
panose/index.html                       PANOSE Editor
family-inspector/index.html             Font Family Inspector
fea-proof/                              built from typedev/fea-proof
rangeproof/                             built from typedev/range-proof
ot-edit/                                built from typedev/OT-tables-online

fonttester/panose.html                  redirect stub -> /panose/
fonttester/font-family-inspector.html   redirect stub -> /family-inspector/
```

The two stubs keep the pre-move URLs working; delete them once nothing links there.

`fea-proof`, `rangeproof` and `ot-edit` are deploy targets — edit them in their own
repositories, not here. The rest are hand-written single-file pages.

## Development

No build step. Serve the directory with any static server and open a page:

```sh
python3 -m http.server 8000
```

Deploy by pushing to `master`.
