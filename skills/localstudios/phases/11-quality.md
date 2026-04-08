# Phase 11 — Quality Check

## Reference
Load `./references/quality-checklist.md` for complete criteria.

## Agent
Spawn `quality-checker` agent.

## Check Categories

### SEO On-Page (per page)
- H1 present, keyword-optimized, unique across site
- Meta Title ≤ 60 chars with primary KW
- Meta Description ≤ 155 chars with CTA
- OG Title + OG Description set
- Primary KW + geo-term in first paragraph
- Geo-term in at least one H2
- Keyword density 1-2%

### Schema
- LocalBusiness complete with sameAs
- BreadcrumbList on all subpages
- Service + FAQPage on service pages
- FAQ text matches visible content
- @id consistent, JSON valid

### Content
- E-E-A-T signals present
- No filler, no placeholders, no duplicates
- FAQ voice-search ready
- Length within targets
- Tone consistent

### Links
- Home → all services, services → Home + Contact
- Cross-links between related services
- Keyword-rich anchors, varied, no "click here"
- NAP identical everywhere

### Images
- All images are WebP format (or served as WebP via next/image)
- All images have descriptive, keyword-relevant alt text
- All images have title attribute
- No alt text is empty or generic ("image", "photo", "IMG_1234")
- Alt text includes city/location where contextually appropriate
- All images ≥ 1024px on longest side
- Above-fold images have `priority` prop (Next.js)
- Below-fold images lazy-loaded (automatic in Next.js)
- `width` and `height` set on all images (prevents CLS)
- No placeholder images remaining
- Image sources documented in docs/pages/[page].md
- File names follow convention: `[page]-[section]-[descriptor].webp`

### Sections
- Home has all 8 mandatory sections (see page-sections.md)
- About has all 6 mandatory sections
- Each Service page has all 7 mandatory sections
- Contact has all 5 mandatory sections
- Sections are in correct order
- No sections missing or empty

### Conversion
- CTA above fold on every page
- Phone clickable (tel:)
- Contact form + Maps on Contact page

## Scoring
`[passed] / [total] — [percentage]%`

- ≥ 95%: EXCELLENT
- 90-94%: GOOD — minor fixes
- 80-89%: NEEDS WORK — fix critical/major
- < 80%: NOT READY

Fix critical issues before proceeding to Phase 12.
