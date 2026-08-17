# woo-website-assets

Central storage for the imagery (jumbotrons and favicons) of the Woo websites
running on [woo-website-template-apiv2](https://github.com/ConductionNL/woo-website-template-apiv2).

## Folder structure

The same category layout as [conduction-theme](https://github.com/ConductionNL/conduction-theme):

| Folder               | Contents                                    |
| -------------------- | ------------------------------------------- |
| `municipalities/`    | Municipalities (gemeenten)                  |
| `water-authorities/` | Water authorities (waterschappen)           |
| `partnerships/`      | Partnerships (samenwerkingsverbanden)       |
| `other/`             | Products, companies and demos (overig)      |

One folder per organisation, with fixed file names:

```
other/
└── openwoo/
    ├── jumbotron.webp
    ├── favicon.ico
    └── source/
        ├── origin.md    (who supplied which asset, and when)
        └── favicon.svg  (optional: source material)
```

## Conventions

- **Folder names** — the organisation's name in kebab-case: everything
  lowercase, every separate word joined with a hyphen, no diacritics
  (Gooise Meren → `gooise-meren`, Hof van Twente → `hof-van-twente`,
  Súdwest-Fryslân → `sudwest-fryslan`). This is the same kebab-case name
  the design-token themes are built on: theme packages — in
  [NL Design System](https://github.com/nl-design-system/themes) and
  conduction-theme alike — are named `<organisation>-design-tokens`
  (e.g. `gooise-meren-design-tokens`), so an organisation's folder here
  always matches its theme name. The name becomes part of a live URL, so it
  must be URL-safe.
- **Jumbotron** — fixed name `jumbotron.<ext>`. The template renders it with
  `background-size: cover`, so it is scaled to the full viewport width on any
  screen. Format preference, in order:
  1. `jumbotron.svg` — whenever the artwork is available as vector: stays
     sharp at every screen size;
  2. `jumbotron.webp` — preferred for everything raster. Organisations
     usually supply a PNG or JPG; converting to WebP
     ([Converting images](docs/converting-images.md))
     is pixel-identical for graphics (lossless mode), visually lossless for
     photos, and considerably smaller either way;
  3. `jumbotron.jpg` (photos) or `jumbotron.png` (flat graphics or
     transparency) — acceptable as supplied. One exception: a *photo* as PNG
     is many times larger than the same photo as JPEG or WebP with no visible
     benefit — always convert those.

  Supply raster images at least 1920 px wide (Full HD,
  the most common desktop width — anything narrower is upscaled even on a
  standard monitor), preferably 2560 px (covers QHD monitors and 4K screens
  at 150% display scaling; going wider roughly doubles the file size for a
  difference that is barely visible in a background photo).
- **Jumbotron size and composition** — the file in the organisation root is
  downloaded by every visitor on every page load, so keep it small: aim for
  **500 KB or less**. Slightly larger is acceptable — this is about a fast
  page for the visitor, not a hard limit. For the originals in `source/` the
  500 KB is only a recommendation: they are never served to visitors, and
  keeping the full-quality source is exactly what that folder is for.
  Keep the supplied original in the organisation's `source/` folder, under the
  same name as the root file it is the source of (like `source/favicon.svg`),
  and write the converted file to the organisation root — commands in
  [Converting images](docs/converting-images.md).

  Composition: the template renders a text card (title and subtitle) on top
  of the left-hand side of the image and crops with a fixed focal point (48%
  from the left, 39% from the top). Use wide, landscape images, keep the
  subject near the centre — the edges are cut off on narrow screens — and
  avoid essential details on the left, where the text card covers the image.
- **Provenance** — assets are supplied by, or approved by, the organisation
  itself; never use stock photos or images of unknown origin. Only when the
  organisation has not (yet) supplied an image may the jumbotron from the home
  page of the organisation's own website be used as a placeholder (see
  [License](#license)). Record the origin of every asset in the organisation's
  `source/origin.md`, so anyone can see at a glance whether an image was
  supplied by the organisation. Give each asset its own heading (`## Favicon`,
  `## Jumbotron`) and one bullet per file, labelled by its role: the
  **Original** (the supplied file in `source/` — who supplied it, and when)
  and the **Served file** (the file in the organisation root that the
  websites use). A file used exactly as supplied is simply the served file.
  Use a fixed phrasing — *supplied {date} by {organisation}*, *taken {date}
  from {website}*, *made {date} in-house by {team}* — and link websites:

  ```markdown
  # Origin

  ## Favicon

  - **Original** (`source/favicon.svg`) — made 2026-08-11 in-house by the
    Conduction frontend team (OpenWoo is a Conduction product).
  - **Served file** (`favicon.ico`) — generated by Conduction from the
    original.

  ## Jumbotron

  - **Served file** (`jumbotron.svg`) — made 2026-08-11 in-house by the
    Conduction frontend team, used as-is.
  ```
- **AI-generated imagery** — only for fictive demo organisations, never
  for real ones. Follow the
  [EU guidance on labelling AI-generated content](https://digital-strategy.ec.europa.eu/en/policies/eu-icons-labelling-ai-generated-content)
  (AI Act, Article 50): composite the EU "AI generated" icon — the official
  icons live in [`ai-labels/`](ai-labels/) — visibly into the served file,
  and keep the unmodified original in `source/`, as it carries the
  machine-readable AI provenance (content credentials), which conversion
  strips. Commands in [Converting images](docs/converting-images.md).
  Record the generation in `origin.md`.
- **Favicon** — **`favicon.ico` only**. It is the only format that works
  correctly in every browser (Safari does not support SVG favicons and
  JPEG/PNG support varies per browser) and it can hold multiple resolutions
  in a single file. Generating it from the source material:
  [Converting images](docs/converting-images.md).
- **Source material** — everything in an organisation's root is what configs
  reference; raw material and records are not mixed in with it. The `source/`
  folder holds the provenance record (`origin.md`) and the supplied
  originals: the favicon's SVG (`source/favicon.svg`) and the original of any
  converted or compressed jumbotron, so a conversion can always be redone
  from the source. Nothing in `source/` is ever referenced from a config.

## Adding a new organisation

Prefer clicking over command lines? There is a step-by-step guide for doing
all of this on github.com in the browser, no tools required:
[Adding images through the GitHub website](docs/adding-images-via-github.md).

1. Create a folder in the appropriate category, named after the organisation
   (lowercase, hyphens, no diacritics).
2. Add `jumbotron.webp` (or `.svg`/`.jpg`/`.png`) and `favicon.ico`.
3. Record in `source/origin.md` who supplied each asset and when — or, for a
   placeholder, which page it was taken from and on what date.
4. Reference the assets from the organisation's config file in the template.

## Usage

The files are referenced through raw URLs in the template's config files.
**Anything merged to `main` is live on the affected websites within minutes**
(the raw URLs are cached for about five minutes). Adding a new organisation is
harmless — nothing references its files yet — but be careful when **replacing
an existing organisation's files**: that changes a live website immediately,
so have someone glance at it before it lands. Never delete or rename a folder
that is still referenced from a config — the live site would lose its images
on the spot; update the configs first, then remove:

```json
"GATSBY_JUMBOTRON_IMAGE_URL": "https://raw.githubusercontent.com/ConductionNL/woo-website-assets/main/other/openwoo/jumbotron.webp",
"GATSBY_FAVICON_URL": "https://raw.githubusercontent.com/ConductionNL/woo-website-assets/main/other/openwoo/favicon.ico"
```

Thanks to the fixed file names the URL is fully predictable:

```
https://raw.githubusercontent.com/ConductionNL/woo-website-assets/main/<category>/<organisation>/<file>
```

Only the extension needs a glance at the folder. To copy a URL from the
GitHub UI without opening or downloading the file: open the file page and
choose **⋯ (kebab menu) → "Copy raw file URL"**.

## License

This repository is licensed under the [EUPL-1.2](LICENSE), in line with the
other OpenWoo.app repositories. Note: the images themselves contain the visual
identity of the organisation concerned; that identity remains the property of
that organisation and is managed here solely to render its own Woo website.

This also covers placeholders: when a Woo website is set up at an
organisation's request before it has supplied its own imagery, the jumbotron
from the home page of that organisation's own website is used as a temporary
placeholder. Such a placeholder equally contains the organisation's own visual
identity, is used exclusively for the Woo website that the organisation itself
commissioned, and is replaced or removed as soon as the organisation supplies
its own material or requests removal.

**Are you a rights holder** — for example a photographer or agency — and do
you believe an image in this repository is being used without the proper
permission? Every image here was either supplied by the organisation concerned
or is such a temporary placeholder, used only for as long as the organisation
has not supplied its own imagery. Reach out via
[conduction.nl/contact](https://www.conduction.nl/contact/) or open an issue
in this repository, and we will remove or replace the image promptly.
