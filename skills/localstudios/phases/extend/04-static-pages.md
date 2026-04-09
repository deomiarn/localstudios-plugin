# Extend Phase 4 — Static Pages

## Task
Build all static pages. Use same design system, shadcnblocks, /frontend-design as homepage.

## Pages to Build (parallel where possible)

### Services Overview — `/leistungen/page.tsx`
Load `./references/page-templates/services-overview.md`
- Grid of all services with images, titles, short descriptions
- Featured services highlighted at top
- Link to each `/leistungen/[slug]` detail page
- SEO: "Unsere Leistungen — [Company] [City]"

### About — `/ueber-uns/page.tsx`
Load `./references/page-templates/about.md`
- Company story (founding, mission, values)
- Why this location, local connection
- Credentials, certifications, memberships
- E-E-A-T signals throughout
- Link to Team page
- Min 3 images

### Team Overview — `/team/page.tsx`
Load `./references/page-templates/team.md`
- Grid of team members with photo, name, role
- Link to each `/team/[slug]` detail page
- Warm, personal tone

### Regions Overview — `/regionen/page.tsx`
Load `./references/page-templates/regions.md`
- Map or visual of service area
- List/grid of all regions served
- Link to each `/regionen/[slug]` detail page
- Strong GEO signals

### FAQ — `/faq/page.tsx`
Load `./references/page-templates/faq.md`
- Accordion sections grouped by category
- FAQPage schema markup
- Voice-search friendly questions
- Internal links to relevant service pages

### Blog Overview — `/blog/page.tsx`
Load `./references/page-templates/blog.md`
- All posts with cover image, title, excerpt, date
- Category filter (buttons/tabs, NOT pill badges)
- Clean grid layout
- Pagination or infinite scroll

### Contact — `/kontakt/page.tsx`
Load `./references/page-templates/contact.md`
- NAP prominent
- Contact form (fields from interview)
- Google Maps embed (URL from interview)
- Opening hours
- Additional contact methods
- ContactPage schema

### Impressum — `/impressum/page.tsx`
- Legal company info from interview
- Standard Swiss/German/Austrian Impressum format
- No design flair needed — clean, readable

### Datenschutz — `/datenschutz/page.tsx`
- Privacy policy based on tools/hosting from interview
- GDPR/DSG compliant template
- Sections: data collection, cookies, analytics, rights, contact

### 404 — `app/not-found.tsx`
- Branded, matches design system
- Helpful: search, link to home, popular pages
- Not generic — reflects the brand personality

## Rules for ALL pages
- Same globals.css design system as homepage
- Same header/footer components
- Sections centered (mx-auto max-w-7xl)
- Buttons readable on all backgrounds
- No pill badges
- Min 3 images per page (placeholder where needed)
- Metadata export with SEO title + description + OG tags
- BreadcrumbList schema on every page
