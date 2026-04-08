# LocalStudios Plugin

Claude Code plugin for homepage generation. Next.js + shadcn/ui + shadcnblocks.

## Project Documentation
- **`docs/BUSINESS.md`** — all business facts (NAP, hours, services, USPs)
- **`docs/SEO-STRATEGY.md`** — keyword targets, geo strategy
- **`docs/pages/home.md`** — homepage: purpose, keywords, SEO, GEO

## Skills — ALWAYS USE WHEN AVAILABLE

### /frontend-design (MANDATORY for Phase 9)
- Use for EVERY component — makes code distinctive, not generic
- Pick a design direction and commit (see skill for options)
- Typography, composition, motion, atmosphere must be intentional
- Anti-patterns: no SaaS hero clones, no generic card piles

### shadcnblocks (MANDATORY for Phase 9)
- Install Blocks as starting points: `npx shadcn add @shadcnblocks/[name]`
- Blocks are ROHLINGE — must be refined with /frontend-design
- Replace all placeholder content with Phase 6 content
- Replace all hardcoded colors with shadcn CSS variables

## UI Rules — CRITICAL

### Read `references/ui-rules.md` before writing ANY component code.

Key rules:
- **ALL styles in `app/globals.css`** — read it before any change
- **ALL colors via shadcn CSS variables** (--primary, --accent etc.)
- **NEVER** hardcode hex/rgb/hsl values in components
- **NEVER** use `className="bg-blue-600"` — use `className="bg-primary"`
- **NEVER** use inline `style={}` props
- **NEVER** create separate CSS files
- **NEVER** leave empty spaces where images should be — use stylish placeholders
- Fonts only via globals.css (`font-heading`, `font-body`)
- Hover states consistent across all components
- Border radius via `--radius` variable

## NAP Data
`lib/site-config.ts` AND `docs/BUSINESS.md` — single source of truth.
Components read from site-config.ts — never hardcode in multiple places.
