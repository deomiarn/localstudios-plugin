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
  version: "2.0.0"
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

## Core Philosophy (v2.0 — read BEFORE writing anything)

The plugin builds websites **from scratch** — no block libraries, no generic design skills.
Every design decision flows from ONE file: **`design.md`** (in the project root).

```
design.md            → Single source of truth for colors, fonts, buttons, spacing, atmosphere
globals.css          → 1:1 derivation from design.md (CSS vars, utility classes, typography)
components/…         → Custom components in their own files, semantic HTML, clean code
Playwright MCP       → Visual validation at the end — screenshot vs. design.md, iterate until match
```

**Pflicht-Input (Phase 2):** Der User muss ENTWEDER einen `getdesign` CLI-Command liefern
(z.B. `npx getdesign@latest add nike`) ODER den Pfad zu einer existierenden `design.md`.
Ohne eines von beiden wird nicht weitergemacht.

---

## Generate Workflow

Creates a **single homepage** (in N design variants) to pitch the client.

| Phase | File | Pause? | Group |
|-------|------|--------|-------|
| 0. Preflight | `./phases/00-preflight.md` | WAIT | — |
| 1. Scrape | `./phases/01-scrape.md` | | — |
| 2. Interview + Design Source | `./phases/02-interview.md` | WAIT | — |
| 3. Keywords | `./phases/03-keywords.md` | | — |
| 4. Geo-Strategy | `./phases/04-geo-strategy.md` | | after 3 |
| 5. Outline | `./phases/05-outline.md` | WAIT | — |
| 6. Content | `./phases/06-content.md` | | ⬇ parallel |
| 7. Schema | `./phases/07-schema.md` | | ⬇ parallel with 6 |
| 8. Design System (from getdesign/design.md) | `./phases/08-design.md` | | ⬇ parallel with 6+7 |
| 9. Build Variants (custom components) | `./phases/09-build.md` | | — (needs 6+7+8) |
| 10. QA + Playwright Validation + SEO Audit | `./phases/10-quality.md` | | — |
| 11. Report | `./phases/11-report.md` | | — |

### Parallelization
- **Phases 6+7+8 parallel**: Content, schema, and design system simultaneously
- **Phase 9 waits for 6+7+8**: Build needs all three

### MANDATORY Tool Usage

| Tool | Phase | Role |
|------|-------|------|
| `npx getdesign@latest add <brand>` | 8 | Design source — erzeugt / ergänzt `design.md` |
| Playwright MCP | 10 | Visual validation — Screenshots pro Variante, Abgleich mit `design.md`, iterative Fixes |
| `/seo page` | 10 | Audit: validates NEW homepage — must FIX all issues |
| `/seo schema` | 10 | Audit: validates schema |
| `/seo content` | 10 | Audit: validates content quality |
| Semrush MCP | 3 | Keyword research |

**Design-Prozess:**
```
Phase 2 Interview → getdesign Command ODER design.md Pfad
  → Phase 8 → design.md im Projekt-Root (normalized: Farben, Fonts, Buttons, Spacing, Atmosphäre, Anti-Muster)
             + variant-blueprints.md (N verschiedene Layout-Strategien pro Section, KEINE Block-Queries)
    → Phase 9 → globals.css 1:1 aus design.md
               + Custom Components pro Section pro Variante
      → Phase 10 → Playwright Screenshots vs. design.md
                  + /seo audits
                  → Fixes bis Match
```

### Rules
- WAIT = do not proceed until user responds
- ONE page only — the homepage (in N Varianten)
- Default 3 Varianten (User kann Anzahl in Phase 2 ändern)
- Gleiche Palette + Content + Schema + globals.css über alle Varianten
- Verschiedene Layouts + visueller Rhythmus + Komposition pro Variante
- Variant Switcher für Vergleich (wird vor Delivery entfernt)
- **Jede Section bekommt mindestens ein Bild** (Wärme/Leben) — Placeholder-Gradient wenn keins verfügbar
- Minimum 5 Bilder pro Variante (10 Sections → 5-10 Bilder)
- No screenshots by Claude during build — user checks localhost
- No `npm run dev` für Build-Abschluss — `npm run build` muss passen
- Images: if scraping fails, use stylish placeholders (never empty spaces)

### Dependencies

| Tool | Check | Fallback |
|------|-------|----------|
| Playwright MCP | `mcp__playwright__*` | WebFetch (eingeschränkt — Playwright ist für Phase 10 de facto Pflicht) |
| Semrush MCP | `mcp__semrush__*` | Manual keywords |
| `npx getdesign@latest` | CLI via `npx` verfügbar | User liefert fertige `design.md` im Projekt |
| claude-seo | `/seo` skill loaded | Skip audit (nicht empfohlen) |

### Tech Stack
- **Next.js** App Router (`npx create-next-app@latest . --typescript --tailwind --app`)
- **Tailwind CSS** (via Next.js Preset)
- **Custom Components** — eigene Files pro Section, semantisches HTML, keine fremden Block-Libraries
- **globals.css** — single source of truth für alle Styles, direkt aus `design.md` abgeleitet
- **design.md** — Tokens: Farben (HSL), Fonts, Typografie-Scale, Button-Klassen, Section-Spacing, Shadows, Radius, Atmosphäre, Anti-Muster
- **Multi-Variant Routing**: `/` (Variant 1), `/variant-2`, `/variant-3`

---

## Clone Workflow

Reuses an existing project as template. Faster than generate — no design decisions needed unless the client wants style changes.

```
/localstudios clone <source-project-path> <new-url>
```

| Phase | File | Pause? |
|-------|------|--------|
| 0. Preflight | `./phases/clone/00-preflight.md` | WAIT |
| 1. Read Source | `./phases/clone/01-read-source.md` | |
| 2. Scrape New | `./phases/clone/02-scrape.md` | |
| 3. Interview (+ optional getdesign) | `./phases/clone/03-interview.md` | WAIT |
| 4. Keywords | `./phases/clone/04-keywords.md` | |
| 5. Adapt | `./phases/clone/05-adapt.md` | |
| 6. QA + Playwright + SEO | `./phases/clone/06-quality.md` | |
| 7. Report | `./phases/clone/07-report.md` | |

### Key Differences vs Generate
- **Kein Custom-Component-Neubau** — existierende Components aus Source werden wiederverwendet, Content wird neu geschrieben
- **Style-Wechsel optional**: „Keep style" → design.md bleibt; „Adapt/New" → neuer getdesign Command ODER neue design.md erforderlich, dann design.md + globals.css regenerieren
- **Much faster** — skip design decisions, just adapt

---

## Extend Workflow

Extends an existing homepage (from `generate`) into a full website with all pages, Sanity CMS for dynamic content, and technical files.

```
/localstudios extend
```

Run in the existing project directory. Reads docs/, design.md, and homepage.

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
| 8. QA + Playwright + SEO | `./phases/extend/08-quality.md` | |
| 9. Report | `./phases/extend/09-report.md` | |

### Pages Built

**Static:** Services Overview, About, Team Overview, Regions Overview, FAQ, Blog Overview, Contact, Impressum, Datenschutz, 404

**Dynamic (Sanity CMS):** Service Detail `[slug]`, Team Member `[slug]`, Region Detail `[slug]`, Blog Category `[slug]`, Blog Post `[slug]`

**Technical:** sitemap.xml, robots.txt, llms.txt, llms-full.txt

### Style-Konsistenz
- **Homepage-Style wird 1:1 auf alle Unterseiten übertragen** — gleiche `design.md`, gleiche `globals.css`, gleicher Header/Footer
- Kein neuer getdesign-Command in Extend — die Tokens sind gesetzt
- Pro Page-Typ variiert der **Layout-Rhythmus** (nicht die Tokens): z.B. Services Overview anders aufgeteilt als About, damit die Seiten sich unterscheiden

### Sanity Integration
- Globale Daten (NAP, Öffnungszeiten, Social Links) in Sanity — components read from CMS
- Collections: services, team, regions, blogPosts, blogCategories
- Sanity MCP (`mcp__Sanity__*`) for schema + content setup

### MANDATORY /seo + Playwright Usage in QA
Run on EVERY page built:
- Playwright: Screenshot + Console/Network check + design.md Abgleich
- `/seo page` — on-page SEO validation
- `/seo schema` — structured data validation
- `/seo content` — content quality + E-E-A-T
- `/seo technical` — crawlability, performance

**FIX all issues immediately. Re-audit after fixes.**
