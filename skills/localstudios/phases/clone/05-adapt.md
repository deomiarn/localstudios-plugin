# Clone Phase 5 — Adapt Project

## Task
Copy the source project and adapt EVERYTHING for the new client.

---

### Step 1 — Copy Source Project
```bash
cp -R [source-path]/ .
rm -rf .git node_modules
```

### Step 2 — Update design-system.md

If user wants style changes:
- Update colors (convert to HSL for shadcn)
- Update fonts if requested
- Update atmosphere if industry changes
- Consider inspo images from interview
- Update anti-patterns for new industry

If user wants to keep style:
- Keep design-system.md as-is
- Only update company name references

### Step 3 — Update globals.css

Apply design-system.md changes:
- Replace color variables with new values
- Replace font imports if changed
- Keep spacing, shadows, section styles unless requested otherwise

### Step 4 — Update site-config.ts

Replace ALL business data:
- Company name
- Full NAP (address, phone, email)
- Opening hours
- Social links
- Google Maps URL
- Description with new primary keyword

### Step 5 — Update Content in ALL Components

For EACH block/section component file:
1. READ the file
2. REPLACE all text content with new content (write fresh for new client)
3. REPLACE service names, descriptions, testimonials
4. REPLACE team member info
5. KEEP the layout structure, responsive design, component composition
6. CHECK buttons: readable on their background? (dark bg → light button etc.)
7. CHECK centering: `mx-auto max-w-7xl` wrapper present?

**Write 1500-2500 words total — same quality as generate.**
Follow all content rules from `references/content-guidelines.md`.

### Step 6 — Update Schema

Edit `lib/schema.ts`:
- New company name, NAP, opening hours
- New industry @type
- New sameAs URLs
- New description with keywords

### Step 7 — Update Metadata

Edit `app/layout.tsx`:
- New meta title (Primary KW — Company — City)
- New meta description
- New OG tags
- Correct `lang` attribute

### Step 8 — Update Images

- Replace image references with new client images (scraped or placeholder)
- Styled placeholders where images missing
- Minimum 5 images

### Step 9 — Update docs/

- Create new `docs/BUSINESS.md`
- Create new `docs/SEO-STRATEGY.md` with new keywords
- Create new `docs/pages/home.md`

### Step 10 — Reinstall Dependencies
```bash
npm install
```

### Step 11 — /frontend-design Review (if style changed)

If the industry or style changed significantly:
- Invoke `/frontend-design` to refine the adapted components
- Ensure the new style feels intentional, not just recolored

### Step 12 — Build Check
```bash
npm run build
```
Must pass before QA.

### Checks After Adaptation
- [ ] ALL text is new client content (no old client text remaining)
- [ ] site-config.ts has new NAP
- [ ] globals.css has new colors/fonts (if changed)
- [ ] Schema has new business data
- [ ] Metadata has new title/description
- [ ] Buttons readable on all backgrounds
- [ ] Sections centered
- [ ] No old client name anywhere in the code
- [ ] `npm run build` passes
