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
| `clone <source-path> <new-url>` | Clone an existing project for a new client |
| `extend` | Extend homepage into full website with all pages + Sanity CMS |

## Routing

- **`generate`** → Execute generate workflow (second arg = URL)
- **`clone`** → Execute clone workflow (second arg = source path, third = new URL)
- **`extend`** → Extend existing homepage into full website (run in project dir)
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

---

## Clone Workflow

Reuses an existing project as template. Faster than generate — no block-hunting, no structure decisions.

```
/localstudios clone <source-project-path> <new-url>
```

| Phase | File | Pause? |
|-------|------|--------|
| 0. Preflight | `./phases/clone/00-preflight.md` | WAIT |
| 1. Read Source | `./phases/clone/01-read-source.md` | |
| 2. Scrape New | `./phases/clone/02-scrape.md` | |
| 3. Interview | `./phases/clone/03-interview.md` | WAIT |
| 4. Keywords | `./phases/clone/04-keywords.md` | |
| 5. Adapt | `./phases/clone/05-adapt.md` | |
| 6. QA | `./phases/clone/06-quality.md` | |
| 7. Report | `./phases/clone/07-report.md` | |

### Key Differences vs Generate
- **No block search** — existing blocks are reused as-is
- **No new structure** — same sections, new content
- **Design Brief is focused** — "keep same style?" or "change to ___?"
- **Much faster** — skip design decisions, just adapt

---

## Extend Workflow

Extends an existing homepage (from `generate`) into a full website with all pages, Sanity CMS for dynamic content, and technical files.

```
/localstudios extend
```

Run in the existing project directory. Reads docs/, design-system.md, and homepage.

| Phase | File | Pause? |
|-------|------|--------|
| 0. Preflight | `./phases/extend/00-preflight.md` | WAIT |
| 1. Read Project | `./phases/extend/01-read-project.md` | |
| 2. Interview | `./phases/extend/02-interview.md` | WAIT |
| 3. Sanity Setup | `./phases/extend/03-sanity.md` | |
| 4. Static Pages | `./phases/extend/04-static-pages.md` | |
| 5. Dynamic Pages | `./phases/extend/05-dynamic-pages.md` | |
| 6. Technical Files | `./phases/extend/06-technical.md` | |
| 7. Content | `./phases/extend/07-content.md` | |
| 8. QA + SEO Audit | `./phases/extend/08-quality.md` | |
| 9. Report | `./phases/extend/09-report.md` | |

### Pages Built

**Static:** Services Overview, About, Team Overview, Regions Overview, FAQ, Blog Overview, Contact, Impressum, Datenschutz, 404

**Dynamic (Sanity CMS):** Service Detail `[slug]`, Team Member `[slug]`, Region Detail `[slug]`, Blog Category `[slug]`, Blog Post `[slug]`

**Technical:** sitemap.xml, robots.txt, llms.txt, llms-full.txt

### Sanity Integration
- Globale Daten (NAP, Öffnungszeiten, Social Links) in Sanity — components read from CMS
- Collections: services, team, regions, blogPosts, blogCategories
- Sanity MCP (`mcp__Sanity__*`) for schema + content setup

### MANDATORY /seo Usage in QA
Run on EVERY page built:
- `/seo page` — on-page SEO validation
- `/seo schema` — structured data validation
- `/seo content` — content quality + E-E-A-T
- `/seo technical` — crawlability, performance
**FIX all issues immediately. Re-audit after fixes.**
