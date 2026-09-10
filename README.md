# Unique Holidays — travel agency website

A fast, dependency-free static website for **Unique Holidays** (*Let's Explore the World*),
built around the brand logo. Plain HTML, CSS and vanilla JavaScript — no build step, no
framework, no npm install. Open `index.html` and it runs.

## Pages

| File | What's on it |
| --- | --- |
| `index.html` | Hero, trip-search widget, featured destinations, best-selling packages, why-us, stats, how-it-works, testimonial slider, CTA |
| `destinations.html` | All 18 destinations with live search + India / International filters |
| `packages.html` | All 18 tour packages with category filters, what's-included, FAQ accordion |
| `about.html` | Story, values, animated stats, team |
| `contact.html` | Enquiry form with validation, contact cards, map slot, WhatsApp/call CTAs |

## Brand

Colours are sampled straight from the uploaded logo and live as CSS custom properties
at the top of `assets/css/styles.css`:

| Token | Value | Used for |
| --- | --- | --- |
| `--navy` | `#002352` | Headings, footer, deep gradients |
| `--blue` | `#0080D8` | Links, primary brand gradient |
| `--sky` | `#29CFFD` | Accents, gradient highlights |
| `--orange` | `#FE9D38` | Primary CTAs, prices, highlights |

Fonts are **Poppins** (headings) and **Inter** (body), loaded from Google Fonts with a
system-font fallback stack.

## Artwork

Out of the box there are no stock photos and no external image requests. Every
destination image is a hand-generated flat-vector SVG in `assets/img/destinations/`,
drawn in the same illustration style as the logo (18 scenes: beaches, mountains, snow,
city skylines, heritage domes, karsts, desert dunes, hot-air balloons and backwaters).

These are placeholders. See **Using real photographs** below to swap in real,
licensed photos with one command — the overlay scrims and text shadows are already
tuned so captions and prices stay legible over busy photography.

Logo assets were derived from the uploaded PNGs: background removed, trimmed, and
exported as `logo-mark.png`, `logo-full.png`, favicons and `og-image.png`.

## Motion layer

`assets/css/motion.css` + `assets/js/motion.js` add the scroll experience. No GSAP,
no Lenis, no ScrollTrigger — it is plain rAF and `IntersectionObserver`, so there is
nothing to install and nothing to load from a CDN.

| Effect | How it works |
| --- | --- |
| **Intro curtain** | Navy overlay with the logo, a 0→100 counter and a progress bar, then the screen splits in two and parts sideways to reveal the page. Full length on the first visit of a session, a short version after that (`sessionStorage`). |
| **Scroll meter** | Fixed left-hand percentage readout (00–100) with a vertical progress bar, updated every frame. |
| **Headline reveals** | Any `data-split` heading is broken into words, each wrapped in an overflow mask, and the words rise into place on a 55ms stagger. |
| **Section reveals** | `data-anim="fade-up | fade-left | scale-in | clip-up | clip-side"`, with `data-stagger="90"` on a container to cascade its children. |
| **Parallax** | `data-parallax="0.35"` drifts an element against the scroll. |
| **Pinned horizontal gallery** | The destinations section sticks to the viewport while the card track scrubs sideways — vertical scroll distance is measured to match the track width exactly, so it maps 1:1 and re-measures on resize. |
| **Day → night set-piece** | A pinned section publishes its scroll progress as a CSS variable (`--p`), which cross-fades the scene from day to night and drives the dial. |
| **Day/night mode** | The switch in the header repaints the whole site through token overrides on `:root[data-theme="night"]`, and the choice is remembered. `assets/js/boot.js` applies it before first paint so there is no flash. |
| **Ambient sky** | Three blurred cloud bands drift across the hero on 190–340s loops, and the collage images breathe on a slow scale. Atmosphere, not decoration — they never pull focus from the copy. |
| **Interlude** | A full-bleed chapter panel between the gallery and the packages, with parallax driven by `--p` and gradients that blend it into the pale sections above and below, so the page reads as one continuous scene. |
| **Nightfall** | The page opens in daylight, the day/night set-piece hands over to dusk, and the footer lands on a night sky with faint stars and an occasional shooting star. |
| **Page transitions** | Internal links fade out before navigating so the next page's curtain starts from black instead of a white flash. External links, downloads, in-page anchors and modified clicks are left alone, and a bfcache restore clears the fade. |

Every one of these is disabled under `prefers-reduced-motion: reduce`: the loader is
removed, the pin is released, and all content renders immediately.

To tune it, the numbers worth touching are the loader `duration` in `motion.js`, the
word stagger in `splitWords()`, and the card height in `.hscroll__item`.

## Using real photographs

The site ships with the hand-drawn SVG scenes so it looks finished out of the box,
but it is built to take real photos. `tools/fetch-photos.py` pulls one for each
destination from **Wikimedia Commons** — no API key, and every file's author and
licence is recorded for you:

```bash
python3 tools/fetch-photos.py --dry-run    # see what it will search for
python3 tools/fetch-photos.py --apply      # download, then point the pages at them
```

`--apply` swaps `destinations/<slug>.svg` for `destinations/<slug>.jpg` across all
five pages, updates each `<img>`'s `width`/`height` to the real dimensions so nothing
shifts on load, and rewrites the alt text from "Illustration of Goa" to "Goa". It
writes `assets/img/destinations/CREDITS.md` with the author and licence per photo —
keep that file if you publish the site.

Useful flags:

| Flag | What it does |
| --- | --- |
| `--only goa,bali` | just those destinations |
| `--force` | re-download files that already exist |
| `--skip-download` | use photos already sitting in the folder |
| `--dry-run` | print the search terms and stop |

**Using your own photos instead** — this is the path to take when you have real
photography of your own trips:

```bash
# save them as assets/img/destinations/<slug>.jpg, then:
python3 tools/fetch-photos.py --apply --skip-download
```

The slugs are the eighteen in `QUERIES` at the top of the script: `kashmir`, `kerala`,
`goa`, `ladakh`, `rajasthan`, `andaman`, `himachal`, `meghalaya`, `dubai`, `maldives`,
`bali`, `thailand`, `singapore`, `switzerland`, `vietnam`, `turkey`, `europe`,
`srilanka`. Landscape shots around 1600px wide work best — the cards crop to fill, and
the text sits over the bottom third, so avoid photos with important detail down there.

If a search returns something unsuitable, edit that destination's term in `QUERIES`
and re-run with `--only <slug> --force`. Nothing is destructive: the SVGs stay on
disk, so `git checkout -- *.html` puts the illustrations back.

If you would rather use Unsplash or Pexels, download the files by hand into the same
folder with the same names and run the `--skip-download` command above — but check
each service's licence terms before publishing commercially.

## Running it

```bash
# any static server works
npx http-server -p 8080 .
# or
python3 -m http.server 8080
```

Then open <http://localhost:8080>.

Deploying is a straight file copy — GitHub Pages, Netlify, Vercel, Cloudflare Pages or
any shared host will serve it as-is.

## Before you go live

Everything below is **placeholder content** and should be replaced with the real thing.

1. **Contact details** — phone, email and address appear in every page footer, in the
   header, and on `contact.html`. Search all five HTML files for these strings and replace:
   - `+91 98765 43210` and `+91 98765 43211`
   - `hello@uniqueholidays.com`, `bookings@uniqueholidays.com`
   - `Unique Holidays Travel Desk`, `2nd Floor, Sunrise Arcade, Ring Road`, `Surat, Gujarat 395002, India`
   - the WhatsApp number in every `https://wa.me/919876543210` link
2. **Photographs** — the destination images are illustrations, not photos of the
   real places. Run `tools/fetch-photos.py --apply` (see above) or drop your own in.
3. **Figures and claims** — the counters (`12,000+ travellers`, `40+ countries`,
   `11 yrs`, `4.8★`) and every package price, rating and review count are illustrative.
   Replace them with your real numbers before publishing.
4. **Testimonials and team** — the four reviews on the home page and the four names on
   `about.html` are placeholders. Use real, permissioned quotes and real staff names.
5. **Social links** — the footer icons point at `#`. Add your real profile URLs.
6. **Map** — `contact.html` has a `<div class="map-embed">` with a `TODO` comment.
   Paste your Google Maps embed `<iframe>` in there.
7. **Domain** — `index.html` and the other pages carry `https://www.uniqueholidays.com/`
   in `<link rel="canonical">` and the structured data. Point these at your real domain.

## Connecting the forms

Both forms are validated client-side and **do not submit anywhere yet** — the enquiry
form shows a success panel and resets. To make them live, pick one:

- **Form service (no backend):** add an `action` and `method` to the `<form>` in
  `contact.html` pointing at Formspree / Web3Forms / Getform, and remove the
  `e.preventDefault()` success branch in `assets/js/main.js` (the block under
  `/* ---- enquiry form ---- */`).
- **Your own endpoint:** replace the same branch with a `fetch()` POST to your API.

The newsletter form in the footer works the same way (`#newsletterForm` in `main.js`).

## Accessibility & performance notes

- Skip link, visible focus rings, `aria-current` on the active nav item, labelled form
  fields with inline error messages, and `aria-expanded` on the menu and accordion.
- `prefers-reduced-motion` disables reveals, counters and the slider's motion.
- All images carry explicit `width`/`height` and `loading="lazy"` below the fold, so
  there is no layout shift.
- Total page weight is a few hundred KB, mostly the SVG artwork.

## Structure

```
index.html  destinations.html  packages.html  about.html  contact.html
site.webmanifest
tools/
  fetch-photos.py       swap the illustrations for real licensed photos
assets/
  css/styles.css        design tokens + all components
  css/motion.css        loader, scroll meter, reveals, pinned gallery, night mode
  js/boot.js            pre-paint theme + scroll lock (loaded synchronously)
  js/main.js            nav, counters, slider, filters, accordion, forms
  js/motion.js          intro, scroll meter, splits, parallax, pinning, day/night
  img/
    logo-mark.png  logo-full.png  swoosh.svg  og-image.png
    favicon.ico  apple-touch-icon.png  icon-192.png  icon-512.png
    destinations/*.svg  (18 scenes)
```
