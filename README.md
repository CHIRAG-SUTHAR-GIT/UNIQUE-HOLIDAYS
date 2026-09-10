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

There are no stock photos and no external image requests. Every destination image is a
hand-generated flat-vector SVG in `assets/img/destinations/`, drawn in the same
illustration style as the logo (18 scenes: beaches, mountains, snow, city skylines,
heritage domes, karsts, desert dunes, hot-air balloons and backwaters).

Logo assets were derived from the uploaded PNGs: background removed, trimmed, and
exported as `logo-mark.png`, `logo-full.png`, favicons and `og-image.png`.

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
2. **Figures and claims** — the counters (`12,000+ travellers`, `40+ countries`,
   `11 yrs`, `4.8★`) and every package price, rating and review count are illustrative.
   Replace them with your real numbers before publishing.
3. **Testimonials and team** — the four reviews on the home page and the four names on
   `about.html` are placeholders. Use real, permissioned quotes and real staff names.
4. **Social links** — the footer icons point at `#`. Add your real profile URLs.
5. **Map** — `contact.html` has a `<div class="map-embed">` with a `TODO` comment.
   Paste your Google Maps embed `<iframe>` in there.
6. **Domain** — `index.html` and the other pages carry `https://www.uniqueholidays.com/`
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
assets/
  css/styles.css        design tokens + all components
  js/main.js            nav, reveals, counters, slider, filters, accordion, forms
  img/
    logo-mark.png  logo-full.png  swoosh.svg  og-image.png
    favicon.ico  apple-touch-icon.png  icon-192.png  icon-512.png
    destinations/*.svg  (18 scenes)
```
