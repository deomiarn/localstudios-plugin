# Phase 10 — Build Homepage

## Reference
Load `./references/code-standards.md` for Next.js + shadcn/ui standards.

## CRITICAL: Apply the Design System from Phase 9

**Before writing ANY code, read the Phase 9 output and extract these values:**
- Primary color hex
- Secondary color hex
- Accent/CTA color hex
- Background color hex
- Text color hex
- Heading font name
- Body font name
- Recommended style name

**Every component, every color, every font in the code MUST match Phase 9.**
If Phase 9 said "Amber CTA #FBBF24" then every CTA button uses #FBBF24. Not blue. Not your default. The exact color.

**DO NOT:**
- Use colors that weren't in the Phase 9 output
- Use fonts that weren't in the Phase 9 output
- Fall back to generic blue/white because "it looks fine"
- Copy styles from the old scraped website
- Ignore the recommended style and do your own thing

---

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

### Step 3 — globals.css (MUST match Phase 9)

Write the EXACT colors and fonts from Phase 9 into `app/globals.css`:

```css
:root {
  --primary: [EXACT hex from Phase 9];
  --secondary: [EXACT hex from Phase 9];
  --accent: [EXACT hex from Phase 9];
  --background: [EXACT hex from Phase 9];
  --foreground: [EXACT hex from Phase 9];
  --font-heading: '[EXACT font from Phase 9]', sans-serif;
  --font-body: '[EXACT font from Phase 9]', sans-serif;
}
```

**Verify: Do the CSS variables match Phase 9? If not, fix before proceeding.**

### Step 4 — Site Config
`lib/site-config.ts` — NAP from Project Brief. Single source of truth.

### Step 5 — MANDATORY: Use 21st.dev Magic MCP for Components

Check if `mcp__magic__*` tools are available.

**If available — USE IT for every section component:**

For each of the 8 homepage sections:

**5a — Find component inspiration:**
```
mcp__magic__21st_magic_component_inspiration
```
Search for: "hero section", "trust bar", "services grid", "testimonials", "CTA section", etc.

**5b — Build each component:**
```
mcp__magic__21st_magic_component_builder
```
Pass to builder:
- Content from Phase 7
- **EXACT colors and fonts from Phase 9** (not defaults)
- Industry context

**Components to build via 21st.dev:**
1. `hero.tsx` — Hero with H1, CTA, hero image
2. `trust-bar.tsx` — Numbers/badges strip
3. `services-grid.tsx` — Service cards
4. `about-teaser.tsx` — Brief about with photo
5. `testimonials.tsx` — Reviews with stars
6. `local-trust.tsx` — Map + local signals
7. `cta-section.tsx` — Final CTA block

**After receiving each component from 21st.dev:**
- Check if colors match globals.css variables → if not, replace hardcoded colors with CSS variables
- Check if fonts match → if not, replace
- All styling through globals.css or Tailwind classes using the CSS variables

**If 21st.dev NOT available:**
Build manually with shadcn primitives. Still use Phase 9 colors/fonts.

### Step 6 — Layout
```
components/layout/header.tsx    — Navigation, logo (uses Phase 9 colors)
components/layout/footer.tsx    — NAP, links, hours (uses Phase 9 colors)
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

### Step 9 — Final Verification Before QA

**Checklist before proceeding to Phase 11:**
- [ ] globals.css colors match Phase 9 output exactly
- [ ] globals.css fonts match Phase 9 output exactly
- [ ] No hardcoded colors in components (all use CSS variables or Tailwind)
- [ ] No inline `style={}` anywhere
- [ ] CTA buttons use the accent/CTA color from Phase 9
- [ ] `npm run build` passes
