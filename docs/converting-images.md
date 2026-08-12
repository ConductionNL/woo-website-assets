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

## Generating the favicon.ico

**From an SVG source** (in `source/`) — the `favicon.ico` is written straight
into the organisation root:

```
npx icon-gen -i source/favicon.svg -o . --ico --ico-name favicon --ico-sizes 16,32,48
```

**From a PNG or JPG source** — icon-gen only accepts SVG input, so first
create the three sizes, then pack them (delete the temporary `icons/` folder
afterwards):

```
mkdir icons
npx sharp-cli -i source/favicon.png -o icons/16.png resize 16 16
npx sharp-cli -i source/favicon.png -o icons/32.png resize 32 32
npx sharp-cli -i source/favicon.png -o icons/48.png resize 48 48
npx icon-gen -i icons -o . --ico --ico-name favicon --ico-sizes 16,32,48
```

Both produce a single `favicon.ico` containing 16/32/48 px variants while
preserving transparency.

## Alternatives

[ImageMagick](https://imagemagick.org) (a separate install —
`magick -density 384 -background none favicon.svg -define icon:auto-resize=48,32,16 favicon.ico`,
which also accepts a `.png`/`.jpg` as input without the `-density` and
`-background` flags) or, without any tooling, in the browser:
<https://realfavicongenerator.net> for favicons and
<https://squoosh.app> for WebP conversion.
