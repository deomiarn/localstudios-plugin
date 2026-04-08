# Phase 10 — Quality Check + SEO Audit

## This phase has TWO parts: QA Checklist + SEO Audit with fixes.

---

## Part 1 — QA Checklist

### SEO On-Page
- [ ] H1 present, contains primary keyword + city
- [ ] Meta Title ≤ 60 chars with primary KW
- [ ] Meta Description ≤ 155 chars with CTA + location
- [ ] OG Title + OG Description set
- [ ] Primary KW + geo-term in first paragraph
- [ ] Geo-term in at least one H2
- [ ] Keyword density 1-2%

### Schema
- [ ] LocalBusiness complete with sameAs
- [ ] @type is industry-specific
- [ ] Valid JSON
- [ ] Opening hours present

### Content
- [ ] All 8 sections present in correct order
- [ ] 1500-2500 words total
- [ ] E-E-A-T signals present
- [ ] No filler/placeholder text
- [ ] Tone consistent

### Images
- [ ] All images have alt text + title
- [ ] Above-fold images have `priority`
- [ ] No empty spaces — styled placeholders where images missing

### UI Rules
- [ ] NO hardcoded colors — all via shadcn variables
- [ ] NO inline `style={}` props
- [ ] Fonts via font-heading, font-body only
- [ ] Hover states consistent
- [ ] globals.css matches MASTER.md from Phase 8

### shadcnblocks
- [ ] Block files are the actual components (not replaced)
- [ ] Block content replaced with Phase 6 content
- [ ] Block structure preserved

### Design (MASTER.md compliance)
- [ ] Style matches what ui-ux-pro-max recommended
- [ ] Colors match MASTER.md palette
- [ ] Anti-patterns from MASTER.md not violated
- [ ] /frontend-design was applied
- [ ] Page has visual identity — not generic AI output

### Conversion
- [ ] CTA above fold
- [ ] CTA repeated after social proof
- [ ] Phone clickable (tel:)
- [ ] NAP in footer matches schema

### Technical
- [ ] `npm run build` passes
- [ ] lang attribute correct
- [ ] Metadata export in page.tsx

---

## Part 2 — MANDATORY: /seo Audit of the NEW Homepage

**If `/seo` skill is available — YOU MUST RUN THESE. No exceptions.**

### Step 1 — Run SEO analysis
```
/seo page [path to built homepage or localhost URL]
```

### Step 2 — Run schema validation
```
/seo schema [path to built homepage or localhost URL]
```

### Step 3 — Run content check
```
/seo content [path to built homepage or localhost URL]
```

### Step 4 — FIX EVERYTHING the audit finds

**Do NOT just report issues. FIX them immediately.**

For each issue found:
1. Read the issue
2. Identify which file needs to change
3. Edit the file to fix it
4. Verify the fix

Common fixes:
- Meta title too long → shorten in layout.tsx metadata
- Missing alt text → add to image component
- Schema validation error → fix in lib/schema.ts
- Keyword density off → adjust content in block files
- H2 missing geo-term → edit block content

### Step 5 — Re-run audit after fixes

If critical issues were found and fixed, run `/seo page` again to verify.

---

## Scoring

Count passed items from Part 1 + Part 2 results:

`[passed] / [total] — [percentage]%`

- ≥ 95%: EXCELLENT — ready to present
- 90-94%: GOOD — minor tweaks
- < 90%: FIX before proceeding to Phase 11

**Do NOT proceed to Phase 11 with unfixed critical issues.**
