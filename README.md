# Prismdrifter — Brand Assets

Public asset site. Logos, illustrations, typeface and brand documentation, downloadable as PNG or SVG in any colour and size.

Live at **ivanvidovic.github.io/prismdrifter**

---

## Before it works

Two folders ship empty. Add these files and everything switches on — no code changes.

**`assets/fonts/`** — from [fonts.google.com/specimen/Archivo](https://fonts.google.com/specimen/Archivo)

```
Archivo-VariableFont_wdth,wght.ttf
Archivo-Italic-VariableFont_wdth,wght.ttf
OFL.txt
```

Filenames must match exactly, commas included. `OFL.txt` is required — redistributing the font means shipping its licence. All three go into the ZIP the site builds on download.

**`assets/docs/`**

```
Prismdrifter_Brand_Assets_01.pdf
```

Until these land, the font row and PDF row say so instead of failing silently. Same for any missing SVG: its tile shows a dashed box naming the file it expected.

---

## Turning on GitHub Pages

Settings → Pages → Source: **Deploy from a branch** → Branch: `main`, folder: `/ (root)` → Save.

First build takes a minute or two.

---

## Structure

```
index.html                 entire site — no build step, no dependencies
.nojekyll                  serve files as-is
assets/
  logos/                   11 SVGs — primary marks and alternates
  illustrations/           11 SVGs
  fonts/                   2 TTFs + OFL.txt          ← add
  docs/                    brand PDF                 ← add
```

---

## Editing

Everything configurable sits at the top of the `<script>` block in `index.html`.

**Rename an asset** — the second string in each manifest row is the label shown on the card. The first is the filename and must match disk.

```js
['Prismdrifter_Logo_A.svg', 'Logo A'],
```

**Add an asset** — drop the SVG in the right folder, add a row to the matching group. Size, colour, dimensions and stroke handling are all derived from the file at load, so nothing else needs touching.

**Swap an asset** — overwrite the file, keep the name. No code change.

**Change export sizes**

```js
const SIZES        = [256, 512, 1024, 2048, 4096];
const SIZE_MAX     = 8192;   // custom-size ceiling
const STROKE_FLOOR = 1.5;    // minimum rendered stroke, px
const DEFAULT_SIZE = 1024;
```

---

## How exports behave

**Size is the longest edge.** A 2000×2000 mark at 1024 gives 1024×1024. The horizontal lockups are 8.2:1, so 1024 gives 1024×125. Every card prints its exact output dimensions before you download.

**PNGs are transparent.** The preview backdrop flips black or white to keep the mark visible; it isn't part of the file.

**Thin strokes are protected.** Below roughly 1024, hairline artwork renders under a pixel wide and washes out to grey. Those strokes get scaled up to hold at `STROKE_FLOOR`, and the card says so when it happens. Affects the outline marks and the arrow illustrations.

**SVG downloads are recoloured but never thickened.** Vector has no pixel size, so compensation would just corrupt the source.

**Large exports are checked.** Past a browser's canvas ceiling, `drawImage` quietly does nothing and you get an empty file. Anything over 4096² is verified before saving and fails loudly instead.

---

## Notes

Brand assets in this repository are proprietary to Prismdrifter. Archivo is licensed separately under the SIL Open Font License 1.1.

Pattern generator: [Visual Loom](https://ivanvidovic.github.io/loomrift/)
