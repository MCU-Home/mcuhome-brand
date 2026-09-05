# mcuhome-brand

The MCUHome corporate identity: logo assets, color tokens, and the design
guide.

The full design guide and `style.css` follow; this repository currently holds
the logo assets.

## Assets

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

All four SVGs share one geometry on a `0 0 64 64` viewBox; the light and dark
variants differ only in the color of the pins. The door is a real transparent
cutout (an SVG mask), so the mark works on any background.

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

At 16 px the two simplified wave arcs are close enough that they read as one
merged band with a lighter waist rather than two clearly separate arcs; 32 px
and 48 px show both arcs separately.

## License

Code, stylesheets, documentation and examples in this repository are licensed
under the Apache License 2.0; see [LICENSE](LICENSE).

The logo assets in `assets/` are excluded from that license. They are the
project's marks and are governed by the brand usage policy in
[LICENSES/LicenseRef-MCUHome-Brand.txt](LICENSES/LicenseRef-MCUHome-Brand.txt):
using the unmodified marks to refer to MCUHome is fine, using them as someone
else's identity or in modified form is not.
