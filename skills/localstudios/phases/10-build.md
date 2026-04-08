# Phase 10 — Build Homepage

## Reference
Load `./references/code-standards.md` for Next.js + shadcn/ui standards.

## Dependency Check
Are `mcp__stitch__*` tools available?

### Path A: Stitch Available

1. Create project in Stitch with company name
2. Generate one screen (homepage) with content, design, and structure
3. Apply design system

### Path B: No Stitch → Generate Next.js Project

**Step 1 — Initialize**
```bash
npx shadcn@latest init --preset b0 --template next
```

**Step 2 — Install shadcn Components**
```bash
npx shadcn@latest add button card accordion input textarea
```

**Step 3 — Create globals.css**
Write all design tokens from Phase 9 into `app/globals.css`.
ALL styling goes here. No inline styles. No separate CSS files.

**Step 4 — Create Site Config**
`lib/site-config.ts` — centralized NAP, company info from Project Brief.

**Step 5 — Create Layout**
```
components/layout/header.tsx    — Navigation, logo
components/layout/footer.tsx    — NAP, links, hours, copyright
```

**Step 6 — Create Homepage Sections**
One component file per section in `components/sections/`:
```
hero.tsx            — H1 + CTA + hero image
trust-bar.tsx       — USP numbers
services-grid.tsx   — Service cards
about-teaser.tsx    — Brief about + team photo
testimonials.tsx    — Reviews/quotes
local-trust.tsx     — Map reference, local signals
cta-section.tsx     — Final CTA
```

**Step 7 — Create Homepage**
```
app/layout.tsx      — Root layout with Header, Footer, schema
app/page.tsx        — Homepage assembling all sections
```

Export `metadata` with title, description, openGraph.

**Step 8 — Add Schema**
```
components/seo/schema-script.tsx  — JSON-LD injection in head
```

### Quality Gates
- [ ] `npm run build` passes
- [ ] No inline `style={}` anywhere
- [ ] All styles in globals.css
- [ ] NAP from siteConfig only
- [ ] Metadata export on page
- [ ] Schema JSON-LD in layout
