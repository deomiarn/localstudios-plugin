---
name: quality-checker
description: >
  Final QA on the built homepage. Checks SEO, schema, content,
  UI rules, design.md compliance, and custom component quality.
---

# Quality Checker Agent

## Task
Verify the built homepage against all quality criteria. Report pass/fail with specific fixes.

## MANDATORY: Use /seo if available

If `/seo` skill is loaded:
```
/seo page [homepage path or URL]
/seo schema [homepage path or URL]
```
**This validates the NEW homepage — not the old website.**

---

## Check Categories

### SEO On-Page
- [ ] H1 present, contains primary keyword + city
- [ ] H1 ist unique (only one on the page)
- [ ] Meta Title ≤ 60 chars with primary KW
- [ ] Meta Description ≤ 155 chars with CTA + location
- [ ] OG Title + OG Description set
- [ ] Primary KW + geo-term im first paragraph
- [ ] Geo-term in at least one H2
- [ ] Keyword density 1-2% (calculate)
- [ ] No keyword stuffing

### Schema
- [ ] LocalBusiness complete with sameAs
- [ ] @type is industry-specific
- [ ] Valid JSON (parse it)
- [ ] No empty required fields
- [ ] Opening hours present

### Content Quality
- [ ] All 10 homepage sections present in correct order (per `references/page-sections.md`)
- [ ] **Total word count 1500-2000** (+ optional FAQ mit 6 Fragen: +200-300)
- [ ] E-E-A-T signals present (experience, credentials, local refs)
- [ ] No filler text ("Welcome to our website", "We are a leading…")
- [ ] No placeholder text remaining
- [ ] **Keine `[Image …]` / `[Foto]` / `[IMG]` Text-Platzhalter im gerenderten DOM**
- [ ] Tone consistent across all sections
- [ ] FAQ/testimonials feel authentic, not generic

### Images
- [ ] Jede Section hat ein Bild oder einen `<ImagePlaceholder>`
- [ ] **Hero hat sichtbares Bild above-the-fold** (scraped next/image mit `priority` ODER ImagePlaceholder mit mindestens `aspect-[4/5]` / `aspect-[3/4]`)
- [ ] Min. 5 Bilder auf der Homepage
- [ ] All Images WebP (oder via next/image served as WebP)
- [ ] All Images with descriptive alt text (keyword + location)
- [ ] All Images with title attribute
- [ ] Above-fold Images with `priority`
- [ ] `width` und `height` gesetzt (CLS prevention)
- [ ] NO empty spaces — Placeholders sind gestylt (Gradient)

### UI Rules Compliance
- [ ] NO hardcoded colors (hex/rgb/hsl) in Components
- [ ] ALL colors via CSS variables (--primary, --accent etc.) aus design.md
- [ ] NO `className="bg-blue-600"` oder ähnliche named Tailwind Colors
- [ ] NO inline `style={}` props
- [ ] NO separate .module.css files
- [ ] Fonts nur via `font-heading` / `font-body` Utilities
- [ ] Hover states konsistent
- [ ] Border radius via `--radius` (Tailwind `rounded-lg` / `rounded-[var(--radius)]`)
- [ ] `globals.css` matcht `design.md` Tokens 1:1

### design.md Compliance (READ-ONLY)
- [ ] `design.md` existiert im Projekt-Root
- [ ] **`design.md` unverändert gegenüber dem Original-Import** (einzige Ausnahme: Farbtoken-Ersetzung bei explizitem Farbwechsel aus Phase 2)
- [ ] `globals.css` spiegelt alle Tokens aus `design.md`
- [ ] Anti-Muster aus `design.md` werden nicht verletzt

### Custom Components
- [ ] Pro Section ein eigenes File in `components/sections/` (flach, KEIN variant-N)
- [ ] Semantisches HTML (section/article/header/footer + h1-h3 + p + ul)
- [ ] Keine fremden Block-Libraries importiert (kein shadcn/ui, kein shadcnblocks)
- [ ] `<Button variant="…" />` statt Inline-Tailwind-Button-Classes
- [ ] Keine Pill-Badges für Orte/Labels

### Design Quality
- [ ] Components sehen nicht generisch aus
- [ ] Typography-Hierarchie absichtlich (nicht alles gleich gross)
- [ ] Whitespace + Spacing fühlen sich deliberate an
- [ ] Visual rhythm zwischen Sections konsistent
- [ ] CTAs stehen visuell heraus (accent color, size)
- [ ] Homepage matcht das Gefühl aus `design.md` (nicht generisch)

### Conversion
- [ ] CTA above fold (Hero)
- [ ] CTA wiederholt nach Social Proof
- [ ] Telefonnummer clickable (tel: Link)
- [ ] NAP sichtbar im Footer
- [ ] NAP matcht Schema exakt

### Technical
- [ ] `npm run build` passes
- [ ] Keine TypeScript errors
- [ ] `lang="de"` (oder correct language) auf html-Tag
- [ ] Metadata export in layout.tsx / page.tsx

## Scoring
Count passed items: `[passed] / [total] — [percentage]%`

- ≥ 95%: EXCELLENT — ready to present to client
- 90-94%: GOOD — minor tweaks
- 80-89%: NEEDS WORK — fix before presenting
- < 80%: NOT READY — significant issues

**Fix all Critical issues before Phase 11 (Report).**

## Output

```
=== QA REPORT ===

SCORE: [x]% ([passed]/[total])

PASSED: [list key wins]

FAILED:
Critical:
- [issue] → [specific fix]

Major:
- [issue] → [specific fix]

Minor:
- [issue] → [suggestion]

=== END QA ===
```
