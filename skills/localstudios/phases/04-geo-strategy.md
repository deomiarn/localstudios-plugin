# Phase 4 — Multi-Keyword Geo-Strategy

## Reference
Load `./references/seo-rules.md` for cluster logic and embedding rules.

## Principle
One strong page beats five thin pages. Each page targets a semantic cluster, not a single keyword.

## Cluster Assignment

**Home**: Brand + Primary KW + City + Region
**Service Pages**: [Service] + City + District + Region variations
**Contact**: [Service] + contact/appointment/directions + City
**About**: Brand + trust/experience keywords + location

## Geo-Term Embedding Rules

- Geo-term in first paragraph of every page
- Geo-term in at least one H2 per page
- District/neighborhood names in "serving" or "how to find us" sections
- Region name in business description
- Max 3 repetitions of the same geo-term per page

## Output

Keyword-to-page assignment table:

```
| Page | Primary KW | Secondary KWs | Geo KWs |
|------|-----------|---------------|---------|
| Home | [kw] | [list] | [list] |
| [Service] | [kw] | [list] | [list] |
| Contact | [kw] | [list] | [list] |
| About | [kw] | [list] | [list] |
```

Show table to user before proceeding.

## Create docs/SEO-STRATEGY.md

After completing the keyword-to-page map, create `docs/SEO-STRATEGY.md` using the template from `./references/docs-structure.md`. Include:
- Primary keyword with volume/difficulty
- Full keyword map table with page assignments, types, and intent
- Geo targeting summary (primary city, districts, region)
- Internal linking map with anchor texts
- Competitor info if available
- Data source (Semrush / Manual / Interview) and date
