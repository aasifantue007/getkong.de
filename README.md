# GETKONG — Webflow Custom Code

All custom code for the three GETKONG Webflow sites — the site-wide Head/Footer
code and the registered scripts injected on every page. This is code that lives
**outside** the visual Designer; if you edit any of it in Webflow, please mirror
the change here.

| Site | Webflow site ID | Where in this repo |
|---|---|---|
| **getkong.de** (main) | `68c9321bb07942194167c67c` | repo root |
| **getkong.delivery** (Meta ads, noindex) | `68dea6e3c920a90256d4bdfd` | `getkong.delivery/` |
| **getkong.shop** (Reddit ads, noindex) | `68fa5985abdbd1625e6d5a81` | `getkong.shop/` |

---

## ⚠️ Read this first — the #1 gotcha

**All of this code runs only on the *published* site — never inside the Webflow
Designer canvas.** So in the Designer you'll see "broken" styling that is
actually fine live. Most visibly, headings render in a fallback font (Arial)
instead of **Switzer Bold Italic**, and the interactive bits (transparent
header, hover states, cloud parallax, counters, testimonial slider, Trustpilot
widget) don't preview. Always verify changes on **staging or the live site**,
not the canvas.

---

## Repository layout

```
/                     getkong.de (main site)
  README.md
  head.html           → Project Settings → Custom Code → Head Code
  footer.html         → Project Settings → Custom Code → Footer Code
  gk*.js              → 13 registered scripts applied site-wide
getkong.delivery/     getkong.delivery (Meta ads landing site)
  head.html
  footer.html
  gkfont2.js
getkong.shop/         getkong.shop (Reddit ads landing site)
  head.html
  footer.html
  gkfont2.js
```

---

## getkong.de (repo root)

Two systems hold this site's code:

1. **Site-wide Custom Code** — `head.html` and `footer.html`, pasted into
   Project Settings → Custom Code (Head + Footer). Analytics, consent, schema,
   and the homepage interactions.
2. **Registered scripts** — the `gk*.js` files, registered against the site and
   applied on every page. Mostly targeted CSS-injection fixes.

**Registered scripts (13 applied):**

| File | Load | What it does |
|---|---|---|
| `gkfontv4.js` | header | Loads **Switzer** (Fontshare) + **Inter** and forces headings to Switzer *Bold Italic*, body to Inter. The type system. |
| `gkhdrscroll3.js` | header | Forces the header (`.gk-nav`) fully transparent in every state — kills the old green bar / shadow on scroll. |
| `gkstdnav3.js` | header | Header layout: centered logo, right-aligned nav, legal-page variant, scroll + mobile sizing. |
| `gkresp3.js` | header | Header responsive tweaks 768–1599px (spacing, WhatsApp button → icon). |
| `gkmobpad.js` | header | Min-height 80px on the mobile header. |
| `gkbtn.js` | header | `white-space:nowrap` on all button classes so labels never wrap. |
| `gkbtnhover.js` | header | Outline process CTA (`.gk-proc-cta`) text turns brand lime `#C2E67E` on hover. |
| `gkblogresp2.js` | header | Blog listing grid responsive (2-up / 1-up). |
| `gkblogshell8.js` | header | Blog **article** mobile fixes — overflow, word-break, code/table/image containment (≤991px). |
| `gkmobile.js` | footer | Builds the **mobile menu**: burger, slide-in panel, bottom WhatsApp + CTA bar (≤767px). |
| `gktable.js` | footer | Turns `<table>` markup in blog rich-text into styled `.gk-tbl` tables. |
| `gkapolink3.js` | footer | Rewrites old Apotheken URLs → `/apotheken`. |
| `gkoldhome.js` | footer | Rewrites old logo / `/old-home` links → `/`. |

> The site has ~39 registered scripts total, but only these 13 are applied. The
> rest are superseded iterations — safe to delete from the site.

**`head.html`:** cookie-consent receiver → GTM (`GTM-MQ6ZKHHR`) → A/B testing
(drip-apex) → PostHog → UTM → Organization + WebSite JSON-LD → testimonial
slider → FAQ accordion → sticky header → tablet responsive → mobile lab cloud.

**`footer.html`:** blocks A–J — transparent header, scroll-highlighted process
cards, cloud/veggie/logo parallax, stat counters, scroll reveals, CTA hover,
logo→home, PLZ row, trust-badge marquee, Trustpilot widget loader, GTM
`<noscript>`, mobile-menu close. **Note:** the A–J block is pasted twice on the
live site (preserved as-is; safe to de-dupe after testing).

---

## getkong.delivery & getkong.shop (ad landing sites)

These two sites are **near-identical** to each other. Each is a single Blüte
landing page used for paid ads, set to `noindex`, with a Twitter/X conversion
pixel (`twq('config','qo20f')`). Each folder contains:

- **`gkfont2.js`** — the same Switzer + Inter font loader as getkong.de's `gkfontv4.js` (the one registered script applied to each site).
- **`head.html`** — tracking (GTM, drip-apex A/B, PostHog, UTM, Twitter pixel, TrustBox) + `noindex` + the testimonial-slider behaviour script.
- **`footer.html`** — the landing-page interactions: cloud parallax, sticky/shrinking nav, floating WhatsApp button + mobile action bar, trust-badge marquee, mobile hero sizing, process cards, FAQ + city accordions, testimonial avatar injection, Trustpilot loader, and a guarded `.bl-cloud` scroll-drift script.

`getkong.delivery/` and `getkong.shop/` are byte-identical apart from the
site-name comment header.

---

## External files referenced (hosted outside Webflow, not in this repo)

- `https://cdn.getkong.de/js/load-posthog.js`
- `https://cdn.getkong.de/js/load-utm.js`
- `https://events.drip-apex.com/...` (A/B testing)
- GTM container `GTM-MQ6ZKHHR`
- Trustpilot business-unit `68495fb031ed1d653c6b58bf`
- Twitter/X pixel `qo20f` (delivery + shop)

---

## Not included here

Site-wide Head/Footer code and the applied registered scripts are covered. Any
**page-level** custom code (pasted into a single page's Page Settings) is not
captured — if you find page-specific behaviour not explained above, check that
page's own settings.

---

## Also relevant (not code, but easy to break)

On **getkong.de**, the location pages (`/apotheken/<city>`, `/aerzte/<city>`)
use CMS Collection List filters set to the **current city**, so each page shows
only that city's pharmacies and FAQs. Live cities: Berlin, Frankfurt, Hamburg,
Köln, München. If you rebuild those templates, keep the current-city filter on
every list or they revert to showing all cities.
