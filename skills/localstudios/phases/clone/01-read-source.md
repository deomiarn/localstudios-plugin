# Clone Phase 1 — Read Source Project

## Task
Understand the source project completely before making any changes.

## Read these files from the source project:

1. **`design.md`** — colors, fonts, buttons, spacing, atmosphere, anti-patterns (Single Source of Truth)
2. **`variant-blueprints.md`** — Layout-Pläne der Varianten (falls vorhanden)
3. **`docs/BUSINESS.md`** — old client's business data (inklusive Design Source Eintrag)
4. **`docs/SEO-STRATEGY.md`** — old keyword strategy
5. **`docs/pages/home.md`** — old homepage documentation
6. **`app/globals.css`** — current design tokens (1:1 Ableitung aus design.md)
7. **`lib/site-config.ts`** — old NAP data
8. **`components/seo/schema-script.tsx`** (oder `lib/schema.ts`) — old schema
9. **`app/page.tsx`** + `app/variant-2/page.tsx` + `app/variant-3/page.tsx` — Varianten-Assembly
10. **All Section-Component files** in `components/sections/variant-1/`, `variant-2/`, `variant-3/`

## Output

```
=== SOURCE PROJECT ANALYSIS ===

STRUCTURE: [list of sections per variant in order]
VARIANTS: [list of variant-names aus variant-blueprints.md]
DESIGN: [style, colors, fonts, atmosphere aus design.md]
CONTENT: [word count, tone, sections]
COMPONENT FILES: [liste der Section-Components pro Variante]

WHAT TO KEEP:
- Layout-Strukturen der Custom-Components
- Responsive Design
- Accessibility-Patterns

WHAT TO REPLACE:
- Content (all text)
- NAP data (site-config.ts)
- design.md + globals.css (NUR wenn Style-Change im Clone Brief)
- Schema (neue business data + @type)
- Images
- Metadata (title, description, OG)

=== END ===
```
