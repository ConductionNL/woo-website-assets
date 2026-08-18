# Converting images

How to produce the served files (`jumbotron.webp`, `favicon.ico`) from the
originals in `source/`. Requires only [Node.js](https://nodejs.org) — `npx`
fetches the conversion tools
([sharp-cli](https://www.npmjs.com/package/sharp-cli) and
[icon-gen](https://www.npmjs.com/package/icon-gen)) on the fly, nothing is
installed system-wide. Run the commands in the organisation's folder.

## Jumbotron to WebP

Resizing, converting and compressing in one go (the output format follows
the output file's extension):

```
npx sharp-cli -i source/jumbotron.jpg -o jumbotron.webp -q 80 resize 2560        # photos
npx sharp-cli -i source/jumbotron.png -o jumbotron.webp --lossless resize 2560   # graphics, pixel-identical
```

Is the result still over 500 KB? Just lower `-q` until it fits — a jumbotron
is a background photo behind a text card, which hides compression artifacts
so well that even `-q 25` can look unchanged in practice.

## Generating the favicon.ico

**From an SVG source** (in `source/`) — the `favicon.ico` is written straight
into the organisation root:

```
npx icon-gen -i source/favicon.svg -o . --ico --ico-name favicon --ico-sizes 16,32,48
```

**From a PNG or JPG source** — icon-gen only accepts SVG input, so first
create the three sizes, then pack them (the last command removes the
temporary `icons/` folder again):

```
mkdir icons
npx sharp-cli -i source/favicon.png -o icons/16.png resize 16 16
npx sharp-cli -i source/favicon.png -o icons/32.png resize 32 32
npx sharp-cli -i source/favicon.png -o icons/48.png resize 48 48
npx icon-gen -i icons -o . --ico --ico-name favicon --ico-sizes 16,32,48
npx rimraf icons
```

For a JPG original, change only the input: `source/favicon.png` becomes
`source/favicon.jpg` in the three resize commands. The output files must
stay `icons/16.png`, `icons/32.png` and `icons/48.png` — icon-gen can only
pack PNGs, so `.jpg` outputs make it error. Note that JPG cannot hold
transparency, so the icon keeps its solid background — if it should be
transparent, ask for a PNG or SVG original instead.

Both routes produce a single `favicon.ico` containing 16/32/48 px variants
while preserving transparency (when the source has it).

## Labelling AI-generated images

AI-generated imagery (only used for fictive demo organisations) must carry
the EU "AI generated" label visibly in the served file — the conversion to
WebP strips the machine-readable AI provenance (C2PA content credentials)
that generators embed, so keep the unmodified original in `source/` and
bake the visible label into the conversion. The official EU icons live in
[`ai-labels/`](../ai-labels/) at the repository root.

Scale the label to roughly a tenth of the image width, then composite it
during conversion — the same command also writes a machine-readable AI
declaration into the served file's EXIF metadata (the last command removes
the temporary scaled label again). The example coordinates fit a
1920 × 819 image (label top-left at 80% of the width, 46% of the height) —
scale them along with your image:

```
npx sharp-cli -i ../../ai-labels/label-ai-generated-black-transparent.png -o label-small.png resize 192
npx sharp-cli -i source/jumbotron.png -o jumbotron.webp -q 80 --metadata.exif.IFD0.ImageDescription "AI-generated image (EU AI Act, Article 50); IPTC digital source type: trainedAlgorithmicMedia" composite label-small.png --left 1540 --top 375
npx rimraf label-small.png
```

Place the label at mid-height, never in a corner or against an edge. The
template crops the image around a fixed focal point (48% from the left,
39% from the top — see the composition rules in the README): wide screens
show the full image width but cut off the top and bottom — depending on
the screen, only roughly 29–54% of the image height always stays in view —
while narrow screens cut off the sides instead. So keep the label's
vertical centre around 40–50% of the image height, and pick a horizontally
calm spot that keeps clear of the text card on the left; the right half of
the image works well, since wide screens always show the full width. On
phone-sized screens the sides are cropped away and the text card covers
most of what remains, so no placement is phone-proof — desktop visibility
is the priority.

## Alternatives

[ImageMagick](https://imagemagick.org) (a separate install —
`magick -density 384 -background none favicon.svg -define icon:auto-resize=48,32,16 favicon.ico`,
which also accepts a `.png`/`.jpg` as input without the `-density` and
`-background` flags) or, without any tooling, in the browser:
<https://realfavicongenerator.net> for favicons and
<https://squoosh.app> for WebP conversion.
