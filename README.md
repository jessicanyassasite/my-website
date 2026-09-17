# Jessica Nyassa — historian's website

Static HTML. No build step, no framework, no dependencies beyond three Google Fonts.
Open `index.html` in a browser and it works.

## Pages

| File | Page |
| --- | --- |
| `index.html` | Home — hero, approach, areas of focus, services index, about teaser |
| `about.html` | About — biography, principal research, method, education |
| `services.html` | Services — the eight services, each with its own anchor |
| `contact.html` | Contact — enquiry form and details |

Navigation is three items: About, Services, Contact. The wordmark top-left goes home.

`assets/css/style.css` is the whole design system. `assets/js/main.js` does one thing:
open and close the mobile menu.

`_archive/` holds the earlier Selected Work and Publications pages and the old SVG
placeholder images. Nothing links to them. Delete the folder whenever you like, or bring
a page back by copying its header and footer from one of the four live pages.

## Before this goes live

Five things need real values. Search for them with:

```sh
grep -on '\[[^]]*\]' *.html
```

1. **Email address** — `hello@jessicanyassa.com` is a guess. It appears in the footer of
   every page and in the contact page details list. `grep -rn 'jessicanyassa.com' *.html`
2. **Location** — "London, United Kingdom" in the same two places. Change or delete.
3. **Photo credit** — `[credit]` in the footer of every page. Delete the whole line if the
   photographs don't need crediting.
4. **Portrait caption** — `[Caption — where the portrait was taken.]` on `about.html`.
   Delete the `<figcaption>` if no caption is wanted.
5. **Principal research photograph** — `sandstone` stands in on `about.html`. It was chosen
   because it carries no European or Roman signifiers, not because it illustrates West
   Africa. Swap it for a relevant photograph when one exists.

The contact form posts nowhere (`action="#"`). To make it live, sign up for a form service
and paste the endpoint in:

```html
<form class="form" method="POST" action="https://formspree.io/f/XXXXXXX">
```

Formspree, Basin, Netlify Forms and Cloudflare Pages Forms all work with this markup
unchanged. The hidden "website" field is a spam trap — most services will honour it if you
name it as a honeypot in their settings. There is no JavaScript involved, so the form
degrades to a normal page POST.

## Social links

Substack, Instagram and TikTok are in the footer of every page and in the details list on
the contact page — all under the handle `@jessthehistorian`. The tracking parameters
(`?r=`, `utm_*`, `_t=`, `stkn=`) were stripped from the URLs she sent; they identify the
device the link was shared from and don't belong in a public page. If she adds a platform,
it goes in the "Elsewhere" list in all four footers and in `contact.html`.

The marks beside them are in `assets/img/social/`, in two tones: bone (`substack.png`) for
the dark footer, and ink (`substack-ink.png`) for light backgrounds. They were derived from
the PNGs in `images/social-source/`. Those originals were inconsistent — Substack arrived as
a bare white glyph, while Instagram and TikTok were white rounded-square plates with the
glyph knocked out of them, so side by side two would have read as solid app tiles and one as
a thin mark. Each was reduced to its glyph alone and scaled to a common height, which is
also how Instagram and TikTok's own brand guidelines expect a monochrome mark to be used.

They are `<img>` elements with `alt=""`, because the visible label next to each one already
names the platform — an alt text would make a screen reader announce it twice.

The service dropdown on that form lists the eight services plus "Other", matching
`services.html` exactly. If a service is renamed, rename it in three places: the dropdown,
the services page heading, and the index list on the home page.

## Photographs

Originals live in `images/`. Web-ready versions are generated into
`assets/img/photos/` — each one at full width (1366px) and at 800px, lightly desaturated
and contrast-lifted so the set reads as one body of work against the palette.

| File | Original | Used on |
| --- | --- | --- |
| `hero-nave` | Jess20 | Home hero |
| `portrait` | Jess23 | About portrait, home about teaser |
| `band-stones` | Jess4 | Quote bands on home and services |
| `nave-wide` | Jess21 | Quote band on about |
| `stalls` | Jess14 | Services 02 |
| `gilded` | Jess8 | Services 04 |
| `choir` | Jess10 | Services 06 |
| `ruins` | Jess6 | Services 08 |
| `sandstone` | Jess17 | About — principal research |
| `profile` | Jess15 | Contact |
| `bust`, `forum` | jess1, Jess25 | Spare — processed but unused |

`Jess22` was left out: it needs the other people cropped out first.

To regenerate or add one, the recipe is a few lines of Python (Pillow): open, optionally
crop, `ImageEnhance.Color` around 0.8–0.9, `ImageEnhance.Contrast` at 1.06, resize to
1366 and 800 wide, save as progressive JPEG at quality 82.

Images are wired up with `srcset`, so phones download the 800px file. If you swap a photo,
keep both sizes or remove the `srcset` attribute.

## Design notes

- **Type** — Bodoni Moda for display (the name, headings, pull quotes, the numerals),
  Source Serif 4 for body text, Inter for navigation, labels, captions and buttons.
  Reading columns are capped at ~65 characters (`--measure`).
- **Why the display type is set the way it is** — Bodoni is a high-contrast face, and left
  to itself its hairlines all but vanish at large sizes, especially reversed out of the
  dark panels. Two things hold it together: display text is set at weight 600 rather than
  400/500, and the variable font's optical-size axis is pinned low with
  `font-optical-sizing: none; font-variation-settings: var(--display-opsz);` — Bodoni draws
  sturdier hairlines at small optical sizes, so pinning the axis at 14 gives a 120px
  heading the robustness of a small one. Change `--display-opsz` in `:root` to trade
  elegance against weight: lower is sturdier, higher is finer.
- **Colour** — near-black `#16130F` for the masthead, hero, footer and alternating
  sections; bone `#F4F0E8` and parchment `#EAE4D8` for the light ones; dark burgundy
  `#5E1F27` for buttons, links and the closing call to action; brass `#A98A52` for
  hairlines, small labels and numerals only. To shift the whole site, change `--ink`,
  `--oxblood` and `--brass` in the `:root` block.
- **Motion** — hover states and the mobile menu. Nothing else. The stylesheet honours
  `prefers-reduced-motion`.
- **Accessibility** — skip link, visible focus rings, labelled form controls,
  `aria-current` on the active nav item, decorative images with empty `alt`, and text that
  passes WCAG AA against every background used.
- **Repeated chrome** — the header and footer are copied into each HTML file, which is the
  trade-off for having no build step. If you change one, change them all;
  `grep -l wordmark__name *.html` lists them.

## Publishing

Any static host: Netlify, Cloudflare Pages, GitHub Pages, or plain FTP. Drop the whole
folder in — `images/` and `_archive/` can be left out. No configuration needed.
`jessicanyassa.com` was showing as available.
