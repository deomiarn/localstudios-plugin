# Phase 10 — Build Homepage

## Reference
Load `./references/code-standards.md` for Next.js + shadcn/ui standards.

## Build Process

### Step 1 — Init Next.js + shadcn
```bash
npx shadcn@latest init --preset b0 --template next
```

### Step 2 — Install Base Components
Use shadcn MCP if available, otherwise CLI:
```bash
npx shadcn@latest add button card accordion
```

### Step 3 — globals.css
Write ALL design tokens from Phase 9 (ui-ux-pro-max output) into `app/globals.css`.
No inline styles. No separate CSS files.

### Step 4 — Site Config
`lib/site-config.ts` — NAP from Project Brief. Single source of truth.

### Step 5 — MANDATORY: Use 21st.dev Magic MCP for Components

Check if `mcp__magic__*` tools are available.

**If available — USE IT for every section component:**

For each of the 8 homepage sections, use 21st.dev to find and build polished components:

**Step 5a — Find inspiration for each section:**
```
mcp__magic__21st_magic_component_inspiration
```
Search for: "hero section", "trust bar", "services grid", "testimonials", "CTA section", etc.
Pick the best matching component for each section.

**Step 5b — Build/customize each component:**
```
mcp__magic__21st_magic_component_builder
```
For each section, build the component with:
- Content from Phase 7
- Design system from Phase 9 (colors, fonts, style)
- Industry context (e.g. "dental practice", "local business")

**Build these section components via 21st.dev:**
1. `hero.tsx` — Hero with H1, CTA, hero image
2. `trust-bar.tsx` — Numbers/badges strip
3. `services-grid.tsx` — Service cards
4. `about-teaser.tsx` — Brief about with photo
5. `testimonials.tsx` — Reviews with stars
6. `local-trust.tsx` — Map + local signals
7. `cta-section.tsx` — Final CTA block

**DO NOT build these from scratch if 21st.dev is available.**
21st.dev components look significantly better than hand-coded ones.

**If NOT available:**
Build components manually with shadcn primitives (Button, Card, etc.).

### Step 6 — Layout Components
```
components/layout/header.tsx    — Navigation, logo
components/layout/footer.tsx    — NAP, links, hours, copyright
```

### Step 7 — Assemble Homepage
```
app/layout.tsx      — Root layout with Header, Footer, SchemaScript
app/page.tsx        — Homepage composing all sections + metadata export
```

### Step 8 — Schema
```
components/seo/schema-script.tsx  — JSON-LD injection
```

### After Build
- Run `npm run build` to verify no errors
- Do NOT start dev server
- Do NOT take screenshots
- Proceed to Phase 11
