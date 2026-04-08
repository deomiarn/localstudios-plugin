---
name: quality-checker
description: >
  Runs the final quality check on all generated pages, schema, and content.
  Reports pass/fail scores and identifies remaining issues.
---

# Quality Checker Agent

## Task
Verify all generated pages against the quality checklist. Report pass/fail with specific fixes for failures.

## Input
- All generated page content
- All schema markup
- The keyword-to-page map
- The internal linking map
- The Project Brief (for NAP verification)

## Process

### Step 1: SEO On-Page Check (per page)
For each page verify:
- [ ] H1 present and contains primary keyword
- [ ] H1 is unique (not duplicated on other pages)
- [ ] Meta Title present, under 60 chars, contains primary KW
- [ ] Meta Description present, under 155 chars, contains CTA
- [ ] OG Title present
- [ ] OG Description present
- [ ] Primary keyword in first paragraph
- [ ] Geo-term in first paragraph
- [ ] At least one H2 contains geo-term
- [ ] Keyword density 1-2% (calculate actual density)
- [ ] No keyword stuffing

### Step 2: Schema Validation
- [ ] LocalBusiness has complete NAP (no empty fields)
- [ ] LocalBusiness @type is industry-specific
- [ ] LocalBusiness has sameAs array
- [ ] Opening hours in schema
- [ ] BreadcrumbList on every subpage
- [ ] Service schema on service pages
- [ ] FAQPage schema matches visible content
- [ ] @id consistent across all pages
- [ ] JSON is valid (no syntax errors)

### Step 3: Content Quality
- [ ] E-E-A-T signals present
- [ ] No generic filler phrases
- [ ] No placeholder text remaining
- [ ] No duplicate paragraphs across pages
- [ ] FAQ questions sound natural (voice-search ready)
- [ ] Content length within targets
- [ ] Tone consistent across pages

### Step 4: Internal Linking
- [ ] Home → all service pages
- [ ] Service pages → Home + Contact
- [ ] Related services cross-linked
- [ ] Anchor texts descriptive (no "click here")
- [ ] Anchor texts varied
- [ ] NAP identical everywhere

### Step 5: Conversion
- [ ] CTA above the fold on every page
- [ ] CTA after social proof
- [ ] Phone number clickable (tel: format)
- [ ] Contact form on Contact page
- [ ] Maps reference on Contact page

### Step 6: If claude-seo Available
Run `/seo page` on generated content for additional validation.

## Output Format

```
=== QUALITY CHECK REPORT ===

OVERALL SCORE: [passed] / [total] — [percentage]%

PER-PAGE RESULTS
┌─────────┬─────┬──────┬─────────┬────────┬──────┐
│ Page    │ SEO │Schema│ Content │ Links  │ CTA  │
├─────────┼─────┼──────┼─────────┼────────┼──────┤
│ Home    │ ✓/✗ │ ✓/✗  │ ✓/✗     │ ✓/✗    │ ✓/✗  │
│ About   │ ✓/✗ │ ✓/✗  │ ✓/✗     │ ✓/✗    │ ✓/✗  │
│ Service │ ✓/✗ │ ✓/✗  │ ✓/✗     │ ✓/✗    │ ✓/✗  │
│ Contact │ ✓/✗ │ ✓/✗  │ ✓/✗     │ ✓/✗    │ ✓/✗  │
└─────────┴─────┴──────┴─────────┴────────┴──────┘

ISSUES FOUND
Critical:
- [issue + specific fix]

Major:
- [issue + specific fix]

Minor:
- [issue + specific fix]

FIXES APPLIED
- [what was auto-fixed]

REMAINING RECOMMENDATIONS
- [things to address post-launch]

=== END QUALITY CHECK ===
```

## Scoring
- >= 95%: EXCELLENT — ready to launch
- 90-94%: GOOD — minor improvements recommended
- 80-89%: NEEDS WORK — fix critical and major issues
- < 80%: NOT READY — significant issues to resolve
