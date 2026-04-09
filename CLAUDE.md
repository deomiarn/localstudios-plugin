# LocalStudios Plugin

Homepage generation plugin. Next.js + shadcn/ui + shadcnblocks + /frontend-design.

## Project Documentation
- **`docs/BUSINESS.md`** — all business facts
- **`docs/SEO-STRATEGY.md`** — keyword targets
- **`docs/pages/home.md`** — homepage: keywords, SEO, GEO
- **`design-system.md`** — colors, fonts, atmosphere (from Design Brief)

## Agents (5)
| Agent | Phase | Purpose |
|-------|-------|---------|
| `scraper` | 1 | Quick website scrape (max 7 requests, Playwright) |
| `keyword-researcher` | 3 | Semrush keyword research (max 5 API calls) |
| `content-writer` | 6 | Extensive homepage content (1500-2500 words) |
| `schema-generator` | 7 | JSON-LD schema (LocalBusiness, WebSite) |
| `quality-checker` | 10 | QA + /seo audit with fixes |

## Design Process

```
Design Brief (Referenz-Screenshots + Adjektive + Farben)
  → design-system.md (Farben, Fonts, Spacing, Anti-Patterns)
    → shadcnblocks (Fundament — Blöcke passend zur Referenz)
      → /frontend-design (Feintuning — unverwechselbar)
        → /seo audit (Prüfung + sofortige Fixes)
```

## Skills — ALWAYS USE

### /frontend-design (MANDATORY Phase 9)
- Use for EVERY component — distinctive, non-generic
- Pick a direction and commit (see skill for options)
- Anti-patterns: no SaaS hero clones, no generic card piles

### shadcnblocks (FUNDAMENT Phase 9)
- Install Blocks: `npx shadcn add @shadcnblocks/[name]`
- EDIT block files directly — NEVER create new files
- Replace content + hardcoded colors, keep structure

### /liquid-glass (optional effect)
- Glassmorphism/frosted-glass effects where fitting
- Use tastefully — not on everything, but where it adds depth

### /seo (AUDIT Phase 10)
- /seo page + /seo schema + /seo content
- Must FIX all issues found

## UI Rules — CRITICAL

Read `references/ui-rules.md` before ANY component code.

- ALL styles in `app/globals.css`
- ALL colors via shadcn CSS variables
- NEVER hardcode hex/rgb in components
- NEVER inline style={}
- NEVER leave empty spaces — styled placeholders
- Minimum 5 images on homepage
- Fonts only via globals.css
- Hover states consistent

## NAP Data
`lib/site-config.ts` — single source of truth.
