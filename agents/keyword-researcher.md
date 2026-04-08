---
name: keyword-researcher
description: >
  Keyword research via Semrush MCP. Max 5 API calls.
  All keywords target the homepage (single-page build).
---

# Keyword Research Agent

## Task
Research keywords for the homepage using Semrush MCP (max 5 calls).

## Rules
- **Max 5 API calls.** Track count.
- **MUST use Semrush** if `mcp__semrush__*` tools are available. No guessing volumes.
- If Semrush not available → return manual keywords from interview as-is.
- All keywords target ONE page — the homepage.

## Process

**Call 1** — Keyword Overview (ALWAYS)
- Query: primary keyword from interview
- Region: user's country
- Get: volume, difficulty, CPC, intent

**Call 2** — Keyword Variations (ALWAYS)
- Get: 5-10 semantically related keywords with volume
- Filter: relevant to the business, reasonable difficulty

**Call 3** — Geo Combinations (ALWAYS for local)
- [service] + [city], [service] + [region], [service] + [district]
- Get: volume per combination

**Call 4** (optional) — Secondary service keyword
- Only if user has 2+ distinct services

**Call 5** (optional) — Competitor keywords
- Only if competitor URL was provided

## Output

```
=== KEYWORDS ===

API Calls: [x]/5

| Keyword | Volume | Difficulty | Role |
|---------|--------|------------|------|
| [kw] | [x] | [x] | Primary |
| [kw] | [x] | [x] | Secondary |
| [kw] | [x] | [x] | Geo |

ALL target: Homepage
=== END ===
```
