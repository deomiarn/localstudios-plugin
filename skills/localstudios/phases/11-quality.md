# Phase 11 — Quality Check (Homepage)

## Reference
Load `./references/quality-checklist.md` for complete criteria.

## Check Categories

### SEO On-Page
- [ ] H1 present, contains primary keyword + city
- [ ] Meta Title ≤ 60 chars with primary KW
- [ ] Meta Description ≤ 155 chars with CTA
- [ ] OG Title + OG Description set
- [ ] Primary KW + geo-term in first paragraph
- [ ] Geo-term in at least one H2
- [ ] Keyword density 1-2%

### Schema
- [ ] LocalBusiness complete with sameAs
- [ ] Valid JSON, no empty required fields
- [ ] @type is industry-specific

### Content
- [ ] All 8 homepage sections present in correct order
- [ ] E-E-A-T signals (experience, credentials, local refs)
- [ ] No filler text, no placeholders
- [ ] Tone consistent across sections
- [ ] 600-1000 words total

### Images
- [ ] All images WebP (or served as WebP via next/image)
- [ ] All images have descriptive alt text with keywords
- [ ] All images have title attribute
- [ ] Above-fold images have `priority` prop
- [ ] `width` and `height` set (CLS prevention)
- [ ] No placeholder images remaining

### Conversion
- [ ] CTA above fold (hero section)
- [ ] CTA repeated after social proof
- [ ] Phone number clickable (tel:)
- [ ] NAP visible in footer

### Technical
- [ ] No inline `style={}` anywhere
- [ ] All styles in globals.css
- [ ] NAP from siteConfig only
- [ ] `npm run build` passes

## Scoring
`[passed] / [total] — [percentage]%`

- ≥ 95%: EXCELLENT
- 90-94%: GOOD
- < 90%: FIX before proceeding

Fix issues before Phase 12.
