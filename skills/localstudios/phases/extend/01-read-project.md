# Extend Phase 1 — Read Existing Project

## Task
Understand the existing homepage project before extending it.

## Read
1. `docs/BUSINESS.md` — all business data
2. `docs/SEO-STRATEGY.md` — keyword strategy
3. `design.md` — colors, fonts, buttons, spacing, atmosphere (Single Source of Truth)
4. `variant-blueprints.md` — Layout-Pläne der Varianten (falls vorhanden)
5. `app/globals.css` — current design tokens (1:1 Ableitung aus design.md)
6. `lib/site-config.ts` — NAP data
7. `app/page.tsx` — Homepage-Assembly der gewählten Variante
8. All components in `components/` — Section-Components + Layout + UI Primitives

## Output
```
=== EXISTING PROJECT ===
Company: [name] — [industry] — [city]
Design: [style, colors, fonts aus design.md]
Homepage Sections: [list]
Component Files: [liste Section-Components]
UI Primitives: [Button, ImagePlaceholder]
=== READY TO EXTEND ===
```
