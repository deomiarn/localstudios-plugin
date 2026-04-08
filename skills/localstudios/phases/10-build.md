# Phase 10 — Website Build

## Reference
Load `./references/code-standards.md` for Next.js + shadcn/ui standards.

## Dependency Check
Are `mcp__stitch__*` tools available?

### Path A: Stitch Available

1. Create project: `mcp__stitch__create_project` with company name
2. Per page, generate screen with content (Phase 7), design (Phase 9), structure (Phase 6)
3. Apply consistent design across all pages
4. Optionally generate variants

### Path B: No Stitch → Generate Next.js Project

**Step 1 — Initialize**
```bash
npx shadcn@latest init --preset b0 --template next
```

**Step 2 — Install shadcn Components**
Use shadcn MCP if available (`mcp__shadcn__*`), otherwise CLI:
```bash
npx shadcn@latest add button card accordion input textarea navigation-menu
```

**Step 3 — Create globals.css**
Write all design tokens from Phase 9 into `app/globals.css`:
- Brand colors as CSS custom properties
- Typography settings
- Section spacing
- Component overrides
- Responsive breakpoints

**ALL styling goes here. No inline styles. No separate CSS files.**

**Step 4 — Create Site Config**
`lib/site-config.ts` — centralized NAP, company info, social links from Project Brief.
Single source of truth — components read from here, never hardcode NAP.

**Step 5 — Create Layout Components**
```
components/layout/header.tsx    — Navigation, logo, mobile menu
components/layout/footer.tsx    — NAP from siteConfig, links, copyright
components/layout/mobile-nav.tsx — Mobile drawer navigation
```

**Step 6 — Create Section Components**
One file per section, all in `components/sections/`:
```
hero.tsx            — H1, subheadline, primary CTA (uses Button)
trust-bar.tsx       — USP numbers/badges
services-grid.tsx   — Service cards with links (uses Card)
testimonials.tsx    — Reviews/quotes
local-trust.tsx     — Map reference, local signals
cta-section.tsx     — Reusable CTA block (uses Button)
faq-section.tsx     — FAQ accordion (uses Accordion)
contact-form.tsx    — Form with validation (uses Input, Textarea, Button)
```

**Step 7 — Create SEO Components**
```
components/seo/schema-script.tsx  — JSON-LD injection
components/seo/breadcrumbs.tsx    — Breadcrumb navigation
```

**Step 8 — Create Pages**
```
app/layout.tsx           — Root layout with Header, Footer, schema
app/page.tsx             — Home (Hero + Trust + Services + Testimonials + CTA)
app/about/page.tsx       — About (Team + Credentials + Local)
app/services/[service]/page.tsx  — Service (Info + Process + FAQ + CTA)
app/contact/page.tsx     — Contact (NAP + Form + Map + Hours)
```

Each page exports `metadata` with:
- `title` (max 60 chars, primary KW)
- `description` (max 155 chars)
- `openGraph.title` + `openGraph.description`

**Step 9 — Add Schema Files**
```
schema/localbusiness.json
schema/organization.json
schema/[service]-faq.json
schema/breadcrumbs.json
```

**Step 10 — Create Keywords Config**
`lib/keywords.ts` — keyword-to-page mapping from Phase 4 for reference.

### Quality Gates Before Completion
- [ ] `npm run build` passes without errors
- [ ] No inline `style={}` anywhere in codebase
- [ ] All styles in globals.css
- [ ] NAP sourced from siteConfig only
- [ ] Every page has metadata export
- [ ] Schema JSON-LD in layout head
- [ ] All shadcn components imported from `@/components/ui/`
