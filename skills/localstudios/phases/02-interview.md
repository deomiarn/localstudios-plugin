# Phase 2 — User Interview

**⚠ MANDATORY PAUSE — Do not proceed until user responds.**

## References
- Load `./references/interview-template.md` for checklist format
- Load `./references/docs-structure.md` for BUSINESS.md template

## Context
We're building a **single homepage** as a client pitch. Only ask what's needed for that.

## Process

1. Present checklist — prefill from Phase 1 scraping
2. `[FOUND — please confirm]: <value>` for scraped data
3. `[MISSING — required]` for essential gaps
4. Wait for response

## Checklist

```
=== HOMEPAGE GENERATOR: INFORMATION CHECK ===

BUSINESS
[ ] Company name: ___
[ ] Industry: ___
[ ] Main location (city + region): ___
[ ] Year founded: ___

SEO
[ ] Primary Keyword (e.g. "Zahnarzt Zürich"): ___ [REQUIRED]
[ ] 2-3 Secondary Keywords: ___
[ ] Target audience: ___

DESIGN
[ ] Style preference — or "AI decides": ___
[ ] Colors — keep existing or new: ___

CONTACT (for schema + footer)
[ ] Full address: ___
[ ] Phone: ___
[ ] Email: ___
[ ] Opening hours: ___
[ ] Google Business Profile URL: ___

SERVICES (for overview section)
[ ] Main services (3-6): ___

TRUST
[ ] Years experience / clients served / Google rating: ___
[ ] Certifications or awards: ___

CONVERSION
[ ] Main goal: Call / Form / Book / Purchase: ___

=== ANSWER THESE — THEN WE BUILD ===
```

## After Response

### 1. Show PROJECT BRIEF
```
=== PROJECT BRIEF ===
Business: [name] — [industry] — [city]
Primary Keyword: [keyword]
Services: [list]
Goal: [call/form/booking]
=== END BRIEF ===
```

Confirm with user.

### 2. Create docs/BUSINESS.md
Fill from interview + scraping data. Single source of truth for all business facts.

### 3. Update CLAUDE.md
Add docs references.
