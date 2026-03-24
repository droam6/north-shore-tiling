# North Shore Projects — Architectural Decisions Log

> Records every significant design and technical decision in the project.
> Format: Decision, rationale, and any trade-offs.

---

## D01: Static HTML over Next.js

**Decision:** Build the site as plain static HTML files instead of using the Next.js scaffold that was already present.

**Rationale:**
- The project is a brochure/lead-gen site for a local trades business — no dynamic content, no user accounts, no database
- Static HTML has zero build step, zero runtime dependencies, and deploys anywhere (Netlify, Vercel, S3, shared hosting)
- Next.js adds unnecessary complexity (hydration, React runtime, node server) for what is essentially 70 pages of marketing copy
- Faster page loads — no JavaScript framework overhead
- Easier for non-developers to understand and edit

**Trade-offs:**
- No component reuse — header/footer duplicated across 70 files (changes require bulk edits)
- No templating — suburb pages were generated via AI agents rather than a loop over data
- Future migration to a static site generator (11ty, Astro) would be straightforward since the HTML is already written

**Artefact:** The Next.js scaffold still exists at `/src/` and `package.json` but is unused. Listed in `.gitignore` under `north-shore-projects/`.

---

## D02: 17 Suburbs x 3 Services = 51 Suburb Landing Pages

**Decision:** Create 51 individual suburb landing pages (one per service per suburb) rather than a single "service areas" page.

**Rationale:**
- Local SEO dominance — each page targets a long-tail keyword like "tiling Chatswood" or "cleaning Mosman"
- Google ranks individual, content-rich pages higher for local intent queries
- Each suburb page contains unique content referencing local landmarks, building types, and neighbourhood characteristics (not boilerplate)
- Allows per-suburb tracking via hidden `landing` / `suburb_page` fields on forms

**Suburbs chosen:**
The 17 suburbs cover the core North Shore corridor from Chatswood to Cremorne, hitting major residential and commercial centres. Selection based on population density, renovation activity, and proximity to the business.

**Trade-offs:**
- 51 files to maintain — any structural change must be applied across all
- Content must genuinely differ per suburb or Google may treat it as thin/duplicate
- Ongoing content freshness required to maintain rankings

---

## D03: Service-Specific Email Routing

**Decision:** Use three separate Gmail addresses tied to services rather than a single inbox.

| Service | Email |
|---------|-------|
| Tiling | northshoretiling8@gmail.com |
| Painting | northshorepainting8@gmail.com |
| Cleaning | northshorecleaning8@gmail.com |
| Default (general pages) | northshoretiling8@gmail.com |

**Rationale:**
- Separate inboxes allow different team members or contractors to handle enquiries for their trade
- Simpler than building a routing layer in the backend — just forward to the right address
- JSON-LD `email` field matches the page's service for schema accuracy
- General pages (homepage, contact, blog) default to the tiling email as the primary business contact

**How it works:**
- The email address is embedded in JSON-LD structured data on each page (for schema/SEO)
- Form submissions POST to `/api/contact` with a `service` field — the backend routes to the correct inbox
- The frontend does not handle email routing directly

**Trade-offs:**
- Three inboxes to monitor instead of one
- Gmail has sending limits (may need to upgrade to Workspace for volume)
- Backend routing logic still needs to be built

---

## D04: Removals Listed in Schema but No Dedicated Pages

**Decision:** Include "Removals" as a service in the homepage JSON-LD `hasOfferCatalog` but do not create a dedicated removals page, suburb pages, or landing page.

**Rationale:**
- Removals is handled by an external partner, not the North Shore Projects team directly
- Listing it in schema signals to Google that the business can arrange removals (increases topical breadth)
- No suburb pages needed since the business doesn't perform the removals themselves — they refer out
- Avoids creating content the business can't directly back up with expertise

**Trade-offs:**
- Users searching "removals North Shore" won't find a dedicated page
- Could add a simple informational page in future if referral volume justifies it

---

## D05: Font Choices — Montserrat + Open Sans

**Decision:** Montserrat for headings, Open Sans for body text.

**Rationale:**
- Montserrat (600/700 weight) — geometric sans-serif that conveys modern professionalism. Strong at large sizes for hero text and section titles
- Open Sans (400 weight) — highly legible at small sizes, excellent for long-form content on suburb and blog pages. One of the most battle-tested body fonts on the web
- Both are Google Fonts (free, CDN-hosted, preconnected for performance)
- The pairing is intentionally safe and corporate — appropriate for a trades business targeting homeowners who value reliability over trendiness

**Trade-offs:**
- Google Fonts CDN introduces an external dependency and a render-blocking request
- Could self-host fonts for better performance (eliminates third-party DNS lookup)
- Limited to 2 weights to minimise download size

---

## D06: Gold (#D4A853) as Accent Colour

**Decision:** Use a warm gold as the primary accent colour against a dark navy (#1a1a2e) background.

**Rationale:**
- Gold connotes quality, premium service, and trust — important for a trades business
- High contrast against the dark navy primary colour for readability
- Works well for CTAs, underlines, hover states, and badges
- Avoids generic trade-industry colours (blue/orange/green) to stand out
- The gold-on-dark palette feels upmarket — aligned with the North Shore Sydney demographic (higher income, quality-conscious homeowners)

**CSS custom properties:**
```css
--color-gold: #D4A853;
--color-gold-hover: #c49a48;
--color-gold-light: rgba(212, 168, 83, 0.1);
```

---

## D07: Inline CSS on Landing Pages

**Decision:** Landing pages (`/landing/*.html`) embed all CSS inline in a `<style>` block rather than linking to `styles.min.css`.

**Rationale:**
- Eliminates one render-blocking CSS request — critical for paid ad landing pages where every millisecond of load time affects conversion rate and Quality Score
- Landing pages have a stripped-down layout (no full nav, no footer links, no blog sections) so only a subset of styles are needed
- Inline CSS means the page is fully styled on first paint with zero external requests beyond fonts
- Meta Ads and Google Ads penalise slow-loading landing pages in ad auction

**Trade-offs:**
- CSS changes must be applied in two places: `styles.css` AND each landing page's inline block
- Form validation CSS was added inline to all 3 landing pages when the validation system was built
- No caching benefit — the CSS is re-downloaded with every page view (acceptable for ad landing pages with short sessions)

---

## D08: Honeypot + Timestamp over CAPTCHA

**Decision:** Use a honeypot field and 3-second timestamp check for spam prevention instead of reCAPTCHA or hCaptcha.

**Rationale:**
- Zero friction for real users — no "click all the traffic lights" challenges
- reCAPTCHA adds ~500KB of JavaScript and a Google dependency
- Honeypot catches naive bots (hidden field that only bots fill)
- Timestamp check catches automated form-fillers (humans can't fill a form in under 3 seconds)
- Both checks show a fake success message to bots — they think the submission went through, reducing retry attempts

**Implementation:**
```
Honeypot: hidden input name="website_url" (injected by JS, aria-hidden)
Timestamp: PAGE_LOAD_TIME vs submission time, reject if < 3000ms
```

**Trade-offs:**
- Won't stop sophisticated bots that execute JavaScript and wait
- No server-side validation layer yet (backend doesn't exist)
- May need to add rate limiting or server-side honeypot check when backend is built
- Could layer in Cloudflare Turnstile later if spam becomes a problem

---

## D09: `noindex` Landing Pages in Sitemap (Known Inconsistency)

**Decision (unintentional):** Landing pages are marked `noindex, nofollow` in their meta tags but are still listed in `sitemap.xml`.

**Status:** Known issue — should be fixed before launch.

**Correct approach:**
- Remove landing page URLs from `sitemap.xml`
- Add `Disallow: /landing/` to `robots.txt`
- The `noindex` meta tag is already correct

---

## D10: Form Validation as External JS (Not Inline)

**Decision:** Create a single `js/form-validation.js` file referenced by all 59 pages with forms, rather than inline validation per page.

**Rationale:**
- Single source of truth — validation rules are defined once
- Auto-discovers forms via `document.querySelectorAll('form[action="/api/contact"]')` — works on any page without configuration
- Dynamically injects error spans, honeypot fields, and character counters — HTML doesn't need to include them
- `defer` attribute ensures DOM is ready before script runs
- Existing inline JS only handles mobile menu toggle and scroll animations — no overlap

**Trade-offs:**
- One additional HTTP request on every page (could inline for landing pages in future)
- Currently unminified (357 lines) — should minify for production
- All 59 pages had their inline form handlers removed to prevent double-submission

---

## D11: Blog Content Strategy — Educational SEO

**Decision:** Create 5 long-form blog posts (800-1200 words) targeting informational search queries rather than transactional ones.

**Posts:**
1. How to Choose Bathroom Tiles — targets "bathroom tiles guide"
2. Interior Painting Tips — targets "painting tips DIY"
3. End of Lease Cleaning Checklist — targets "bond cleaning checklist Sydney"
4. Kitchen Splashback Trends 2026 — targets "splashback ideas"
5. How Often to Repaint Your House — targets "when to repaint house"

**Rationale:**
- Informational content builds topical authority — Google sees the site as an expert on tiling/painting/cleaning
- Each post links to the relevant service page and 1-2 suburb pages — passes link equity
- Users researching these topics are likely future customers (top of funnel)
- Blog content is more shareable and linkable than service pages

**Trade-offs:**
- Blog content needs periodic updates to stay relevant (especially the "2026 trends" post)
- No CMS — editing blog posts requires HTML editing
- Images are placeholder — blog posts need real visuals to perform

---

## D12: Hidden Form Fields for Attribution

**Decision:** Embed hidden fields in forms to track lead source and landing page.

**Fields:**
| Field | Values | Present On |
|-------|--------|-----------|
| `source` | `organic` / `meta-ad` | All pages with forms |
| `landing` | `{service}-{suburb}` | Some suburb pages (cleaning + some tiling) |
| `suburb_page` | `{suburb name}` | Some painting suburb pages |
| `service` | Pre-selected value | All forms |

**Rationale:**
- Allows the backend to attribute leads to organic SEO vs paid Meta Ads
- Suburb-level tracking shows which suburb pages generate the most enquiries — informs SEO investment
- Service pre-selection on suburb pages reduces form friction (one less field to fill)

**Known inconsistency:** Some suburb pages use `name="landing"` while others use `name="suburb_page"`. These were created in different batches. Should be standardised to one field name.

---

## D13: Mobile-First CSS with 3 Breakpoints

**Decision:** Design for mobile first, then progressively enhance at 768px, 1024px, and 1200px.

**Breakpoints:**
| Width | Target | Changes |
|-------|--------|---------|
| Default | Mobile | Single column, stacked layout, hamburger nav |
| 768px | Tablet | 2-column grids, larger typography |
| 1024px | Desktop | Horizontal nav, 3-column grids, sidebar layouts |
| 1200px | Large | Extended grid columns, max-width containers |

**Rationale:**
- 60%+ of local service searches happen on mobile — mobile must be the primary experience
- Progressive enhancement ensures the site works everywhere, even on slow connections
- Three breakpoints cover the practical range without over-engineering

---

## D14: Single Phone Number Across All Services

**Decision:** Use one phone number (0433 333 332) for all three services rather than separate numbers.

**Rationale:**
- Simpler for customers — one number to remember
- Easier to manage — no call routing complexity
- The person answering can route based on the caller's request
- Consistent across JSON-LD, nav bars, footers, and CTAs

**Trade-offs:**
- Can't track which service generated a phone call (unlike email routing)
- Could add call tracking numbers later for marketing attribution

---

## D15: Placeholder ABN and Review Data

**Decision:** Use placeholder values for ABN and aggregate ratings in structured data until real data is available.

**Placeholders:**
- ABN: `12 345 678 901` (dummy)
- Aggregate rating: 4.9 stars, 87 reviews (aspirational)

**Rationale:**
- JSON-LD schema structure is in place and correct — just needs real values swapped in
- Having the structure ready means launch-day updates are a simple find-and-replace
- The ABN format is correct (11 digits with spaces) so schema validators pass

**Risk:** Launching with fake review data violates Google's structured data guidelines and could result in a manual action. Must update before going live.

---

## D16: AOS CDN Switch — cdnjs to jsDelivr

**Decision:** Switch AOS (Animate on Scroll) library from `cdnjs.cloudflare.com` to `cdn.jsdelivr.net` and wrap `AOS.init()` in try-catch.

**Rationale:**
- cdnjs URL was returning 404 for `aos/2.3.4/aos.min.js` and `aos.css`, causing "AOS is not defined" which crashed all subsequent JavaScript on every page
- jsDelivr serves the same files reliably at `cdn.jsdelivr.net/npm/aos@2.3.4/dist/`
- Try-catch around `AOS.init()` ensures that even if the CDN fails in future, the rest of the page's JS (slideshow, menu, forms) continues to work

**Trade-offs:**
- jsDelivr is another external CDN dependency — could self-host AOS in future
- Try-catch means AOS failures are silent — no visual indication that scroll animations aren't loading

---

## D17: Mobile Hero — Instant Slide Switching (No Crossfade)

**Decision:** Disable CSS transitions on `.hero-slide` in the mobile media query (`transition: none`) and auto-rotate slides via `setInterval` at 5-second intervals.

**Rationale:**
- The desktop crossfade transition (0.8s opacity) caused two slides to be visible simultaneously on mobile, with text from both slides overlapping
- On mobile the hero is smaller and the overlap is much more noticeable and distracting
- With arrows and indicators hidden on mobile, autoplay is the only way users see different slides — the 5s interval gives enough reading time
- Instant switching is cleaner on small screens where the crossfade doesn't add perceived quality

**Trade-offs:**
- Mobile users see an abrupt slide change rather than a smooth fade
- Desktop retains the original crossfade behaviour (desktop media query is unaffected)

---

## D18: Visual Redesign — Editorial Design System (v2)

**Decision:** Rewrite the entire CSS design system with an editorial aesthetic, adapted to NSP's brand colours.

**What changed:**
- **Fonts:** Montserrat + Open Sans → **DM Serif Display** (headings) + **DM Sans** (body). DM Serif is a modern serif that conveys premium quality; DM Sans pairs cleanly for body text.
- **Gold accent:** #D4A853 → **#C19A6B** (warmer, more muted gold)
- **Background:** Pure white → **#FAFAF8 cream / #FDFCFA warm-white** (subtle warmth, less clinical)
- **Body text colour:** Navy → **#71706E muted** for body copy (softer contrast, editorial feel)
- **Section pattern:** Added editorial-style section labels (11px uppercase gold, 0.3em tracking) above section titles
- **Services layout:** Added editorial numbered list layout alongside bento grid
- **Trust bar:** Stats section with gold-bordered dark background
- **Navigation:** Refined with transparent→opaque backdrop-blur, gold-bordered CTA
- **Forms:** Transparent bg inputs, cream border, gold focus state (dark sections); light variant for cream sections
- **Footer:** Darker navy-dark (#12121F), 4-column grid, gold category labels
- **New components:** FAQ accordion, process steps, gallery/lightbox, CTA banners, service hero, Google rating badge
- **All existing class names preserved** — `.navbar`, `.mobile-menu`, `.hero-slideshow`, `.section`, `.form-group`, `.blog-card`, etc. all still work

**Rationale:**
- The editorial aesthetic (serif headings, muted palette, generous whitespace, numbered services) feels upmarket — aligned with North Shore Sydney's demographic
- A proven design approach for a local professional services business targeting the same geographic area
- Keeping class names identical means no HTML changes are needed for the CSS swap — pages degrade gracefully until HTML is rebuilt
- The serif heading + sans body pairing differentiates NSP from the typical all-sans-serif trades business website

**Trade-offs:**
- HTML pages still load Montserrat + Open Sans via Google Fonts CDN — fonts will fall back to Georgia/Helvetica until `<link>` tags are updated
- Gold colour change (#D4A853 → #C19A6B) means any inline styles or landing page CSS using the old gold will be visually inconsistent
- DM Serif Display is a display font — may not be ideal at very small sizes (below 16px). Body text uses DM Sans to avoid this
- CSS grew from ~1,400 to ~2,900 lines — more component styles for the richer visual system

---

## D19: Blog Removed from Site

**Decision:** Remove all blog references from the live site — navigation, mobile menu, footer, homepage blog preview section, service page "Related Blog Posts" sections, inline content links in suburb pages, and sitemap.xml entries.

**Rationale:**
- Business decision — blog content is not needed for launch
- Reduces site complexity and maintenance burden (5 blog posts + listing page = 6 fewer pages to maintain)
- Blog files are retained in `/blog/` directory but completely unlinked — can be restored later if needed

**What was removed:**
- Nav `<li>` blog link from all 60 pages (root + suburb + removals)
- Mobile menu blog link from all 60 pages
- Footer blog link from all 60 pages
- Homepage "Tips & Guides" blog preview section (3 blog cards)
- tiling.html "Related Blog Posts" section (2 blog cards)
- painting.html "Related Blog Posts" section (2 blog cards)
- cleaning.html "Related Blog Posts" section (1 blog card)
- 6 blog `<url>` entries from sitemap.xml
- Inline `<a>` links to blog articles within suburb page content (51 pages)
- Broken sentences from link removal fixed in tiling-gordon.html and painting-chatswood.html

**Trade-offs:**
- Loss of SEO topical authority from informational content (see D11)
- Blog files still exist on disk — could be accidentally discovered via direct URL
- If blog is restored later, all links must be re-added across 60+ files

---

## D20: Bug Fix Pass — 6 Fixes Before Launch

**Decision:** Systematically fix 6 known bugs/inconsistencies across the site.

**Fixes applied:**

1. **contact.html navbar** — Instagram link was missing `?igsh=MW9vbzJtaXRoY3h3OQ==` tracking parameter that all other pages had. Added for consistency.

2. **Mobile horizontal overflow** — AOS `fade-left`/`fade-right` animations create off-screen elements that caused horizontal scrollbar. `overflow-x: hidden` was only on `html` — added to `body` as well. Both are needed because mobile Safari and Chrome handle overflow differently on `html` vs `body`.

3. **Hero text overlap on small screens** — On viewports under 767px, the hero's label + title + subtitle + 2 CTA buttons exceeded the 100vh container. Added comprehensive mobile overrides: reduced min-height to 85vh (with 85svh for iOS Safari), tightened all spacing and font sizes.

4. **Dynamic copyright year** — Replaced hardcoded `&copy; 2026` with `<script>document.write(new Date().getFullYear())</script>` across 63 files. Landing pages excluded (they use inline CSS and are maintained separately).

5. **Hidden form field consistency (verified)** — The `name="landing"` vs `name="suburb_page"` inconsistency noted in D12 no longer exists — all 51 suburb pages already use `name="landing"`. The different field sets across page types (homepage/contact vs service vs suburb) are intentional: pages where the service is already known use a hidden field; pages where the user chooses use a `<select>` dropdown.

6. **Landing pages removed from sitemap.xml + robots.txt updated** — Removed 3 `/landing/*.html` entries from sitemap.xml and added `Disallow: /landing/` to robots.txt. Resolves D09 (noindex pages contradictorily listed in sitemap).

**Rationale:**
- All fixes target launch-readiness — mixed signals to crawlers, mobile UX regressions, and maintenance debt
- No structural or architectural changes — pure bug fixes and consistency cleanup

**Trade-offs:**
- `document.write()` for copyright year is a deprecated API pattern, but it's safe in this context (inline, synchronous, no performance impact) and avoids the complexity of a DOMContentLoaded listener across 63 files
- `overflow-x: hidden` on body may clip intentionally overflowing elements in future — acceptable since no current design requires horizontal overflow

---

## D21: Internal Removals Page Alongside External Site

**Decision:** Create `northshore-removals.html` as an internal removals service page on the NSP domain, while keeping all existing nav/footer/mobile menu links pointing to the external `northshoreremovals.com` site.

**Rationale:**
- The external removals site (`northshoreremovals.com`) is a separate domain and may not always rank for queries related to NSP
- An internal page captures SEO value on the NSP domain — Google sees removals as a first-party service, not just an external referral
- The page uses removals-specific data (155+ reviews, $170/hr pricing, 1,500+ moves) that differs from the generic NSP stats (87 reviews)
- Secondary links added to index.html hero slide 3 and services section #03 give users a path to the internal page without disrupting the primary external CTA flow
- The existing draft `removals.html` was not suitable — it used generic content and the old 87-review count

**What changed:**
- New file: `northshore-removals.html` (full service page with pricing callout, removals-specific trust bar, 6 services, FAQ, contact form)
- index.html hero slide 3: "Get a Quote" button replaced with "View Our Removals Page" linking to internal page
- index.html services #03: Secondary "View our removals page" link added below existing external link
- sitemap.xml: New URL entry added

**What was NOT changed:**
- Nav dropdown links (still external northshoreremovals.com)
- Mobile menu links (still external)
- Footer service links (still external)
- The existing draft `removals.html` (left as-is, unlinked)
- No removals suburb pages created (per D04)

**Trade-offs:**
- Two removals pages now exist on the NSP domain (`removals.html` draft + `northshore-removals.html`) — the draft is unlinked and should be cleaned up or deleted eventually
- Users may see both the external site and internal page in search results — this is intentional (more SERP real estate)
- The internal page's review count (155) and pricing ($170/hr) must be kept in sync with the external site manually

---

## D22: Site Split — Tiling-Only Repo from Multi-Service Site

**Decision:** Split the multi-service NORTH-SHORE-PROJECTS repo into a tiling-only site at `northshoretiles.com.au` (NORTH-SHORE-TILING repo). Sister sites for painting and cleaning will be created separately.

**What changed:**
- Forked NORTH-SHORE-PROJECTS into a new NORTH-SHORE-TILING directory with fresh git history
- `tiling.html` renamed to `index.html` — tiling is the homepage
- Deleted all painting/cleaning content: 34 suburb pages, 3 blog posts, 2 landing pages, `images/painting/` directory
- Created thin redirect pages for `painting.html`, `cleaning.html`, `northshore-removals.html` — minimal pages with full nav/footer that point users to sister site domains
- Domain updated across all files: `northshoreprojects.com.au` → `northshoretiles.com.au`
- Branding updated: "North Shore Projects" → "North Shore Tiling" in nav, footer, titles, JSON-LD
- Blog listing trimmed to 2 tiling-only articles
- Sitemap rewritten with 22 tiling-focused URLs
- Nav/footer still lists all 4 services (Tiling links to homepage, others link to redirect pages)

**Rationale:**
- Separate domains allow each service to build independent SEO authority and topical relevance
- `northshoretiles.com.au` is a stronger exact-match domain for tiling queries than `northshoreprojects.com.au`
- Each service can evolve its design, content, and marketing independently
- Redirect pages on the tiling site preserve cross-service discoverability — users looking for painting/cleaning are directed to the right domain
- The multi-service NORTH-SHORE-PROJECTS repo is preserved as-is for reference

**Trade-offs:**
- Three repos to maintain instead of one (tiling, painting, cleaning)
- Nav/footer changes must be applied across all three sites if structure changes
- Redirect pages add 3 extra files that exist purely for cross-linking
- Blog content is now split — tiling gets 2 posts, painting/cleaning will get their own when those repos are created
- DNS and hosting must be configured for 3 separate domains
