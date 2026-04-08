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
  version: "1.2.0"
  category: website-generation
---

# LocalStudios Plugin

## Commands

| Command | Description |
|---------|-------------|
| `generate <url>` | Generate a single-page homepage as client pitch |

## Routing

- **`generate`** → Execute generate workflow (second arg = URL)
- **No argument** → Show commands table
- **Unknown** → Show "Unknown command." + commands table

---

## Generate Workflow

Creates a **single homepage** to pitch the client. Run phases in parallel where possible.

| Phase | File | Pause? | Group |
|-------|------|--------|-------|
| 0. Preflight | `./phases/00-preflight.md` | WAIT | — |
| 1. Scrape | `./phases/01-scrape.md` | | — |
| 2. Interview | `./phases/02-interview.md` | WAIT | — |
| 3. Keywords | `./phases/03-keywords.md` | | — |
| 4. Geo-Strategy | `./phases/04-geo-strategy.md` | | after 3 |
| 5. Outline | `./phases/05-outline.md` | WAIT | — |
| 6. Content | `./phases/06-content.md` | | ⬇ parallel |
| 7. Schema | `./phases/07-schema.md` | | ⬇ parallel with 6 |
| 8. Design | `./phases/08-design.md` | | ⬇ parallel with 6+7 |
| 9. Build | `./phases/09-build.md` | | — (needs 6+7+8) |
| 10. QA | `./phases/10-quality.md` | | — |
| 11. Report | `./phases/11-report.md` | | — |

### Parallelization

- **Phases 6+7+8 parallel**: Content, schema, and design simultaneously
- **Phase 9 waits for 6+7+8**: Build needs all three

**Use subagents for parallel work.**

### MANDATORY Skill & Tool Usage

**If a skill or tool is available, you MUST use it. Never skip.**

| Tool | Phase | Action |
|------|-------|--------|
| `/frontend-design` | Phase 9 | MUST use for every component — distinctive design, not generic |
| shadcnblocks | Phase 9 | MUST install Blocks as starting points, then refine |
| `/seo page` | Phase 10 | MUST validate the NEW homepage |
| `/seo schema` | Phase 10 | MUST validate schema markup |
| Semrush MCP | Phase 3 | MUST use for keyword research |
| shadcn MCP | Phase 9 | MUST use to install base + blocks |

**Fallbacks ONLY for genuinely unavailable tools.**

### Rules
- WAIT = do not proceed until user responds
- ONE page only — the homepage
- No screenshots — user checks localhost
- No `npm run dev` — just `npm run build`
- Load `./references/ui-rules.md` in Phase 9 — ALL code must follow these rules
- Images: if scraping fails, use stylish placeholders (never empty spaces)

### Dependencies

| Tool | Check | Fallback |
|------|-------|----------|
| Browser MCP | `mcp__MCP_DOCKER__*` or `mcp__playwright__*` | WebFetch |
| Semrush MCP | `mcp__semrush__*` | Manual keywords |
| shadcn MCP | `mcp__shadcn__*` | CLI install |
| shadcnblocks | Registry in components.json | Build from scratch with shadcn |
| claude-seo | `/seo` skill loaded | Skip validation |
| frontend-design | `/frontend-design` loaded | Auto-install via npx |

### Tech Stack
- **Next.js** App Router (`npx shadcn@latest init --preset b0 --template next`)
- **shadcn/ui** base components + **shadcnblocks** pre-built blocks
- **frontend-design** for distinctive, production-grade refinement
- **globals.css** — single source of truth for ALL styles
- See `./references/ui-rules.md` for strict CSS/code rules
