# Phase 10 — Build Homepage

## Reference
Load `./references/code-standards.md` for Next.js + shadcn/ui standards.

## MANDATORY: Use Stitch if available

Check if `mcp__stitch__*` tools are available.

### If Stitch available — USE IT:

1. `mcp__stitch__create_project` with company name
2. `mcp__stitch__generate_screen_from_text` — pass complete content from Phase 7, design from Phase 9
3. Apply the design system from ui-ux-pro-max

**DO NOT build manually if Stitch is available.**

### If Stitch NOT available — Build Next.js project:

**Step 1 — Init**
```bash
npx shadcn@latest init --preset b0 --template next
```

**Step 2 — Install Components**
Use shadcn MCP if available, otherwise:
```bash
npx shadcn@latest add button card accordion
```

**Step 3 — globals.css**
Write ALL design tokens from Phase 9 (ui-ux-pro-max output) into `app/globals.css`.
No inline styles. No separate CSS.

**Step 4 — Site Config**
`lib/site-config.ts` — NAP from Project Brief. Single source.

**Step 5 — Layout**
```
components/layout/header.tsx
components/layout/footer.tsx
```

**Step 6 — Sections** (one file each in `components/sections/`)
```
hero.tsx, trust-bar.tsx, services-grid.tsx, about-teaser.tsx,
testimonials.tsx, local-trust.tsx, cta-section.tsx
```

**Step 7 — Homepage**
```
app/layout.tsx    — Root layout with Header, Footer, SchemaScript
app/page.tsx      — Homepage composing all sections + metadata export
```

**Step 8 — Schema**
```
components/seo/schema-script.tsx
```

### After Build
- Run `npm run build` to verify
- Do NOT start dev server
- Do NOT take screenshots
- Proceed to Phase 11 (QA)
