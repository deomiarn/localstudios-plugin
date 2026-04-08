# Phase 3 — Keyword Research

## Dependency Check
Are `mcp__semrush__*` tools available?
- **No** → Use manual keywords from interview. Inform user. Skip to output.

## Agent
Spawn `keyword-researcher` agent.

## API Call Budget: 5 max

| Priority | Call | When |
|----------|------|------|
| 1 | Keyword Overview (primary KW, user's region) | Always |
| 2 | Keyword Variations (semantically related) | Always |
| 3 | Geo Combinations ([service]+[city/region/district]) | Always if local |
| 4 | Service Keywords (secondary services) | If 2+ distinct services |
| 5 | Competitor Keywords | If competitor URL provided |

Stop when budget exhausted.

## Output

```
| Keyword | Volume/month | Difficulty | Target Page | Type |
|---------|-------------|------------|-------------|------|
| [kw]    | [vol]       | [diff]     | Home        | Primary |
| [kw]    | [vol]       | [diff]     | Service     | Secondary |
| [kw]    | [vol]       | [diff]     | Home+Contact| Geo |
```

Pass keyword table to Phase 4.
