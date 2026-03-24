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
- **ABN:** 12 345 678 901
- **Hours:** Mon-Sat 7am-6pm

## Sister Sites
- **Painting:** northshorepainting.com.au (linked from `painting.html` redirect)
- **Cleaning:** northshorecleans.com.au (linked from `cleaning.html` redirect)
- **Removals:** northshoreremovals.com (linked from `northshore-removals.html` redirect)

## Project Structure

```
/
├── index.html                  # Homepage (tiling service page)
├── painting.html               # Redirect → northshorepainting.com.au
├── cleaning.html               # Redirect → northshorecleans.com.au
├── northshore-removals.html    # Redirect → northshoreremovals.com
├── contact.html                # Contact / enquiry page
├── sitemap.xml                 # XML sitemap (22 URLs)
├── robots.txt                  # Crawler directives
├── CLAUDE.md                   # This file
│
├── css/
│   ├── styles.css              # Full design system CSS
│   └── styles.min.css          # Minified CSS
│
├── js/
│   └── form-validation.js      # Form validation (IIFE, 357 lines)
│
├── images/
│   ├── logos/                  # Brand logos
│   └── tiling/                 # Tiling project photos (60+ images)
│
├── videos/                     # Project videos (.MOV — gitignored)
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
All enquiry forms POST to `/api/contact` with fields:
- name, email, phone, service, suburb, message
- Hidden `source` field (organic / meta-ad)
- dataLayer push on submission: `{event: 'form_submission', service, source}`

## Internal Linking Strategy
- Homepage → 17 tiling suburb pages, blog, contact
- Suburb pages → homepage, 2-3 nearby suburb pages
- Blog posts → homepage, 1-2 suburb pages
- Footer → services (redirects for non-tiling), 6 popular suburb pages
- Nav dropdown → Tiling (homepage), Painting/Cleaning/Removals (redirect pages)

## To-Do (Not Yet Implemented)
- [ ] Add actual images to /images/ folder
- [ ] Configure Google Tag Manager container
- [ ] Install Meta Pixel
- [ ] Set up form backend (/api/contact endpoint)
- [ ] Configure Google My Business
- [ ] Submit sitemap to Google Search Console
- [ ] Set up Google Ads conversion tracking
- [ ] Add actual Google Map embed on contact page
- [ ] Create privacy.html and terms.html pages
- [ ] Add favicon and apple-touch-icon
- [x] Remove landing pages from sitemap.xml
- [x] Add `Disallow: /landing/` to robots.txt
- [ ] Replace placeholder ABN with real ABN
- [ ] Replace placeholder aggregate ratings with real review data
- [ ] Minify form-validation.js for production

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
- **Next ID:** D23

### Rules for All Sessions
1. **Read both files at the start** of any session that involves code changes
2. **Update `progress.md`** whenever you complete a task, discover a bug, or add something to the remaining work list
3. **Add to `decisions.md`** whenever you make a choice that a future developer would want to understand (use the next available D-number)
4. **Never delete decision entries** — if a decision is reversed, add a new entry explaining the reversal and reference the original
5. **Keep `progress.md` timestamps current** — update the "Last updated" date at the top
