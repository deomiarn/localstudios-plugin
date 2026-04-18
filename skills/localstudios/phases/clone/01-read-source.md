# Clone Phase 1 — Read Source Project

## Task
Understand the source project completely before making any changes.

## Read these files from the source project:

1. **`design.md`** — colors, fonts, buttons, spacing, atmosphere, anti-patterns (**READ-ONLY Source of Truth**)
2. **`layout-plan.md`** — Layout-Plan der Homepage (falls vorhanden)
3. **`docs/BUSINESS.md`** — old client's business data (inklusive Design Source Eintrag)
4. **`docs/SEO-STRATEGY.md`** — old keyword strategy
5. **`docs/pages/home.md`** — old homepage documentation + Image-Source-Tracking
6. **`app/globals.css`** — current design tokens (CSS-Vars aus design.md + Tailwind v4 @theme)
7. **`lib/site-config.ts`** — old NAP data
8. **`components/seo/schema-script.tsx`** (oder `lib/schema.ts`) — old schema
9. **`app/page.tsx`** — Homepage-Assembly
10. **All Section-Component files** flach in `components/sections/`

## Output

```
=== SOURCE PROJECT ANALYSIS ===

STRUCTURE: [Liste der Sections in Reihenfolge]
DESIGN: [Style, Farben, Fonts, Atmosphäre aus design.md]
CONTENT: [word count, tone, sections]
COMPONENT FILES: [Liste der Section-Components]

WHAT TO KEEP:
- Layout-Strukturen der Custom-Components
- Responsive Design
- Accessibility-Patterns

WHAT TO REPLACE:
- Content (all text)
- NAP data (site-config.ts)
- design.md + globals.css (NUR wenn Style-Change im Clone Brief — design.md bleibt READ-ONLY ab Übernahme)
- Schema (neue business data + @type)
- Images
- Metadata (title, description, OG)

=== END ===
```
