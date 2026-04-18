# Extend Phase 8 — QA + Playwright + SEO Audit

## MANDATORY: Full site audit — for EVERY page

Three parts pro Page: Checklist → Playwright Visual Validation → SEO Audit.
**Fix every issue immediately.**

---

## Part 1 — Manual Checklist (every page)

**SEO**
- [ ] Unique H1 mit keyword + city
- [ ] Meta Title ≤ 60 chars, unique pro Page
- [ ] Meta Description ≤ 155 chars, unique pro Page
- [ ] OG tags gesetzt
- [ ] BreadcrumbList schema auf jeder Subpage

**Schema**
- [ ] LocalBusiness auf home + contact
- [ ] Service schema auf Service-Pages
- [ ] FAQPage schema auf FAQ + Service-Pages
- [ ] Person schema auf Team-Pages
- [ ] BlogPosting schema auf Blog-Posts
- [ ] BreadcrumbList auf ALL Subpages

**Content**
- [ ] No duplicate content across pages
- [ ] Jede Page hat unique, substanziellen Content
- [ ] Internal links zwischen related pages
- [ ] No placeholder text übrig

**Design (design.md compliance)**
- [ ] Alle Pages nutzen dieselbe `design.md` und `globals.css` wie Homepage
- [ ] **`design.md` unverändert** gegenüber Homepage-Version (READ-ONLY)
- [ ] Keine hardcoded Farben in Components
- [ ] Sections zentriert (`mx-auto max-w-7xl` Container via Tailwind)
- [ ] Buttons readable auf jedem Section-Background (`variant="outline-on-dark"` auf dunklen Sections)
- [ ] Keine Pill-Badges
- [ ] Layout-Rhythmus pro Page-Typ klar erkennbar (About anders als Services Overview, Blog anders als Contact)
- [ ] Bilder oder Placeholders (min. 3 pro Page, page-specific minimums aus image-strategy.md)
- [ ] **Jede Page mit einem Hero hat ein Bild above-the-fold** (Split oder Full-Bleed — keine text-only Heros)
- [ ] Keine `[Image …]` / `[Foto]` Text-Platzhalter im DOM

**Custom Components**
- [ ] Pro Section eigenes File in `components/sections/<page>/`
- [ ] Semantisches HTML
- [ ] Keine fremden Block-Libraries importiert

**Technical**
- [ ] sitemap.xml includes all pages
- [ ] robots.txt correct
- [ ] llms.txt complete
- [ ] 404 page branded
- [ ] `npm run build` passes
- [ ] Keine TypeScript errors
- [ ] Sanity queries working

**PageSpeed**
- [ ] Lighthouse oder PageSpeed Insights
- [ ] Images optimized (next/image)
- [ ] Keine render-blocking resources
- [ ] Fonts preloaded
- [ ] Core Web Vitals acceptable

---

## Part 2 — Playwright Visual Validation (MANDATORY)

Für JEDE gebaute Page (Static + Dynamic).

### Ablauf
1. `npm run dev` (oder `build && start`)
2. Pro Page:
   - Playwright MCP: navigate zur URL
   - Desktop Screenshot (1440×900)
   - Mobile Screenshot (375×812)
   - Console-Messages + Network-Errors abfragen
3. **Abgleich gegen `design.md`**:
   - Farben (Primary/Accent) wie auf Homepage?
   - Typografie-Hierarchie konsistent (H1 dominiert, H2 klar, Body lesbar)?
   - Section-Spacing identisch zur Homepage?
   - Atmosphäre (Whitespace/Shadows/Radius) konsistent?
   - Bilder: min. page-type-spezifisches Minimum erreicht?
   - Buttons auf allen Section-Backgrounds lesbar?
   - Pill-Badges: keine sichtbar?
   - Page-Typ-Rhythmus erkennbar (About ≠ Services Overview ≠ Blog)?
4. Abweichungen SOFORT im Code fixen → re-screenshot → bis Match

### Dynamic Pages
Bei dynamischen Routes (`[slug]`): mindestens 2 echte Slugs aus Sanity testen, nicht nur generateStaticParams-Listen.

---

## Part 3 — /seo Audit (if available)

Run on EVERY page:
```
/seo page [url/path]
/seo schema [url/path]
/seo content [url/path]
/seo technical [url/path]
```

**FIX all issues immediately. Re-run after fixes.**

---

## Scoring
- Per page: pass/fail per category
- Overall: `[passed] / [total] — [percentage]%`
- ≥ 90%: Ready for release
- < 90%: Fix before release
