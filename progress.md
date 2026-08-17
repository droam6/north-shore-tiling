# North Shore Tiling — Progress Tracker

> Last updated: 2026-08-17 (Full site audit + quick wins + media overhaul + conversion/legal pass)

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
| Canonical URLs | Done | All pages — domain: northshoretiles.com.au |
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
| Domain updated | Done | northshoreprojects.com.au → northshoretiles.com.au across all files |
| Blog trimmed | Done | Removed 3 non-tiling article cards from blog listing |
| Sitemap rewritten | Done | 22 tiling-only URLs |
| CLAUDE.md rewritten | Done | Tiling-only repo documentation |
| .gitignore updated | Done | Added *.MOV/*.mov |

## Live Deploy Verification (2026-08-17, post-push 40f1eba) — PASSED

Crawled https://northshoretiles.com.au after the Cloudflare Pages deploy:
all 21 sitemap pages + landing page returned 200; all 71 referenced
assets (renamed WebP images, videos, favicon, CSS/JS) returned 200 —
no case-sensitivity breakage; all 6 OG images + robots.txt 200. Only
non-200 was Cloudflare's own `/cdn-cgi/l/email-protection` endpoint
(expected CDN behaviour, not a site asset). No fixes required.

---

## Phase 9: Audit Fix Pass (2026-08-17) — COMPLETE

| Item | Status | Notes |
|------|--------|-------|
| Junk file removal | Done | `index 2.html` (old NSP homepage), server.js, server.py, js/main.js, stale styles.min.css, 4 unused sister logos |
| Wrong-brand logo fix | Done | contact.html + 3 blog pages showed NSP logo; swapped to NST (9 refs) |
| Dead/blank link fixes | Done | 6 blog footer links to deleted painting/cleaning suburb pages; 3 blank inline links on suburb pages |
| Favicon rollout | Done | 32px + 180px versions generated; tags on all 26 pages (was 1 page, 253KB file) |
| Index hygiene | Done | Redirect pages noindexed + out of sitemap; robots.txt `Disallow: /landing/` removed so noindex is crawlable; lastmod refreshed |
| Webhook centralisation | Done | n8n lead webhook moved into form-validation.js: all 20 forms, post-validation, full payload; fake-success on failure removed |
| Media optimization (W1) | Done | 20 gallery/hero images 215MB → 1.2MB WebP 1600px; videos 18.3MB → 5.4MB; 14MB poster → 33KB; logo 517KB → 11.5KB |
| Unreferenced media purge (W1b) | Done | 45 orphan photos (630MB) + all originals archived to `Desktop/nst-media-archive/` then removed from repo |
| Croydon Park photos | Done | 8 best of 80 new job photos processed; hero + gallery group added |
| Unsplash heroes replaced (W7) | Done | 17 suburb pages + contact now use distinct local project photos |
| OG images (W2) | Done | 6 missing 1200×630 social images generated; broken schema logo URLs fixed |
| Suburb hero CTAs (W3) | Done | Get a Free Quote + Call buttons on all 17 suburb heroes |
| Privacy + terms (W11) | Done | privacy.html + terms.html created, footer-linked on all 27 pages incl. landing; in sitemap |

### Deferred from audit (user hold — later session)
- W4 Nav gaps (Contact nav link, desktop header phone, sticky call button on redirect pages)
- W5 Blog decision (re-link vs delete — currently orphaned by design pending decision)
- W6 Reviews carousel + Service Areas mega-menu port from sister sites
- W8 Accessibility pass (gold-on-cream contrast 2.48:1, aria-hidden on ~460 icons, h2→h4 jumps)
- W9 Performance pass (AOS blocks hero LCP, no hero preload, full Font Awesome for 14 icons)
- W10 Schema upgrades (BreadcrumbList, FAQPage, lang/hreflang cleanup)
- Q7 Instagram normalisation — BLOCKED: two handles in use (`northshore.tiling` on suburb pages, `northshoretiling` elsewhere); need the real handle from the owner

---

## REMAINING WORK

### High Priority (blocks launch)

| Item | Blocked By | Notes |
|------|-----------|-------|
| ~~Form backend~~ | Done | Formspree delivery + n8n lead webhook (both wired in form-validation.js) |
| ~~Privacy policy page~~ | Done | privacy.html live, footer-linked |
| ~~Terms & conditions page~~ | Done | terms.html live, footer-linked |
| ~~Favicon + apple-touch-icon~~ | Done | On all pages |

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
| Two Instagram handles in use | Medium | Suburb pages link `northshore.tiling` (51 links); everything else `northshoretiling` (27). One 404s — awaiting the real handle before normalising |
| Git history still holds old photo blobs | Low | Repo pack ~870MB from previously committed originals; needs a history rewrite + force push to shrink (coordinate before doing) |
| Blog orphaned by design | Low | No inbound links, not in sitemap — pending W5 re-link vs delete decision |
| ~~ABN placeholder~~ | Resolved | Real ABN 21 726 132 250 committed (01ecf2d), consistent in all 26 footers |
| ~~Aggregate ratings in schema~~ | Resolved | No aggregateRating exists anywhere — never actually implemented |
| ~~Old fonts on suburb pages~~ | Resolved | Verified: no Montserrat/Open Sans references remain sitewide |
