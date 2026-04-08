# LocalStudios Plugin

Homepage generation plugin. Next.js + shadcn/ui + shadcnblocks + /frontend-design.

## Project Documentation
- **`docs/BUSINESS.md`** — all business facts (NAP, hours, services, USPs)
- **`docs/SEO-STRATEGY.md`** — keyword targets, geo strategy
- **`docs/pages/home.md`** — homepage: purpose, keywords, SEO, GEO

## Agents (5)
| Agent | Phase | Purpose |
|-------|-------|---------|
| `scraper` | 1 | Quick website scrape (max 7 requests, Playwright MCP) |
| `keyword-researcher` | 3 | Semrush keyword research (max 5 API calls) |
| `content-writer` | 6 | Extensive homepage content (1500-2500 words, E-E-A-T) |
| `schema-generator` | 7 | JSON-LD schema (LocalBusiness, WebSite) |
| `quality-checker` | 10 | Final QA (SEO, UI rules, shadcnblocks, design quality) |

## Skills — ALWAYS USE WHEN AVAILABLE

### /ui-ux-pro-max (LEITET — Phase 8 + 9)
- Generates the Design System (MASTER.md) — 67 styles, 97 palettes, industry rules
- ALL design decisions come from MASTER.md
- Reviews the built homepage at end of Phase 9
- Hierarchy: ui-ux-pro-max > shadcnblocks > frontend-design

### shadcnblocks (FUNDAMENT — Phase 9)
- Install Blocks via `npx shadcn add @shadcnblocks/[name]`
- Blocks are structural starting points — not the final product
- EDIT the block files directly — NEVER create new files that replace them
- Replace content + colors, keep structure

### /frontend-design (FEINTUNING — Phase 9)
- Refines blocks WITHIN the MASTER.md design rules
- Komposition, Atmosphäre, Motion, Typografie
- Must respect MASTER.md style + anti-patterns

### /seo (AUDIT — Phase 10)
- Runs /seo page, /seo schema, /seo content on the NEW homepage
- Must FIX all issues found, not just report them

## UI Rules — CRITICAL

### Read `references/ui-rules.md` before writing ANY component code.

- **ALL styles in `app/globals.css`** — read before any change
- **ALL colors via shadcn CSS variables** (--primary, --accent etc.)
- **NEVER** hardcode hex/rgb/hsl in components
- **NEVER** use `className="bg-blue-600"` — use `className="bg-primary"`
- **NEVER** use inline `style={}` props
- **NEVER** leave empty spaces where images should be — use styled placeholders
- Fonts only via globals.css (`font-heading`, `font-body`)
- Hover states consistent across all components

## NAP Data
`lib/site-config.ts` AND `docs/BUSINESS.md` — single source of truth.
Never hardcode NAP in multiple places.

## Development
- Keep SKILL.md concise — detail in phases/ and references/
- All external dependencies have fallbacks
- Agents are spawned by phases, not called directly
