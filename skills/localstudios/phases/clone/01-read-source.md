# Clone Phase 1 — Read Source Project

## Task
Understand the source project completely before making any changes.

## Read these files from the source project:

1. **`design-system.md`** — colors, fonts, atmosphere, anti-patterns
2. **`docs/BUSINESS.md`** — old client's business data
3. **`docs/SEO-STRATEGY.md`** — old keyword strategy
4. **`docs/pages/home.md`** — old homepage documentation
5. **`app/globals.css`** — current design tokens
6. **`lib/site-config.ts`** — old NAP data
7. **`lib/schema.ts`** — old schema
8. **`app/page.tsx`** — homepage structure (which components in which order)
9. **All component files** in `components/sections/` or `components/blocks/`

## Output

```
=== SOURCE PROJECT ANALYSIS ===

STRUCTURE: [list of sections/components in order]
DESIGN: [style, colors, fonts from design-system.md]
CONTENT: [word count, tone, sections]
BLOCKS USED: [list of shadcnblocks used]

WHAT TO KEEP:
- [layout structure]
- [component files]
- [responsive design]

WHAT TO REPLACE:
- Content (all text)
- NAP data (site-config.ts)
- Colors/fonts (if industry changes)
- Schema (new business data)
- Images

=== END ===
```
