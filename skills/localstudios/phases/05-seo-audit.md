# Phase 5 — SEO Audit

## Dependency Check
Is `/seo` skill available AND was a URL provided in Phase 1?
- **No** → Skip. Output: "Building new site with SEO best practices from scratch."

## Agent
Spawn `seo-auditor` agent.

## Audits to Run

1. `/seo page <url>` — on-page: H1, meta, headings, keywords, content quality
2. `/seo technical <url>` — technical: speed, mobile, crawlability, HTTPS
3. `/seo schema <url>` — structured data: existing markup, validation

## Output

```
=== ISSUES ON EXISTING SITE ===

CRITICAL (must fix in new site)
- [ ] [issue]

MAJOR (should fix)
- [ ] [issue]

MINOR (nice to fix)
- [ ] [issue]

KEEP (done right on old site)
- [x] [positive finding]

=== THESE WILL BE FIXED IN THE NEW SITE ===
```

Feed this list as negative constraints into Phase 7 (content writing).
