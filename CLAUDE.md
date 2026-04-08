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

### /frontend-design (MANDATORY in Phase 9)
- Use for EVERY component — distinctive, not generic
- Pick a design direction and commit
- Typography, composition, motion, atmosphere must be intentional
- Anti-patterns: no SaaS hero clones, no generic card piles

### shadcnblocks (MANDATORY in Phase 9)
- Install Blocks via `npx shadcn add @shadcnblocks/[name]`
- Blocks are starting points — MUST be refined with /frontend-design
- EDIT the block files directly — NEVER create new files that replace them
- Replace placeholder content with Phase 6 content
- Replace hardcoded colors with shadcn CSS variables

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
