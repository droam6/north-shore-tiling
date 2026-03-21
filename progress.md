# North Shore Tiling — Progress Tracker

> Last updated: 2026-03-21 (Site split from NORTH-SHORE-PROJECTS into tiling-only repo)

---

## Phase 1: Technical SEO Foundation — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| CSS design system (`css/styles.css`) | Done | ~2,885 lines. Editorial-style DM Serif Display + DM Sans, gold #C19A6B, cream backgrounds |
| Minified CSS (`css/styles.min.css`) | Done | Production bundle (44.1KB) |
| Google Fonts (DM Serif Display + DM Sans) | Done | Preconnected, loaded via Google Fonts CDN |
| Font Awesome 6.5.1 icons | Done | CDN with SRI hash |
| Mobile-first responsive breakpoints | Done | 768px / 1024px / 1200px |
| Mobile hamburger nav | Done | Inline JS toggle on all navigable pages |
| Skip-to-content link | Done | Accessibility baseline |
| Semantic HTML5 structure | Done | `<header>`, `<main>`, `<section>`, `<footer>`, `<nav>` |
| Meta tags (title, description) | Done | Unique per page |
| Open Graph + Twitter Card tags | Done | All pages |
| Canonical URLs | Done | All pages — domain: northshoretiling.com.au |
| hreflang `en-AU` | Done | All pages |
| JSON-LD structured data | Done | LocalBusiness + Service schemas per page type |
| Heading hierarchy (single H1) | Done | Verified across all pages |

## Phase 2: Suburb Landing Pages (17) — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| 17 tiling suburb pages | Done | Unique content per suburb, local landmarks referenced |
| Internal cross-links (nearby suburbs) | Done | 2-3 nearby suburb links per page |
| Service-specific JSON-LD | Done | @graph with Service + HomeAndConstructionBusiness |
| Hidden tracking fields | Done | `source=organic`, `landing=tiling-{suburb}` |
| Forms with `action="/api/contact"` | Done | All 17 pages |
| Breadcrumb navigation | Done | Home > Tiling > Suburb |

### Suburbs covered
Chatswood, Killara, Gordon, Pymble, Turramurra, Lindfield, Roseville, St Ives, Wahroonga, Lane Cove, Willoughby, Artarmon, Crows Nest, North Sydney, Neutral Bay, Mosman, Cremorne

## Phase 3: Blog — PARTIAL (Tiling articles only)

| Item | Status | Notes |
|------|--------|-------|
| Blog listing page (`blog/index.html`) | Done | 2 tiling article cards |
| How to Choose Bathroom Tiles | Done | Tiling guide |
| Kitchen Splashback Trends 2026 | Done | Splashback trends |
| Non-tiling articles removed | Done | Painting/cleaning articles deleted during site split |

## Phase 4: Meta Ads Landing Page — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| Tiling landing (`landing/tiling.html`) | Done | Inline CSS, no nav, conversion-optimised |
| `noindex, nofollow` meta | Done | Prevents indexing of ad-only page |
| Hidden `source=meta-ad` field | Done | Attribution tracking |

## Phase 5: Internal Linking — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| Homepage links to 17 suburb pages | Done | Service areas grid |
| Suburb pages link to homepage | Done | Breadcrumb + body links |
| Suburb pages link to nearby suburbs | Done | Pill-style buttons, 2-3 nearby |
| Footer links | Done | Services (with redirects), 6 popular suburb pages |

## Phase 6: Form Validation — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| `js/form-validation.js` | Done | 357 lines, IIFE pattern |
| Validation CSS in `styles.css` | Done | Error/success states, char counter, loading spinner |
| Honeypot spam field | Done | Hidden `website_url` field |
| Timestamp check | Done | Rejects submissions under 3 seconds |

## Phase 7: Gallery + Video — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| 60 tiling photos in gallery | Done | DSC06968–DSC07069 series, `loading="lazy"` |
| Gallery arrow navigation | Done | Gold-bordered circular buttons, progress bar |
| Phone-frame video section | Done | `videos/North Shore Tiling Ashfield.MOV` |

## Phase 8: Site Split — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| Forked from NORTH-SHORE-PROJECTS | Done | Clean copy, fresh git init |
| Deleted painting/cleaning content | Done | 34 suburb pages, 3 blog posts, 2 landing pages, images/painting/ |
| tiling.html → index.html | Done | Tiling service page is now the homepage |
| Redirect pages created | Done | painting.html, cleaning.html, northshore-removals.html — thin pages linking to sister sites |
| Navigation updated | Done | All ~28 HTML files: tiling.html→index.html, branding→North Shore Tiling |
| Domain updated | Done | northshoreprojects.com.au → northshoretiling.com.au across all files |
| Blog trimmed | Done | Removed 3 non-tiling article cards from blog listing |
| Sitemap rewritten | Done | 22 tiling-only URLs |
| CLAUDE.md rewritten | Done | Tiling-only repo documentation |
| .gitignore updated | Done | Added *.MOV/*.mov |

---

## REMAINING WORK

### High Priority (blocks launch)

| Item | Blocked By | Notes |
|------|-----------|-------|
| Form backend (`/api/contact`) | Hosting decision | Need serverless function or email service |
| Privacy policy page (`privacy.html`) | Legal content | Linked from footers. Currently 404 |
| Terms & conditions page (`terms.html`) | Legal content | Linked from footers. Currently 404 |
| Favicon + apple-touch-icon | Asset creation | No `<link rel="icon">` on any page |

### Medium Priority (blocks marketing)

| Item | Blocked By | Notes |
|------|-----------|-------|
| Google Tag Manager container | GTM account | Replace placeholder with real container ID |
| Meta Pixel installation | Meta Business account | Replace placeholder in landing page |
| Google Ads conversion tracking | Ads account | Configure conversion snippet |
| Google My Business setup | GMB account | Needed for local SEO |
| Google Search Console | DNS verification | Submit sitemap.xml |
| Google Maps embed on contact page | Maps API key | Currently placeholder |

### Low Priority (nice to have)

| Item | Notes |
|------|-------|
| CSS minification automation | Currently manual |
| JS minification | `form-validation.js` is unminified |
| Image optimisation pipeline | WebP conversion, srcset |
| 404 page | No custom 404.html |
| Accessibility audit | Needs WAVE/axe testing |
| Performance audit | Lighthouse once live |
| Convert .MOV to .mp4 | Better cross-browser support |

---

## KNOWN ISSUES

| Issue | Severity | Details |
|-------|----------|---------|
| ABN is placeholder | Medium | `12 345 678 901` is a dummy ABN — needs real ABN before launch |
| Aggregate ratings in schema | Medium | 4.9 stars / 87 reviews hardcoded in JSON-LD — need real review data or remove |
| Blog dates may be future-dated | Low | Blog posts dated Jan-Feb 2026 — verify publish schedule |
| Suburb pages still load old fonts | Low | Some suburb pages may still reference Montserrat + Open Sans in `<link>` tags |
