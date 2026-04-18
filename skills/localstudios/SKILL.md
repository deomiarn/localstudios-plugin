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
  version: "2.1.0"
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

## Core Philosophy (v2.1 — read BEFORE writing anything)

The plugin builds websites **from scratch** — no block libraries, no generic design skills, no variants.
Every design decision flows from ONE file: **`design.md`** (in the project root, **READ-ONLY**).

```
design.md            → Single source of truth for colors, fonts, buttons, spacing, atmosphere — READ-ONLY
layout-plan.md       → One layout strategy for ONE homepage (Phase 8 output)
globals.css          → CSS vars from design.md + Tailwind v4 @theme inline mapping
components/…         → Flat custom components, semantic HTML, clean code
Playwright MCP       → Visual validation at the end — screenshot vs. design.md, iterate until match (fix code, never design.md)
```

**Pflicht-Input (Phase 2):** Der User muss ENTWEDER einen `getdesign` CLI-Command liefern
(z.B. `npx getdesign@latest add nike`) ODER den Pfad zu einer existierenden `design.md`.
Ohne eines von beiden wird nicht weitergemacht.

**design.md ist READ-ONLY** — ab Phase 8 wird die Datei nicht mehr angefasst. Einzige Ausnahme: ein explizit vom User in Phase 2 gewünschter Farbwechsel (z.B. „primary von gelb auf blau") — dann NUR die Farb-Tokens ersetzen. Gefühl, Typografie, Atmosphäre, Anti-Muster, Wording bleiben 1:1.

---

## Generate Workflow

Creates a **single homepage** to pitch the client (one design, no variants).

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
| 9. Build (custom components, single homepage) | `./phases/09-build.md` | | — (needs 6+7+8) |
| 10. QA + Playwright Validation + SEO Audit | `./phases/10-quality.md` | | — |
| 11. Report | `./phases/11-report.md` | | — |

### Parallelization
- **Phases 6+7+8 parallel**: Content, schema, and design system simultaneously
- **Phase 9 waits for 6+7+8**: Build needs all three

### MANDATORY Tool Usage

| Tool | Phase | Role |
|------|-------|------|
| `npx getdesign@latest add <brand>` | 8 | Design source — erzeugt `design.md` (einmalig, danach READ-ONLY) |
| Playwright MCP | 10 | Visual validation — Screenshots der Homepage, Abgleich mit `design.md`, Fixes IMMER im Code |
| `/seo page` | 10 | Audit: validates NEW homepage — must FIX all issues |
| `/seo schema` | 10 | Audit: validates schema |
| `/seo content` | 10 | Audit: validates content quality |
| Semrush MCP | 3 | Keyword research |

**Design-Prozess:**
```
Phase 2 Interview → getdesign Command ODER design.md Pfad (+ optional: Farbwechsel-Wunsch)
  → Phase 8 → design.md im Projekt-Root (READ-ONLY ab hier — nie wieder anfassen)
             + layout-plan.md (EIN Layout-Plan für EINE Homepage, alle 10 Sections)
    → Phase 9 → globals.css aus design.md (CSS-Vars + Tailwind v4 @theme inline)
               + Custom Components flach in components/sections/
               + Hero MUSS above-the-fold Bild haben
      → Phase 10 → Playwright Screenshots vs. design.md
                  + /seo audits
                  → Fixes im Code bis Match (NIE design.md anpassen)
```

### Rules
- WAIT = do not proceed until user responds
- ONE homepage only — keine Varianten, keine `/variant-2`, kein Switcher
- **`design.md` ist READ-ONLY** ab Phase 8 (einzige Ausnahme: Farbtoken-Ersetzung bei explizitem Farbwechsel-Wunsch in Phase 2)
- Content: **1500-2000 Wörter** total (+ optional FAQ mit 6 Fragen)
- **Hero MUSS ein Bild above-the-fold haben** (Split oder Full-Bleed) — text-only Hero ist verboten
- **Keine `[Image …]`-Platzhalter-Strings im JSX** — Image-Metadaten gehören in `docs/pages/home.md`
- **Jede Section bekommt mindestens ein Bild** — Placeholder-Gradient wenn keins verfügbar
- Minimum 5 Bilder auf der Homepage
- Images: if scraping fails, use stylish placeholders (never empty spaces)
- No screenshots by Claude during build — user checks localhost
- `npm run build` muss passen

### Dependencies

| Tool | Check | Fallback |
|------|-------|----------|
| Playwright MCP | `mcp__playwright__*` | WebFetch (eingeschränkt — Playwright ist für Phase 10 de facto Pflicht) |
| Semrush MCP | `mcp__semrush__*` | Manual keywords |
| `npx getdesign@latest` | CLI via `npx` verfügbar | User liefert fertige `design.md` im Projekt |
| claude-seo | `/seo` skill loaded | Skip audit (nicht empfohlen) |

### Tech Stack
- **Next.js** App Router (`npx create-next-app@latest . --typescript --tailwind --app`)
- **Tailwind CSS** v4 (via Next.js Preset)
- **Custom Components** — eigene Files flach in `components/sections/`, semantisches HTML, keine fremden Block-Libraries, keine Varianten
- **globals.css** — nur CSS-Vars aus `design.md` + `@theme inline` Mapping + minimale Base-Styles
- **design.md** — READ-ONLY Single Source of Truth: Farben (HSL), Fonts, Typografie, Button-Stil, Section-Spacing, Shadows, Radius, Atmosphäre, Anti-Muster
- **Einzelne Homepage-Route**: `/`

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
- **Style-Wechsel optional**: „Keep style" → design.md bleibt unverändert; „Adapt/New" → neuer getdesign Command ODER neue design.md — danach ebenfalls READ-ONLY. Bei „nur Farbwechsel" nur Farb-Tokens ersetzen.
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
