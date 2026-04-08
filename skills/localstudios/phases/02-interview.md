# Phase 2 — User Interview

**⚠ MANDATORY PAUSE — Do not proceed until user responds.**

## References
- Load `./references/interview-template.md` for the checklist format
- Load `./references/docs-structure.md` for BUSINESS.md template

## Process

1. Present structured checklist to user
2. Prefill values from Phase 1: `[FOUND — please confirm]: <value>`
3. Mark missing required fields: `[MISSING — required]`
4. Mark missing optional fields: `[MISSING — optional]`

## GBP Scraping (before interview)

If user provided a Google Business Profile URL or one was found in Phase 1:
- Use WebFetch to scrape the GBP page
- Extract: business name, address, phone, opening hours, category, reviews count, rating
- Prefill these in the interview with `[FROM GBP — please confirm]`

If no GBP URL but business name + city are known:
- Try WebFetch on `https://www.google.com/maps/search/[business+name]+[city]`
- If found, extract data and prefill

## Checklist Sections

**BUSINESS**: name, industry, location, service area, founding year
**SEO**: primary keyword (required), secondary keywords, main service, target audience, competitors
**DESIGN**: style, colors, logo, reference website
**PAGES**: page list, separate service pages, blog section, additional pages
**CONVERSION**: website goal (call/form/booking/purchase), promotions
**CONTACT** (for schema): legal name, full address, phone, email, hours, GBP URL
**TECHNICAL**: language(s), domain name, existing GBP

## After Response

### 1. Compile and show PROJECT BRIEF

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
Design: [style or "AI decides"]
NAP: [name] | [address] | [phone]
=== END BRIEF ===
```

Ask user to confirm before proceeding.

### 2. Create docs/BUSINESS.md

After confirmation, create `docs/BUSINESS.md` using the template from `./references/docs-structure.md`.
Fill in ALL fields from the interview + scraping. This file becomes the **single source of truth** for all business data.

```
docs/
└── BUSINESS.md    ← created now
```

### 3. Update CLAUDE.md

Add docs references to the project CLAUDE.md:

```markdown
## Project Documentation
- **Read `docs/BUSINESS.md`** for all business facts (NAP, hours, services)
- **Read `docs/SEO-STRATEGY.md`** for keyword targets and geo strategy
- **Read `docs/pages/[page].md`** before editing any page
- **Update docs/** when making changes — keep in sync with code
```
