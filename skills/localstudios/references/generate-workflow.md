# Generate Website — Complete Workflow

This is the full 12-phase workflow for `/localstudios generate <url>`.
Execute phases sequentially. Pause for user input where marked with **[WAIT FOR USER]**.

---

## Phase 1 — Scrape and Analyze

Use WebFetch to retrieve the provided URL. Extract everything listed in `./references/scraping-checklist.md`.

If the URL is unreachable or no website exists:
- Inform the user: "Website not reachable. We'll gather everything in the interview."
- Mark all scraping fields as `[NOT FOUND]`
- Proceed directly to Phase 2

If the URL is reachable:
- Fetch the homepage
- Attempt to fetch /about, /contact, /services, /impressum, /ueber-uns, /kontakt, /leistungen (common patterns)
- Extract all data points from the checklist
- Store findings for Phase 2 comparison

---

## Phase 2 — Interactive User Interview [WAIT FOR USER]

This phase is **MANDATORY**. Never skip it. Never proceed to Phase 3 without user responses.

Load `./references/interview-template.md` and present the full checklist to the user.

Rules:
- Items found in Phase 1: Show with status `[FOUND — please confirm]` and the extracted value
- Items not found: Show with status `[MISSING — required]` or `[MISSING — optional]`
- Wait for the user to respond to ALL items before proceeding
- If the user skips optional items, use sensible defaults
- If the user skips required items, ask again specifically

After the user responds, compile a **Project Brief** summary:
```
=== PROJECT BRIEF ===
Business: [name] — [industry]
Location: [city, region, country]
Primary Keyword: [keyword]
Secondary Keywords: [list]
Target Audience: [description]
Website Goal: [call / form / booking / purchase]
Pages: [list]
Language: [language(s)]
Design: [style preference or "AI decides"]
=== END BRIEF ===
```

Show the brief and ask the user to confirm before proceeding.

---

## Phase 3 — Keyword Research (max 5 API calls)

**Check:** Are Semrush MCP tools available? (`mcp__semrush__*`)

### If Semrush is available:

Execute in this priority order, stop at 5 calls total:

**Call 1** — Keyword Overview for the primary keyword
- Region: based on user's country from interview
- Get: search volume, keyword difficulty, CPC, intent

**Call 2** — Keyword variations and semantically related keywords
- Get: 5-10 secondary keywords with volume

**Call 3** — Geo-specific combinations
- Pattern: [service] + [city], [service] + [region], [service] + [district]
- Get: volume for each combination

**Call 4** (optional) — Additional keywords for separate service pages
- Only if user has multiple distinct services

**Call 5** (optional) — Competitor keyword analysis
- Only if user named a competitor

### If Semrush is NOT available:

Inform the user:
```
Semrush MCP is not configured. Proceeding with manual keywords from your interview.
For richer keyword data, configure Semrush MCP and re-run.
```

Use the primary and secondary keywords from the interview as-is.

### Output — Keyword Table:

```
| Keyword | Volume/month | Difficulty | Target Page | Type |
|---------|-------------|------------|-------------|------|
| [primary] | [x] | [x] | Home | Primary |
| [secondary 1] | [x] | [x] | Service Page | Secondary |
| [geo combo] | [x] | [x] | Home + Contact | Geo |
```

---

## Phase 4 — Multi-Keyword Geo-Strategy per Page

Map keyword clusters to pages. Each page targets a semantic cluster, not a single keyword.

### Cluster Logic:

**Home Page:**
- Brand + Primary Keyword + City + Region
- Example: "Smith Dental — Dentist Zurich — Canton Zurich"

**Service Pages** (one per main service):
- Specific service + geo-combinations (district, city, region)
- Example: "Teeth Whitening Zurich" also targets "Teeth Whitening Zurich North", "Teeth Whitening Canton Zurich"

**Contact Page:**
- "[Service] + Contact + [City]" + opening hours keywords
- "[Service] near me" variations

**About Page:**
- Brand + trust keywords + location name
- "[Company] team", "[Company] experience"

### Unique Page Strategy:
One strong page beats five thin pages. Embed geo-terms naturally in content rather than creating separate pages for each location variant.

### Output — Keyword-to-Page Map:

Show a clear table mapping every keyword to its target page before proceeding to content writing.

---

## Phase 5 — SEO Audit of Existing Website

**Check:** Is the claude-seo plugin available? (Is `/seo` skill loaded?)

### If claude-seo is available AND a URL was provided:

Run these analyses:
- `/seo page <url>` — on-page SEO analysis
- `/seo technical <url>` — technical SEO check
- `/seo schema <url>` — structured data check

Compile results into a **"What Was Wrong" list**:
```
=== ISSUES ON EXISTING SITE ===
- [ ] Missing H1 / duplicate H1s
- [ ] No meta descriptions
- [ ] No schema markup
- [ ] Missing alt texts on images
- [ ] No internal linking strategy
- [ ] Slow page load
- [ ] Not mobile-optimized
- [ ] Missing OG tags
- [ ] NAP inconsistencies
=== THESE WILL BE FIXED IN THE NEW SITE ===
```

### If claude-seo is NOT available or no URL:

Skip this phase. Inform the user:
```
Skipping SEO audit (claude-seo not available or no existing site).
Building the new site with SEO best practices from scratch.
```

---

## Phase 6 — Website Structure Outline [WAIT FOR USER]

Before writing ANY content, create the complete page outline and get user approval.

### Outline Format per Page:

```
PAGE: HOME
H1: [Primary Keyword + Location]
Sections:
  1. Hero — H1, subheadline with secondary KW, primary CTA button
  2. Trust Bar — 3 numbers or USPs (years experience, clients served, etc.)
  3. Services Overview — cards per service with internal links to service pages
  4. Social Proof — Google reviews or testimonials
  5. Local Trust — map reference, address, "In [City] since [Year]"
  6. CTA — repeat main action
  7. Footer — complete NAP, consistent across all pages
Keywords targeting: [list from Phase 4]
Meta Title: [Primary KW] — [Company] — [City] (max 60 chars)
Meta Description: [Concrete benefit + CTA + location] (max 155 chars)

PAGE: ABOUT
H1: [Company Name] — [Trust phrase + Location]
Sections:
  1. Team Story — founding, mission, personal touch
  2. Credentials — education, certifications, awards (E-E-A-T)
  3. Photo placeholders — team, office, workspace
  4. Local connection — community involvement, local references
Keywords targeting: [list]
Meta Title: ...
Meta Description: ...

PAGE: SERVICE — [Name] (one per main service)
H1: [Service + Location]
Sections:
  1. What is it — clear explanation with primary keyword
  2. Who is it for — target audience, use cases
  3. Process — step-by-step how it works
  4. Benefits — why choose this service
  5. FAQ — 5-7 questions, voice-search friendly
  6. CTA — book/call/contact
  7. Internal links — back to Home, to Contact, to related services
Keywords targeting: [list]
Meta Title: ...
Meta Description: ...

PAGE: CONTACT
H1: [Service + Contact + City]
Sections:
  1. NAP — prominent display (name, address, phone)
  2. Contact form — with clear labels
  3. Google Maps embed placeholder
  4. Opening hours
  5. Additional contact methods (email, WhatsApp if applicable)
Keywords targeting: [list]
Meta Title: ...
Meta Description: ...
```

### Internal Linking Map:
Show which pages link to which, and with what anchor texts:
```
Home → Service A (anchor: "[Service A] in [City]")
Home → Service B (anchor: "[Service B] — learn more")
Home → Contact (anchor: "Contact us" / "Book appointment")
Service A → Contact (anchor: "Book your [Service A]")
Service A → Service B (anchor: "Also see: [Service B]")
Service B → Service A (anchor: "Related: [Service A]")
All pages → Home (via logo/nav)
```

**Present the complete outline to the user and WAIT for approval before proceeding.**

---

## Phase 7 — SEO-Optimized Content per Page

Load `./references/content-guidelines.md` for detailed rules.

### Write content for each page following these rules:

**SEO Rules:**
- H1 contains primary keyword of the page, exactly once, at the top
- H2s contain secondary keywords and geo-terms, naturally phrased
- Geo-term in the first paragraph and in at least one H2
- Primary keyword density: 1-2%, never keyword-stuff
- Local signals: city name, district, region, known local references
- Meta Title: Primary KW — Company — City (max 60 chars)
- Meta Description: concrete benefit + CTA + location (max 155 chars)
- OG Title and OG Description for social sharing on every page

**E-E-A-T Content Quality:**
- Write from experience perspective, not generic
- Include concrete numbers where available
- Local references that build trust
- FAQ on every service page, phrased as people speak (voice-search ready)

**Internal Linking:**
- Home links to all service pages
- Every service page links back to Home and Contact
- Related service pages cross-link with meaningful anchor texts
- Never use "click here" as anchor text

**CTA Positioning:**
- Primary CTA above the fold (in hero section)
- Repeat CTA after social proof section
- Every page has at least one visible CTA without scrolling

### Output per page:
- Complete content with all headings (H1, H2, H3)
- Meta Title
- Meta Description
- OG Title
- OG Description
- Internal links with anchor texts noted

---

## Phase 8 — Schema Markup Generation

Load `./references/schema-templates.md` for JSON-LD templates.

### Generate for Home + Contact:

**LocalBusiness Schema:**
- All NAP data from interview
- Opening hours
- Geo-coordinates (if known, otherwise note as placeholder)
- Price range
- `sameAs` links (Google Business Profile, social media)
- Description with primary keyword

**BreadcrumbList Schema:**
- For every page: Home > [Page Name]
- For service pages: Home > Services > [Service Name]

### Generate for each Service Page:

**Service Schema:**
- Service name, description, provider (linked to LocalBusiness)
- Area served
- Price range if applicable

**FAQPage Schema:**
- All FAQ questions and answers from the content

### Generate for About Page:

**Organization or Person Schema:**
- Founding date
- Credentials, certifications
- Team members if applicable

### Output:
Complete JSON-LD blocks per page, ready to paste into `<head>`.

---

## Phase 9 — Design System

**Check:** Is the ui-ux-pro-max plugin available?

### If ui-ux-pro-max is available:

Use the `/ui-ux-pro-max` skill to generate a design system:
- Input: industry/service type from interview + company name
- Get: layout pattern, color palette, typography, effects, anti-patterns

### If ui-ux-pro-max is NOT available:

Create a generic professional design system:
```
DESIGN SYSTEM (Generic Professional)
- Layout: Clean, card-based with clear hierarchy
- Colors: Based on user preference from interview, or neutral professional palette
- Typography: System fonts or popular Google Fonts pairing
- CTA: High-contrast button, consistent across all pages
- Mobile: Responsive, touch-friendly targets
```

### Output:
Complete design system brief that can be passed to Stitch.

---

## Phase 10 — Website Generation in Google Stitch

**Check:** Are Stitch MCP tools available? (`mcp__stitch__*`)

### If Stitch is available:

1. Create a new project in Stitch with the company name
2. For each page, generate a screen passing:
   - Complete content from Phase 7
   - Design system from Phase 9
   - Section structure from Phase 6
   - Meta tags and schema markup reference
3. Apply consistent design system across all pages
4. Generate screen variants if applicable

### If Stitch is NOT available:

Export everything as structured markdown files:
```
output/
├── home.md          # Complete content + meta + schema
├── about.md
├── service-[name].md
├── contact.md
├── schema/
│   ├── localbusiness.json
│   ├── breadcrumbs.json
│   └── faq-[service].json
├── design-system.md
└── sitemap-structure.md
```

Inform the user:
```
Google Stitch MCP is not available. All content, schema, and design 
specifications have been exported as markdown files in the output/ directory.
You can use these to build the website in any tool or CMS.
```

---

## Phase 11 — Quality Check

Load `./references/quality-checklist.md` and verify every item.

### If claude-seo is available:
Run `/seo page` on the generated pages (if accessible via URL).

### Manual Checklist:
```
SEO
- [ ] H1 present and keyword-optimized on every page
- [ ] Meta Title set on every page (max 60 chars)
- [ ] Meta Description set on every page (max 155 chars)
- [ ] OG Title and OG Description on every page
- [ ] Primary keyword density 1-2% (not over-optimized)
- [ ] Geo-terms present in first paragraph of each page

SCHEMA
- [ ] LocalBusiness schema with complete NAP
- [ ] LocalBusiness has sameAs for Google Business Profile
- [ ] BreadcrumbList on all pages
- [ ] Service schema on service pages
- [ ] FAQPage schema on pages with FAQ sections

CONTENT
- [ ] E-E-A-T signals present (experience, credentials, local refs)
- [ ] FAQ sections voice-search friendly
- [ ] No duplicate content across pages
- [ ] All placeholder text replaced with real content

INTERNAL LINKING
- [ ] Home links to all service pages
- [ ] Service pages link to Home and Contact
- [ ] Related services cross-linked
- [ ] Meaningful anchor texts (no "click here")
- [ ] NAP identical on ALL pages

CONVERSION
- [ ] At least one CTA visible without scrolling on each page
- [ ] Contact form present on Contact page
- [ ] Google Maps embed reference on Contact page
- [ ] Phone number clickable (tel: link)

TECHNICAL
- [ ] Responsive design specified (375px, 768px, 1024px, 1440px)
- [ ] All images have alt text placeholders with keywords
- [ ] No broken internal links
```

Report any issues found and fix them before proceeding to Phase 12.

---

## Phase 12 — Final Report

Present the complete summary:

```
=== WEBSITE COMPLETE: [Company Name] ===

KEYWORD STRATEGY
Primary Keyword: [KW] — Volume: [x]/month — Difficulty: [x]
Secondary Keywords: [list]
Total geo-cluster: [count] keywords distributed across all pages

PAGES CREATED
- Home — targets: [keywords]
- About — targets: [keywords]
- [Service] — targets: [keywords]
- Contact — targets: [keywords]

SEO TECHNICAL
Schema Markup: LocalBusiness, Service, Breadcrumb, FAQ — all set
Meta Tags: All pages complete
OG Tags: All pages complete
Internal Linking: Complete with documented anchor texts
Mobile-optimized: Specified

DESIGN SYSTEM
Style: [applied style]
Colors: [palette summary]
Typography: [font pairing]

NEXT STEPS FOR THE CLIENT
1. Create or update Google Business Profile
   — Use NAP exactly as on the website
   — Add sameAs URL to schema markup
2. Register website in Google Search Console and submit sitemap
3. Collect 5-10 Google Reviews in the first 30 days
4. Expect first rankings in 3-6 months
5. Monthly SEO monitoring recommended
6. Share website on social media (OG tags are set for preview)

NOTE: This website is correctly structured from day 1.
The foundation for organic growth is in place.
=== END REPORT ===
```
