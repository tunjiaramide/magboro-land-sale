# Ivory Garden Estate land listing — project notes

## What this is
A single-page site (`index.html`) to replace repetitive Facebook replies when selling the
300sqm plot at Ivory Garden Estate, Phase 1, Makogi, Magboro, Ogun State. Plain HTML +
Tailwind (via CDN, no build step) + a little vanilla JS-free interactivity (native
`<details>` for the FAQ, native video controls for lazy playback).

Source facts came from `land_information.txt`. Everything else (area context, buying-from-
abroad steps, cost breakdown framing) was written to support that, not invented as unrelated
claims.

## Structure
- `index.html` — the entire page.
- `assets/videos/` — the 7 source clips, renamed for clarity:
  - `land-exact-plot.mp4`, `land-full-view.mp4`, `land-back-view.mp4` (the actual plot)
  - `estate-entrance.mp4`, `estate-inside-1.mp4`, `estate-inside-2.mp4`, `estate-within-tour.mp4` (the estate)
- `assets/posters/` — one JPG poster per video, extracted with `ffmpeg` so videos don't
  download until a visitor taps play (they're 3–23MB each — `preload="none"` keeps initial
  page weight small).
- `assets/land_information.txt` — the seller's original raw notes the site content was built
  from. Not referenced by `index.html`; kept only as a source record.
- `robots.txt`, `sitemap.xml` — minimal SEO scaffolding. These must stay at the project root
  (not in `assets/`) — both only work when served from the domain root.
- `.vercelignore` — excludes this file and `assets/land_information.txt` from the deployed
  site (they're project notes, not page content, and shouldn't be publicly served even though
  they're harmless if committed to the repo).

## Design decisions
- **Concept**: a surveyor's/deed-document aesthetic (dark ink-green ground, graph-paper
  texture, brass/gold accent, mono type for measurements and money) rather than a generic
  real-estate template — the hero's centerpiece is a hand-drawn SVG survey diagram of the
  actual 15m × 20m / 300sqm plot dimensions, not a stock photo.
- **Honesty over polish**: the poster stills show the plot is currently bushy/undeveloped and
  that some estate roads are still laterite. Copy says this plainly instead of overselling —
  the checklist claims from the original ad (good roads, electricity, drainage, etc.) are kept
  as the seller's stated facts, and the raw video lets buyers judge condition themselves.
- **Cost transparency**: split "paid to the owner" (₦5M) from "paid to the developer" (survey
  plan and deed documentation, both optional, ₦400k each; development levy, compulsory,
  ₦950k). The headline total (₦5,950,000) is land + the compulsory levy only, with a footnote
  giving the ₦6,750,000 figure if both optional fees are also paid — this was explicitly
  requested as the kind of thing that helps a buyer decide.
- **Diaspora section**: added because the brief asked for buyers outside Nigeria to feel
  comfortable — documents-first review, video inspection, Power of Attorney route.
- **SEO**: title/meta/H1 target "land for sale in Magboro", "Makogi", "Ivory Garden Estate";
  `FAQPage` and `RealEstateListing` JSON-LD included; FAQ answers in the JSON-LD match the
  visible accordion content (required for FAQ rich results, not just decorative).

## Known TODOs before going live
1. **Domain**: `index.html` has no canonical/`og:url`, and `sitemap.xml`'s `<loc>` is a bare
   `/` — both need the real absolute URL once this is deployed somewhere. Search each file for
   "TODO" / the placeholder to find them.
2. **Coordinates**: the `GeoCoordinates` in the JSON-LD are an approximate town-level pin for
   Magboro, not a surveyed pin for this specific plot — fine for SEO, not for navigation.
3. **Video weight**: `estate-entrance.mp4` (23MB) and `estate-within-tour.mp4` (22MB) are the
   two heaviest files. If hosting has bandwidth limits, consider re-encoding them smaller with
   ffmpeg before deploy.
4. **Hosting**: this is fully static — Netlify, Vercel, Cloudflare Pages, or GitHub Pages all
   work by pointing at this folder with no build command.
