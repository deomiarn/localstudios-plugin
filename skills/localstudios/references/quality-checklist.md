# Quality Checklist

Run through every item after website generation. Report pass/fail for each.

---

## SEO On-Page

- [ ] H1 present on every page
- [ ] H1 contains primary keyword of that page
- [ ] H1 is unique per page (no duplicate H1s across site)
- [ ] Meta Title set on every page (max 60 chars, contains primary KW)
- [ ] Meta Description set on every page (max 155 chars, contains CTA)
- [ ] OG Title set on every page
- [ ] OG Description set on every page
- [ ] Primary keyword in first paragraph of every page
- [ ] Geo-term in first paragraph of every page
- [ ] At least one H2 contains a geo-term per page
- [ ] Keyword density 1-2% (not over-optimized)
- [ ] No keyword stuffing detected
- [ ] Image alt texts contain relevant keywords

## Schema Markup

- [ ] LocalBusiness schema present with complete NAP
- [ ] LocalBusiness @type is industry-specific where applicable
- [ ] LocalBusiness has `sameAs` array (Google Business Profile, social)
- [ ] Opening hours specified in schema
- [ ] BreadcrumbList schema on all subpages
- [ ] Service schema on each service page
- [ ] FAQPage schema on pages with FAQ sections
- [ ] FAQ schema content matches visible page content exactly
- [ ] Organization/Person schema on About page
- [ ] All schema validates (no empty required fields)
- [ ] @id references consistent across pages

## Content Quality

- [ ] Written from experience perspective (E-E-A-T)
- [ ] Credentials and expertise mentioned (About page)
- [ ] Local references present (community, landmarks, neighborhoods)
- [ ] Concrete numbers included where available (years, clients, etc.)
- [ ] No generic filler text ("Welcome to our website", "We are a leading...")
- [ ] No placeholder text remaining
- [ ] No duplicate content across pages
- [ ] FAQ sections voice-search friendly (natural question phrasing)
- [ ] Tone is consistent across all pages
- [ ] Content length meets guidelines (Home 1500-2000w + optional FAQ 200-300w, Service 800-1200w)

## Internal Linking

- [ ] Home links to all service pages
- [ ] All service pages link back to Home
- [ ] All service pages link to Contact
- [ ] Related service pages cross-link each other
- [ ] Anchor texts are keyword-rich and descriptive
- [ ] No "click here" anchor texts
- [ ] Anchor texts varied (not identical for same target)
- [ ] Internal linking map matches the plan from Phase 6

## NAP Consistency

- [ ] Business name identical on ALL pages
- [ ] Address identical on ALL pages
- [ ] Phone number identical on ALL pages (same format everywhere)
- [ ] Email identical on ALL pages
- [ ] NAP in footer matches NAP in schema
- [ ] NAP in Contact page matches NAP in schema

## Conversion

- [ ] At least one CTA visible without scrolling (above the fold) per page
- [ ] CTA repeated after social proof section
- [ ] CTA text is action-oriented ("Book", "Call", "Get Quote")
- [ ] Phone number is clickable (tel: link)
- [ ] Contact form present on Contact page
- [ ] Google Maps embed reference on Contact page
- [ ] Opening hours displayed on Contact page

## Technical

- [ ] Responsive design specified for 375px, 768px, 1024px, 1440px
- [ ] No broken internal links
- [ ] Language attribute set correctly
- [ ] Canonical URLs logical
- [ ] Sitemap structure documented

## Scoring

Count passed items. Report as: `[passed] / [total] — [percentage]%`

If score < 90%: List all failed items with specific fix instructions.
If score >= 90%: Proceed to final report, note remaining items as recommendations.
