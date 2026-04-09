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
  version: "1.3.0"
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

Creates a **single homepage** to pitch the client.

| Phase | File | Pause? | Group |
|-------|------|--------|-------|
| 0. Preflight | `./phases/00-preflight.md` | WAIT | — |
| 1. Scrape | `./phases/01-scrape.md` | | — |
| 2. Interview + Design Brief | `./phases/02-interview.md` | WAIT | — |
| 3. Keywords | `./phases/03-keywords.md` | | — |
| 4. Geo-Strategy | `./phases/04-geo-strategy.md` | | after 3 |
| 5. Outline | `./phases/05-outline.md` | WAIT | — |
| 6. Content | `./phases/06-content.md` | | ⬇ parallel |
| 7. Schema | `./phases/07-schema.md` | | ⬇ parallel with 6 |
| 8. Design System | `./phases/08-design.md` | | ⬇ parallel with 6+7 |
| 9. Build | `./phases/09-build.md` | | — (needs 6+7+8) |
| 10. QA + SEO Audit | `./phases/10-quality.md` | | — |
| 11. Report | `./phases/11-report.md` | | — |

### Parallelization
- **Phases 6+7+8 parallel**: Content, schema, and design simultaneously
- **Phase 9 waits for 6+7+8**: Build needs all three

### MANDATORY Skill & Tool Usage

| Tool | Phase | Role |
|------|-------|------|
| shadcnblocks | 9 | FUNDAMENT: Blocks as structural starting points |
| `/frontend-design` | 9 | FEINTUNING: makes blocks distinctive, non-generic |
| `/seo page` | 10 | AUDIT: validates NEW homepage — must FIX all issues |
| `/seo schema` | 10 | AUDIT: validates schema |
| `/seo content` | 10 | AUDIT: validates content quality |
| Semrush MCP | 3 | Keyword research |
| shadcn MCP | 9 | Install base + blocks |

**Design-Prozess:**
```
Design Brief (Referenz-Screenshots + Adjektive + Farben)
  → design-system.md (Farben, Fonts, Spacing, Anti-Patterns)
    → shadcnblocks (Fundament — Blöcke passend zur Referenz)
      → /frontend-design (Feintuning — unverwechselbar machen)
        → /seo audit (Prüfung + Fixes)
```

### Rules
- WAIT = do not proceed until user responds
- ONE page only — the homepage
- Minimum 5 Bilder auf der Homepage (8 Sections = 5-8 Bilder)
- No screenshots — user checks localhost
- No `npm run dev` — just `npm run build`
- Images: if scraping fails, use stylish placeholders (never empty spaces)

### Dependencies

| Tool | Check | Fallback |
|------|-------|----------|
| Playwright MCP | `mcp__playwright__*` | WebFetch |
| Semrush MCP | `mcp__semrush__*` | Manual keywords |
| shadcn MCP | `mcp__shadcn__*` | CLI install |
| shadcnblocks | Registry in components.json | Build from scratch |
| frontend-design | `/frontend-design` loaded | Auto-install |
| claude-seo | `/seo` skill loaded | Skip audit |

### Tech Stack
- **Next.js** App Router (`npx shadcn@latest init --preset b0 --template next`)
- **shadcn/ui** + **shadcnblocks** (Fundament)
- **frontend-design** (Feintuning)
- **globals.css** — single source of truth for ALL styles
