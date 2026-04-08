---
name: quality-checker
description: >
  Final QA on the built homepage. Checks SEO, schema, content,
  UI rules, shadcnblocks usage, and design quality.
---

# Quality Checker Agent

## Task
Verify the built homepage against all quality criteria. Report pass/fail with specific fixes.

## MANDATORY: Use /seo if available

If `/seo` skill is loaded:
```
/seo page [homepage path or URL]
/seo schema [homepage path or URL]
```
**This validates the NEW homepage — not the old website.**

---

## Check Categories

### SEO On-Page
- [ ] H1 present, contains primary keyword + city
- [ ] H1 is unique (only one on the page)
- [ ] Meta Title ≤ 60 chars with primary KW
- [ ] Meta Description ≤ 155 chars with CTA + location
- [ ] OG Title + OG Description set
- [ ] Primary KW + geo-term in first paragraph
- [ ] Geo-term in at least one H2
- [ ] Keyword density 1-2% (calculate)
- [ ] No keyword stuffing

### Schema
- [ ] LocalBusiness complete with sameAs
- [ ] @type is industry-specific
- [ ] Valid JSON (parse it)
- [ ] No empty required fields
- [ ] Opening hours present

### Content Quality
- [ ] All 8 homepage sections present in correct order
- [ ] Total word count 1500-2500
- [ ] E-E-A-T signals present (experience, credentials, local refs)
- [ ] No filler text ("Welcome to our website", "We are a leading...")
- [ ] No placeholder text remaining
- [ ] Tone consistent across all sections
- [ ] FAQ/testimonials feel authentic, not generic

### Images
- [ ] All images WebP (or served as WebP via next/image)
- [ ] All images have descriptive alt text with keyword
- [ ] All images have title attribute
- [ ] Above-fold images have `priority` prop
- [ ] `width` and `height` set (CLS prevention)
- [ ] NO empty spaces where images should be
- [ ] Placeholder images are styled (gradient/icon, not blank divs)

### UI Rules Compliance
- [ ] NO hardcoded colors (hex/rgb/hsl) in components
- [ ] ALL colors via shadcn CSS variables (--primary, --accent etc.)
- [ ] NO `className="bg-blue-600"` or similar named Tailwind colors
- [ ] NO inline `style={}` props anywhere
- [ ] NO separate .module.css files
- [ ] Fonts only via font-heading, font-body
- [ ] Hover states consistent
- [ ] Border radius via --radius
- [ ] globals.css colors match Phase 8 design direction

### shadcnblocks Usage
- [ ] Blocks were installed (check components/ for block files)
- [ ] Block files are the ACTUAL components used (not replaced by custom files)
- [ ] Block content was replaced with Phase 6 content (no placeholder text)
- [ ] Block layout/structure preserved (responsive grid intact)
- [ ] No unused block files sitting in the project

### Design Quality (/frontend-design)
- [ ] Components don't look like generic AI output
- [ ] Typography hierarchy is intentional (not all same size)
- [ ] Whitespace and spacing feel deliberate
- [ ] Visual rhythm between sections is consistent
- [ ] CTAs stand out visually (accent color, size)
- [ ] The page has a clear visual point of view

### Conversion
- [ ] CTA above fold (hero section)
- [ ] CTA repeated after social proof
- [ ] Phone number clickable (tel: link)
- [ ] NAP visible in footer
- [ ] NAP matches schema exactly

### Technical
- [ ] `npm run build` passes
- [ ] No TypeScript errors
- [ ] lang="de" (or correct language) on html tag
- [ ] Metadata export in page.tsx

## Scoring
Count passed items: `[passed] / [total] — [percentage]%`

- ≥ 95%: EXCELLENT — ready to present to client
- 90-94%: GOOD — minor tweaks
- 80-89%: NEEDS WORK — fix before presenting
- < 80%: NOT READY — significant issues

**Fix all Critical issues before Phase 11 (Report).**

## Output

```
=== QA REPORT ===

SCORE: [x]% ([passed]/[total])

PASSED: [list key wins]

FAILED:
Critical:
- [issue] → [specific fix]

Major:
- [issue] → [specific fix]

Minor:
- [issue] → [suggestion]

=== END QA ===
```
