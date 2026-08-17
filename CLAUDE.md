# North Shore Tiling — Website Documentation

## Local Path
- **Mac:** `~/Desktop/NSP/NSP-Website-Building/NORTH-SHORE-TILING`

## Business
- **Name:** North Shore Tiling
- **Domain:** https://northshoretiles.com.au
- **Service:** Tiling & Waterproofing (primary). Redirect pages link to sister sites for Painting, Cleaning, and Removals.
- **Area:** North Shore Sydney, NSW, Australia
- **Phone:** 0433 333 332
- **Email:** northshoretiling8@gmail.com
- **ABN:** 21 726 132 250
- **Hours:** Mon-Sat 7am-6pm

## Sister Sites
- **Painting:** northshorepaints.com.au (linked from `painting.html` redirect)
- **Cleaning:** northshorecleans.com.au (linked from `cleaning.html` redirect)
- **Removals:** northshoreremovals.com (linked from `northshore-removals.html` redirect)

## Project Structure

```
/
├── index.html                  # Homepage (tiling service page)
├── painting.html               # Redirect → northshorepaints.com.au
├── cleaning.html               # Redirect → northshorecleans.com.au
├── northshore-removals.html    # Redirect → northshoreremovals.com
├── contact.html                # Contact / enquiry page
├── privacy.html                # Privacy policy (Privacy Act 1988 / APPs)
├── terms.html                  # Terms & conditions
├── sitemap.xml                 # XML sitemap (21 URLs — redirect pages excluded/noindexed)
├── robots.txt                  # Crawler directives
├── CLAUDE.md                   # This file
│
├── css/
│   └── styles.css              # Full design system CSS
│
├── js/
│   └── form-validation.js      # Form validation + Formspree submit + n8n lead webhook
│
├── images/
│   ├── logos/                  # NSTLOGO-HD-FINAL.png (176px, optimized)
│   ├── og-*.jpg, blog-*.jpg    # 1200×630 social/OG images
│   └── tiling/                 # Optimized WebP project photos + MP4 clips (~7MB total; originals in Desktop/nst-media-archive)
│
├── videos/                     # ashfield-project.mp4
│
├── suburbs/                    # 17 tiling suburb landing pages
│   ├── tiling-chatswood.html
│   ├── tiling-killara.html
│   ├── tiling-gordon.html
│   ├── tiling-pymble.html
│   ├── tiling-turramurra.html
│   ├── tiling-lindfield.html
│   ├── tiling-roseville.html
│   ├── tiling-st-ives.html
│   ├── tiling-wahroonga.html
│   ├── tiling-lane-cove.html
│   ├── tiling-willoughby.html
│   ├── tiling-artarmon.html
│   ├── tiling-crows-nest.html
│   ├── tiling-north-sydney.html
│   ├── tiling-neutral-bay.html
│   ├── tiling-mosman.html
│   └── tiling-cremorne.html
│
├── blog/
│   ├── index.html                          # Blog listing (2 tiling articles)
│   ├── how-to-choose-bathroom-tiles.html   # Tiling guide
│   └── kitchen-splashback-trends.html      # Splashback trends 2026
│
├── landing/                    # Meta Ads landing page (conversion-optimized)
│   └── tiling.html
│
└── src/                        # Next.js source (unused scaffold)
    └── app/
```

## Suburbs Covered (17)
Chatswood, Killara, Gordon, Pymble, Turramurra, Lindfield, Roseville, St Ives, Wahroonga, Lane Cove, Willoughby, Artarmon, Crows Nest, North Sydney, Neutral Bay, Mosman, Cremorne

## Suburb Page Naming Convention
`suburbs/tiling-{suburb-slug}.html`
- Slugs use lowercase with hyphens: `st-ives`, `north-sydney`, `crows-nest`, `lane-cove`

## Design System
- **Primary colour:** #1a1a2e (dark navy)
- **Gold accent:** #C19A6B (warm gold)
- **Heading font:** DM Serif Display (Google Fonts)
- **Body font:** DM Sans (Google Fonts)
- **Icons:** Font Awesome 6.5.1 (CDN)
- **Approach:** Mobile-first responsive CSS

## SEO Implementation
- Unique `<title>` and `<meta description>` on every page
- JSON-LD structured data on every page (LocalBusiness + Service schemas)
- Open Graph and Twitter Card tags on every page
- Canonical URLs on every page
- hreflang="en-AU" on every page
- Sitemap XML with 22 pages
- robots.txt with sitemap reference
- Proper heading hierarchy (single H1 per page)
- Descriptive alt text on all images
- aria-labels on interactive elements

## Tracking Placeholders
All pages include:
- `<!-- GOOGLE TAG MANAGER -->` in `<head>`
- `<!-- GTM NOSCRIPT -->` after opening `<body>`
- `window.dataLayer.push()` on form submission
- `// CONVERSION TRACKING - fire GTM event here` comment

Landing page additionally includes:
- `<!-- META PIXEL CODE HERE -->` in `<head>`
- Hidden field `source=meta-ad` on form

## Form Structure
All enquiry forms have `action="https://formspree.io/f/xojkgngr"` and are driven by `js/form-validation.js`, which:
- validates, then POSTs the payload to Formspree (customer-facing delivery)
- fires the n8n lead-tracking webhook `https://droam8.app.n8n.cloud/webhook/lead-submission` (fire-and-forget, full payload incl. suburb, callback Yes/No, hidden source/landing, page path)
- shows a visible error with the phone number if Formspree fails (no fake success)
- Hidden `source` field (organic / meta-ad); dataLayer push `{event: 'form_submission', service, source}`

## Internal Linking Strategy
- Homepage → 17 tiling suburb pages, blog, contact
- Suburb pages → homepage, 2-3 nearby suburb pages
- Blog posts → homepage, 1-2 suburb pages
- Footer → services (redirects for non-tiling), 6 popular suburb pages
- Nav dropdown → Tiling (homepage), Painting/Cleaning/Removals (redirect pages)

## To-Do (Not Yet Implemented)
- [x] Add actual images to /images/ folder (Croydon Park + optimized existing sets)
- [ ] Configure Google Tag Manager container
- [ ] Install Meta Pixel
- [x] Set up form backend (Formspree + n8n webhook via form-validation.js)
- [ ] Configure Google My Business
- [ ] Submit sitemap to Google Search Console
- [ ] Set up Google Ads conversion tracking
- [ ] Add actual Google Map embed on contact page
- [x] Create privacy.html and terms.html pages
- [x] Add favicon and apple-touch-icon (all pages)
- [x] Remove landing pages from sitemap.xml
- [x] ~~Add `Disallow: /landing/` to robots.txt~~ (reversed in D23 — blocked crawl hid the noindex)
- [x] Replace placeholder ABN with real ABN
- [x] Aggregate ratings — verified none exist in schema (nothing to replace)
- [ ] Minify form-validation.js for production
- [ ] Confirm real Instagram handle, then normalise all links (Q7 — blocked on owner)
- [ ] Audit holds: W4 nav gaps, W5 blog decision, W6 reviews/mega-menu port, W8 a11y, W9 perf, W10 schema

---

## File Tracking Protocol

This project maintains two tracking files that must be kept current across all sessions:

### `progress.md`
- **Purpose:** Tracks what has been built, what remains, and known issues
- **When to update:** After completing any feature, fixing a bug, or discovering a new issue
- **Structure:** Phased checklist (Done/Remaining/Known Issues tables)

### `decisions.md`
- **Purpose:** Logs every significant architectural or design decision with rationale
- **When to update:** When making a non-trivial choice (new technology, structural change, naming convention, etc.)
- **Structure:** Numbered entries (D01, D02...) with Decision, Rationale, and Trade-offs
**Next ID:** D27

### Rules for All Sessions
1. **Read both files at the start** of any session that involves code changes
2. **Update `progress.md`** whenever you complete a task, discover a bug, or add something to the remaining work list
3. **Add to `decisions.md`** whenever you make a choice that a future developer would want to understand (use the next available D-number)
4. **Never delete decision entries** — if a decision is reversed, add a new entry explaining the reversal and reference the original
5. **Keep `progress.md` timestamps current** — update the "Last updated" date at the top
