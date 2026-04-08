# Phase 10 — Quality Check (NEW Homepage)

## MANDATORY: Use /seo skills if available

Check if `/seo` skill is loaded.

### If available — YOU MUST RUN THESE:

**Step 1 — Validate the built homepage content:**
```
/seo content [homepage URL or file path]
```

**Step 2 — Validate schema markup:**
```
/seo schema [homepage URL or file path]
```

**Step 3 — Technical check:**
```
/seo technical [homepage URL or file path]
```

Incorporate all findings. Fix issues before proceeding to Phase 11.

**This audits the NEW homepage we just built — NOT the old website.**

**DO NOT skip these just because you "already checked manually".**

---

## Manual Checklist (always run, in addition to /seo)

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
- [ ] E-E-A-T signals present
- [ ] No filler text, no placeholders
- [ ] 600-1000 words total

### Images
- [ ] All images WebP with alt text + title
- [ ] Above-fold images have `priority` prop
- [ ] `width` and `height` set (CLS prevention)

### Conversion
- [ ] CTA above fold (hero section)
- [ ] CTA repeated after social proof
- [ ] Phone number clickable (tel:)
- [ ] NAP visible in footer

### Code Quality
- [ ] No inline `style={}` anywhere
- [ ] All styles in globals.css
- [ ] NAP from siteConfig only
- [ ] `npm run build` passes
- [ ] /frontend-design was applied (not generic AI-looking output)
- [ ] shadcnblocks Blocks were refined (not left as-is)

## Scoring
`[passed] / [total] — [percentage]%`

- ≥ 95%: EXCELLENT
- 90-94%: GOOD
- < 90%: FIX before proceeding
