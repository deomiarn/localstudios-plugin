# Phase 9 — Build Homepage

## Reference
Load `./references/code-standards.md` for Next.js + shadcn/ui standards.

## CRITICAL: Apply the Design System from Phase 8

**Before writing ANY code, read the Phase 8 output and extract these values:**
- Primary color hex
- Secondary color hex
- Accent/CTA color hex
- Background color hex
- Text color hex
- Heading font name
- Body font name
- Recommended style name

**Every component, every color, every font in the code MUST match Phase 8.**

**DO NOT:**
- Use colors that weren't in the Phase 8 output
- Use fonts that weren't in the Phase 8 output
- Fall back to generic blue/white because "it looks fine"
- Copy styles from the old scraped website

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

### Step 3 — globals.css (MUST match Phase 8)

Write the EXACT colors and fonts from Phase 8 into `app/globals.css`:

```css
:root {
  --primary: [EXACT hex from Phase 8];
  --secondary: [EXACT hex from Phase 8];
  --accent: [EXACT hex from Phase 8];
  --background: [EXACT hex from Phase 8];
  --foreground: [EXACT hex from Phase 8];
  --font-heading: '[EXACT font from Phase 8]', sans-serif;
  --font-body: '[EXACT font from Phase 8]', sans-serif;
}
```

**Verify: Do the CSS variables match Phase 8? If not, fix before proceeding.**

### Step 4 — Site Config
`lib/site-config.ts` — NAP from Project Brief. Single source of truth.

### Step 5 — MANDATORY: Use 21st.dev Inspiration Search for Components

Check if `mcp__magic__*` tools are available.

**If available — USE the free Inspiration Search for every section:**

For each of the 8 homepage sections, search for inspiration FIRST, then build:

**5a — Search for inspiration (FREE):**
```
mcp__magic__21st_magic_component_inspiration
```

Search queries per section:
1. "hero section medical" or "hero section [industry]"
2. "trust bar stats badges"
3. "services grid cards"
4. "about teaser team"
5. "testimonials reviews"
6. "local area map"
7. "cta section call to action"

**5b — Study the inspiration result, then build the component yourself** using:
- The layout and structure from the inspiration
- shadcn/ui primitives (Button, Card, etc.)
- Phase 8 design system (colors, fonts)
- Content from Phase 6

**ONLY use `21st_magic_component_inspiration` (free). Do NOT use `21st_magic_component_builder` (paid).**

**DO NOT skip the inspiration search and build from scratch.** The inspiration gives you proven layouts and patterns that look better than what you'd invent.

**If 21st.dev NOT available:**
Build components with shadcn primitives directly.

### Step 6 — Build Section Components (MUST use /frontend-design)

**If `/frontend-design` skill is loaded, invoke it** for building the components.
It ensures distinctive, production-grade code — not generic AI-looking output.

One file per section in `components/sections/`:
```
hero.tsx            — based on inspiration + Phase 8 design + Phase 6 content
trust-bar.tsx
services-grid.tsx
about-teaser.tsx
testimonials.tsx
local-trust.tsx
cta-section.tsx
```

**After building each component:**
- Verify colors use CSS variables from globals.css, not hardcoded
- Verify fonts match Phase 8
- All styling through globals.css or Tailwind classes

### Step 7 — Layout Components
```
components/layout/header.tsx    — Navigation, logo (Phase 8 colors)
components/layout/footer.tsx    — NAP, links, hours (Phase 8 colors)
```

### Step 8 — Assemble Homepage
```
app/layout.tsx      — Root layout with Header, Footer, SchemaScript
app/page.tsx        — Homepage composing all sections + metadata export
```

### Step 9 — Schema
```
components/seo/schema-script.tsx  — JSON-LD injection
```

### Step 10 — Final Verification Before QA

- [ ] globals.css colors match Phase 8 output exactly
- [ ] globals.css fonts match Phase 8 output exactly
- [ ] No hardcoded colors in components
- [ ] No inline `style={}` anywhere
- [ ] 21st.dev inspiration was used for component layouts
- [ ] `npm run build` passes
- [ ] Do NOT start dev server
- [ ] Do NOT take screenshots
