# GETKONG — Webflow Custom Code

All custom code currently applied to the GETKONG Webflow site
(`getkong.de`, site ID `68c9321bb07942194167c67c`).

This repo is the source of truth for code that lives **outside** the visual
Designer — the site‑wide Head/Footer code and the registered scripts injected
on every page. If you edit any of it in Webflow, please mirror the change here.

---

## ⚠️ Read this first — the #1 gotcha

**All of this code runs only on the *published* site — never inside the Webflow
Designer canvas.** So in the Designer you'll see "broken" styling that is
actually fine live. Most visibly:

- Headings render in a fallback font (Arial) instead of **Switzer Bold Italic**.
- The transparent header, hover states, cloud parallax, counters, testimonial
  slider and Trustpilot widget don't preview.

Always verify changes on **staging or the live site**, not the canvas.

---

## Files in this repo

Everything is at the repo root:

**Site‑wide Custom Code** — pasted into Project Settings → Custom Code:
- `head.html`   → Head Code
- `footer.html` → Footer Code

**Registered scripts** — small JS files registered against the site and applied
on every page via the Apps / Custom‑code panel (mostly targeted CSS‑injection
fixes). The 13 files below (`gk*.js`) are the ones currently applied.

---

## Registered scripts (applied site‑wide)

Load = where Webflow injects the script (header or footer).

| File | Load | What it does |
|---|---|---|
| `gkfontv4.js` | header | Loads **Switzer** (Fontshare) + **Inter** (Google) and forces headings to Switzer *Bold Italic*, body to Inter. The whole type system. |
| `gkhdrscroll3.js` | header | Forces the header (`.gk-nav`) fully transparent in every state — kills the old green bar / shadow on scroll. |
| `gkstdnav3.js` | header | Header layout: centered logo, right‑aligned nav, legal‑page variant (`:has(.gk-legal)`), scroll + mobile sizing. |
| `gkresp3.js` | header | Header responsive tweaks for 768–1599px (spacing, WhatsApp button collapses to icon). |
| `gkmobpad.js` | header | Min‑height 80px on the mobile header. |
| `gkbtn.js` | header | Forces `white-space:nowrap` on all button classes so labels never wrap. |
| `gkbtnhover.js` | header | Outline process CTA (`.gk-proc-cta`) text turns brand lime `#C2E67E` on hover. |
| `gkblogresp2.js` | header | Blog listing grid responsive (2‑up / 1‑up, card + image fit). |
| `gkblogshell8.js` | header | Blog **article** mobile fixes — overflow, word‑break, code/table/image containment (≤991px). |
| `gkmobile.js` | footer | Builds the **mobile menu**: burger, slide‑in panel, and bottom WhatsApp + CTA bar (≤767px). |
| `gktable.js` | footer | Turns `<table>` markup pasted into blog rich‑text into styled `.gk-tbl` tables. |
| `gkapolink3.js` | footer | Rewrites old Apotheken URLs (`/apothekenverzeichnis`, `/apotheken-new`) → `/apotheken`. |
| `gkoldhome.js` | footer | Rewrites old logo / `/old-home` links → `/`. |

> **Cleanup note:** the site has ~39 registered scripts in total, but only these
> 13 are applied. The rest are superseded iterations (`gkhdrscroll`/`gkhdrscroll2`,
> `gkfont`…`gkfontv3`, `gkblogshell`…`gkblogshell7`, `gkstdnav`/`gkstdnav2`,
> `gkresp`/`gkresp2`, `gkapolink`/`gkapolink2`, etc.) left registered but unused —
> safe to delete from the site. Only the 13 above matter.

---

## `head.html` — Site‑wide Head code

In order: cross‑domain cookie‑consent receiver → Finsweet Attributes →
**Google Tag Manager** (`GTM-MQ6ZKHHR`) → A/B testing (drip‑apex) → **PostHog**
loader → **UTM** tooling → Organization + WebSite **JSON‑LD schema** →
homepage **testimonial slider** (12 reviews, edit inline) → **FAQ accordion** →
**sticky header** styles → tablet‑portrait responsive → mobile lab‑section cloud.

## `footer.html` — Site‑wide Footer code

Blocks **A–J**: (A) transparent header, (B) scroll‑highlighted process cards,
(C) cloud + veggie + logo scroll parallax, (D) animated stat counters,
(E) section scroll reveals, (F) CTA arrow hover, (G) logo → home,
(H) PLZ CTA row layout, (I) trust‑badge marquee, (J) **live Trustpilot widget**
loader — plus the GTM `<noscript>` and a mobile‑menu‑close helper.

> **Note — the footer contains the A–J block twice.** That is the actual live
> state (the block was pasted a second time under the "SITE‑WIDE FOOTER CODE"
> comment). Preserved here as‑is; de‑duplicating it is a safe cleanup, but test
> the homepage after.

---

## External files referenced (hosted outside Webflow, not in this repo)

Loaded by `head.html` but stored elsewhere:

- `https://cdn.getkong.de/js/load-posthog.js`
- `https://cdn.getkong.de/js/load-utm.js`
- `https://events.drip-apex.com/...` (A/B testing)
- GTM container `GTM-MQ6ZKHHR`
- Trustpilot business‑unit `68495fb031ed1d653c6b58bf`

---

## Not included here

**Site‑wide** Head/Footer code and the applied registered scripts are covered.
Any **page‑level** custom code (pasted into a single page's Page Settings) is not
captured — if you find page‑specific behaviour not explained above, check that
page's own settings.

---

## Also relevant (not code, but easy to break)

**Location pages** (`/apotheken/<city>`, `/aerzte/<city>`) use CMS Collection
List filters set to the **current city**, so each page shows only that city's
pharmacies and FAQs. Live cities: Berlin, Frankfurt, Hamburg, Köln, München.
If you rebuild those templates, keep the current‑city filter on every list or
they revert to showing all cities.
