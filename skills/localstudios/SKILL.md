---
name: localstudios
description: >
  Website generation toolkit by LocalStudios. Use /localstudios generate <url>
  to create a single-page homepage as a client pitch. SEO-optimized, ready to show.
user-invokable: true
argument-hint: "[generate] [url]"
license: MIT
metadata:
  author: LocalStudios
  version: "1.0.0"
  category: website-generation
---

# LocalStudios Plugin

## Commands

| Command | Description |
|---------|-------------|
| `generate <url>` | Generate a single-page homepage as client pitch |

## Routing

Parse the first argument:

- **`generate`** → Execute generate workflow (second arg = URL)
- **No argument** → Show commands table
- **Unknown** → Show "Unknown command." + commands table

---

## Generate Workflow

Creates a **single homepage** to pitch the client. Not a full website — just one strong page that shows what's possible.

Execute phases 0-12 sequentially. Load each phase file **only when that phase begins**.

| Phase | File | Pause? |
|-------|------|--------|
| 0. Preflight | `./phases/00-preflight.md` | WAIT |
| 1. Scrape | `./phases/01-scrape.md` | |
| 2. Interview | `./phases/02-interview.md` | WAIT |
| 3. Keywords | `./phases/03-keywords.md` | |
| 4. Geo-Strategy | `./phases/04-geo-strategy.md` | |
| 5. SEO Audit | `./phases/05-seo-audit.md` | |
| 6. Outline | `./phases/06-outline.md` | WAIT |
| 7. Content | `./phases/07-content.md` | |
| 8. Schema | `./phases/08-schema.md` | |
| 9. Design | `./phases/09-design.md` | |
| 10. Build | `./phases/10-build.md` | |
| 11. QA | `./phases/11-quality.md` | |
| 12. Report | `./phases/12-report.md` | |

### Rules
- Never skip phases — execute in order
- WAIT = do not proceed until user responds
- Load reference files on demand (see each phase)
- This generates ONE page only — the homepage

### Dependencies (all optional)

| Tool | Check | Fallback |
|------|-------|----------|
| Playwright MCP | `mcp__playwright__*` available? | WebFetch |
| Semrush MCP | `mcp__semrush__*` available? | Manual keywords from interview |
| shadcn MCP | `mcp__shadcn__*` available? | Install components via CLI |
| claude-seo | `/seo` skill available? | Skip audit |
| ui-ux-pro-max | `/ui-ux-pro-max` available? | Generic design system |
| Stitch MCP | `mcp__stitch__*` available? | Generate Next.js project directly |

### Tech Stack
- **Next.js** App Router (initialized via `npx shadcn@latest init --preset b0 --template next`)
- **shadcn/ui** for all UI components
- **globals.css** — single source of truth for ALL styles (never inline)
- See `./references/code-standards.md` for full standards
