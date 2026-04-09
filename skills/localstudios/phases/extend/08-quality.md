# Extend Phase 8 — QA + SEO Audit

## MANDATORY: Full site audit

### /seo Audit (if available)
Run on EVERY page:
```
/seo page [url/path] — for each page
/seo schema [url/path] — schema validation
/seo content [url/path] — content quality
/seo technical [url/path] — technical SEO
```

**FIX all issues immediately. Re-run after fixes.**

### Manual Checklist (every page)

**SEO**
- [ ] Unique H1 with keyword + city
- [ ] Meta Title ≤ 60 chars, unique per page
- [ ] Meta Description ≤ 155 chars, unique per page
- [ ] OG tags set
- [ ] BreadcrumbList schema on every subpage

**Schema**
- [ ] LocalBusiness on home + contact
- [ ] Service schema on service pages
- [ ] FAQPage schema on FAQ + service pages
- [ ] Person schema on team pages
- [ ] BlogPosting schema on blog posts
- [ ] BreadcrumbList on ALL subpages

**Content**
- [ ] No duplicate content across pages
- [ ] Each page has unique, substantial content
- [ ] Internal links between related pages
- [ ] No placeholder text remaining

**Design**
- [ ] All pages use same design system
- [ ] Sections centered on every page
- [ ] Buttons readable everywhere
- [ ] No pill badges
- [ ] Images or placeholders (min 3 per page)

**Technical**
- [ ] sitemap.xml includes all pages
- [ ] robots.txt correct
- [ ] llms.txt complete
- [ ] 404 page branded
- [ ] `npm run build` passes
- [ ] No TypeScript errors
- [ ] Sanity queries working

**PageSpeed**
- [ ] Run Lighthouse or PageSpeed Insights
- [ ] Images optimized (next/image)
- [ ] No render-blocking resources
- [ ] Fonts preloaded
- [ ] Core Web Vitals acceptable

## Scoring
- Per page: pass/fail per category
- Overall: `[passed] / [total] — [percentage]%`
- ≥ 90%: Ready for release
- < 90%: Fix before release
