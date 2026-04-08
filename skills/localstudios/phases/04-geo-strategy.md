# Phase 4 — Keyword Geo-Strategy for Homepage

## Reference
Load `./references/seo-rules.md` for cluster logic and embedding rules.

## Principle
The homepage targets a semantic keyword cluster — not a single keyword.

## Homepage Cluster
All keywords land on this one page:
- Brand + Primary KW + City + Region
- Main services mentioned (not separate pages — just listed)
- "Near me" variations
- Trust keywords: "experienced", "certified", "since [year]"

## Geo-Term Embedding Rules
- City name in H1 + first paragraph
- District/neighborhood in one H2 or service mention
- Region/state in the "about" or "local trust" section
- Max 3 repetitions of the same geo-term

## Output

Homepage keyword list:

```
PRIMARY: [keyword] — for H1 + first paragraph
SECONDARY: [kw1], [kw2], [kw3] — for H2s and body
GEO: [city], [district], [region] — distributed across sections
```

Show to user before proceeding.

## Create docs/SEO-STRATEGY.md

Create `docs/SEO-STRATEGY.md` using the template from `./references/docs-structure.md`. Include:
- Primary keyword with volume/difficulty
- All keywords mapped to homepage sections
- Geo targeting summary
- Data source and date
