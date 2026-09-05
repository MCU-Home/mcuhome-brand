# mcuhome-brand

The MCUHome corporate identity in one place: the logo assets, the color and
typography tokens as `style.css`, and this design guide. Everything MCUHome
projects and third parties need to display the mark and match the look lives
here.

## Logo

The mark is a house with signal waves above it and connector pins on both
sides, drawn on a `0 0 64 64` viewBox. The door is a real transparent cutout,
so the mark works on any background.

### Variants

There are two drawings, each in a light-background and a dark-background
version — four files in total.

| Variant | Detail | Use for |
|---|---|---|
| Normal | 4 pins, 3 waves | Everything from roughly 32 px upwards: sites, documents, presentations |
| Simplified | 2 pins, 2 waves | Favicons and small renderings |

Light and dark versions differ in exactly one value: the color of the pins.
Everything else — house, waves, geometry — is identical.

### Assets

Everything in `assets/` is distributable; the file names are stable, so the
folder can be unpacked into a consuming project as-is.

| File | Content |
|---|---|
| `logo.svg` | Full mark (4 pins, 3 waves) for light backgrounds |
| `logo-on-dark.svg` | Full mark for dark backgrounds |
| `logo-simplified.svg` | Reduced mark (2 pins, 2 waves) for small renderings, light backgrounds |
| `logo-simplified-on-dark.svg` | Reduced mark for dark backgrounds |
| `favicon-16.png`, `favicon-32.png`, `favicon-48.png` | Favicon bitmaps, transparent background |
| `favicon.ico` | The three favicon sizes in one file |
| `avatar.png` | 512x512 organization avatar, dark background, safe inside a round crop |

All raster assets are generated from `logo-simplified.svg` and
`logo-simplified-on-dark.svg` — there is no separate source for any size —
with `rsvg-convert` and ImageMagick:

```sh
cd assets
rsvg-convert -w 16 -h 16 logo-simplified.svg -o favicon-16.png
rsvg-convert -w 32 -h 32 logo-simplified.svg -o favicon-32.png
rsvg-convert -w 48 -h 48 logo-simplified.svg -o favicon-48.png
magick favicon-16.png favicon-32.png favicon-48.png favicon.ico
rsvg-convert -w 338 -h 338 logo-simplified-on-dark.svg -o /tmp/mark.png
magick -size 512x512 xc:'#1d1815' /tmp/mark.png -gravity center \
    -composite -alpha off -depth 8 -strip PNG24:avatar.png
```

### Known limitation at 16 px

At 16 px the two wave arcs of the simplified mark are close enough that they
read as one merged band with a lighter waist rather than two clearly separate
arcs; the mark reads as a house with a signal cap. 32 px and 48 px show both
arcs separately. This is inherent to the arc spacing at that size, not a
rendering artifact — thinner strokes widen the gap but do not open it.

### Colors in the mark

| Element | Color |
|---|---|
| House and waves | `#d95513` (the accent color) |
| Pins on light backgrounds | `#b0a89e` |
| Pins on dark backgrounds | `#8a8078` |

The pin color is also available as a token (`--mcuhome-pin-gray`), so
surrounding UI can match the mark.

### Usage rules

The logo assets are not covered by the repository's Apache-2.0 license. They
are the project's marks and are governed by the brand usage policy in
[LICENSES/LicenseRef-MCUHome-Brand.txt](LICENSES/LicenseRef-MCUHome-Brand.txt):
in short, using the unmodified marks to refer to MCUHome is permitted, using
them as another identity, in modified form, or in a way that implies
affiliation or endorsement is not. The policy file is authoritative — read it
before using the marks.

Beyond the policy, two practical points:

- Pick the version that matches the background (`-on-dark` on dark surfaces),
  and use the simplified drawing only where the normal one loses detail.
- Do not recolor, redraw, rotate, or add effects to the mark; scaling and
  placing it on a background are the intended adjustments.

## Colors

All colors are available as CSS custom properties in
[`style.css`](style.css). Each token is defined once and resolves to its light
or its dark value depending on the active color scheme, so the same variable
name is used in both themes.

### Core tokens

| Token | Variable | Light | Dark |
|---|---|---|---|
| Accent | `--mcuhome-accent` | `#d95513` | `#ef7b33` |
| Accent, strong | `--mcuhome-accent-strong` | `#a8420f` | `#f49a5e` |
| Ink (text) | `--mcuhome-ink` | `#221c18` | `#f4efe9` |
| Muted text | `--mcuhome-muted` | `#6f6659` | `#a89d90` |
| Border | `--mcuhome-border` | `#e8e1d6` | `#3d342c` |
| Background | `--mcuhome-bg` | `#fdfbf8` | `#1d1815` |
| Surface (cards) | `--mcuhome-surface` | `#ffffff` | `#292219` |
| Background, alternate | `--mcuhome-bg-alt` | `#f4efe9` | `#231d17` |
| Pin gray (matches the mark) | `--mcuhome-pin-gray` | `#b0a89e` | `#8a8078` |

`--mcuhome-accent-strong` is the hover/pressed variant of the accent: darker
on light backgrounds, brighter on dark ones. `--mcuhome-bg-alt` sits between
background and surface and is meant for subtly separated areas such as table
headers or code blocks.

### Semantic tokens

Four states, four tokens each: the color itself, a tinted fill, a border for
that fill, and the text color on it.

Light theme:

| State | Color | Tint | Tint border | Text on tint |
|---|---|---|---|---|
| Success | `#3a7d44` | `#e9f2ea` | `#c5dcc9` | `#2d6336` |
| Info | `#2e6ca4` | `#e7eff6` | `#c2d6e6` | `#26597f` |
| Warning | `#b07d0a` | `#f7f0da` | `#e2d3a8` | `#8a6106` |
| Error | `#b3332b` | `#f7e9e7` | `#e3c2bd` | `#8f2822` |

Variables follow the state name: `--mcuhome-success`,
`--mcuhome-success-tint`, `--mcuhome-success-tint-border`,
`--mcuhome-success-on-tint`, and the same four for `info`, `warning` and
`error`.

Dark theme: there are no dark tints. Each state gets one brighter color that
carries text and border, placed on the normal surface color.

| State | Color (dark) |
|---|---|
| Success | `#5cab6b` |
| Info | `#6aa5d8` |
| Warning | `#d99a1e` |
| Error | `#dd6a5f` |

The dark mapping of the four tokens per state is therefore:

| Token | Dark value |
|---|---|
| `--mcuhome-<state>` | the bright color from the table above |
| `--mcuhome-<state>-tint` | `#292219` (the surface color) |
| `--mcuhome-<state>-tint-border` | the bright color |
| `--mcuhome-<state>-on-tint` | the bright color |

## Typography

| Role | Variable | Family |
|---|---|---|
| Wordmark and headings | `--mcuhome-font-heading` | Sora, weights 400 / 600 / 700 |
| Body text | `--mcuhome-font-body` | system sans stack |
| Code, YAML, terminal output | `--mcuhome-font-mono` | system monospace stack |

The wordmark is the plain text `MCUHome` set in Sora 700.

`style.css` declares the families but does not fetch Sora — it must not force a
network request on every consumer. Pages that want Sora load it themselves, for
example from Google Fonts:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&display=swap">
```

Without it, the heading font falls back to the system sans stack.

## Using style.css

Link the stylesheet and use the variables:

```html
<link rel="stylesheet" href="assets/mcuhome-brand/style.css">
```

```css
.card {
  background: var(--mcuhome-surface);
  border: 1px solid var(--mcuhome-border);
  color: var(--mcuhome-ink);
  font-family: var(--mcuhome-font-body);
}
```

`style.css` sets `color-scheme: light dark` on `:root`, so the tokens follow
the visitor's system preference out of the box. A page or a single subtree can
force a theme instead:

```css
:root { color-scheme: dark; }   /* or: light */
```

That rule has to be loaded after `style.css`, otherwise the stylesheet's own
`color-scheme: light dark` wins.

Both example pages in [`examples/`](examples) do exactly that — they are the
shortest complete demonstration of the tokens.

### Browser support

The tokens use the CSS
[`light-dark()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/light-dark)
function, supported since Chrome and Edge 123, Firefox 120, and Safari 17.5.
`light-dark()` needs `color-scheme` to be set — `style.css` does that itself on
`:root`. A browser that does not know the function still stores the tokens (a
custom property accepts any token sequence), but every property that uses one
becomes invalid at computed-value time and falls back to its inherited or
initial value; a `var()` fallback does not help there, because the variable is
defined. The three font tokens are unaffected.

### Releases

A tag publishes one archive, `mcuhome-brand-<version>.tar.zst`, attached to the
GitHub release of that tag; `<version>` is the tag without its leading `v`. Its
members sit at the archive root — `style.css`, the asset files, `LICENSE` and
`LICENSES/` — so it unpacks straight into a consuming project's asset folder:

```sh
mkdir -p assets/mcuhome-brand
tar --zstd -xf mcuhome-brand-<version>.tar.zst -C assets/mcuhome-brand
```

The `assets/` folder disappears in the process: `logo.svg` and `style.css` end
up side by side, which is what the paths in this guide assume. What this
repository needs for its own upkeep — this guide, the example pages, the
tooling — is not part of the archive.

## Development — how to work on this repository

This repository has its own virtual environment in `.venv/`; nothing is
installed into the system Python or into another repository's environment.
There is no Python source here — the environment exists so the lint wrappers
have their tools. `scripts/` holds the development tooling: `scripts/test` and
`scripts/lint` dispatch the checks — `all` runs every one, `list` names them,
`<name>` runs one — and each check is its own wrapper in `scripts/test.d/` or
`scripts/lint.d/`. The wrappers select `.venv` themselves (never activate one by
hand) and are exactly what CI runs, one job per check. There are no test
wrappers: this repository holds nothing to execute.

Needs Python ≥3.13 and a pip new enough for `--group` (25.1).

```sh
python3 -m venv .venv && .venv/bin/pip install -U pip && .venv/bin/pip install --group dev
```

```sh
scripts/test all
scripts/lint all
```

The rules that hold across every MCUHome repository — coding standards,
commits, licensing — are in the organization's
[contributing guide](https://github.com/mcu-home/.github/blob/main/CONTRIBUTING.md).

## License

Code, stylesheets, documentation and examples in this repository are licensed
under the Apache License 2.0; see [LICENSE](LICENSE).

The logo assets in `assets/` are excluded from that license. They are the
project's marks and are governed by the brand usage policy in
[LICENSES/LicenseRef-MCUHome-Brand.txt](LICENSES/LicenseRef-MCUHome-Brand.txt):
using the unmodified marks to refer to MCUHome is fine, using them as someone
else's identity or in modified form is not.
