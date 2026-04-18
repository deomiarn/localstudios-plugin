# Phase 10 — Quality Check + Playwright Validation + SEO Audit

## Three parts, in order: QA Checklist → Playwright Visual Validation → SEO Audit.
**Fix every issue immediately before moving to the next part.**

---

## Part 1 — QA Checklist

### SEO On-Page
- [ ] H1 present, contains primary keyword + city
- [ ] Meta Title ≤ 60 chars with primary KW
- [ ] Meta Description ≤ 155 chars with CTA + location
- [ ] OG Title + OG Description set
- [ ] Primary KW + geo-term in first paragraph
- [ ] Geo-term in at least one H2
- [ ] Keyword density 1-2%

### Schema
- [ ] LocalBusiness complete with sameAs
- [ ] @type is industry-specific
- [ ] Valid JSON
- [ ] Opening hours present

### Content
- [ ] All 10 sections present in correct order (per `page-sections.md`)
- [ ] **1500-2000 words total** (FAQ mit 6 Fragen extra erlaubt)
- [ ] E-E-A-T signals present
- [ ] No filler/placeholder text
- [ ] **Keine `[Image …]`-Platzhalter-Strings im gerenderten DOM**
- [ ] Tone consistent

### Images
- [ ] Jede Section hat mindestens ein Bild oder einen ImagePlaceholder
- [ ] **Hero hat sichtbares Bild above-the-fold** (scraped next/image mit `priority` ODER ImagePlaceholder mit mindestens `aspect-[4/5]`)
- [ ] Mindestens 5 Bilder auf der Homepage
- [ ] All images have alt text (+ title where relevant)
- [ ] Above-fold images have `priority`

### design.md compliance (READ-ONLY)
- [ ] **`design.md` unverändert ggü. Phase 8** (einzige Ausnahme: Farbtoken-Ersetzung falls Farbwechsel aus Phase 2)
- [ ] `globals.css` enthält alle Tokens aus `design.md` (Farben, Typografie-Scale, Radius)
- [ ] Keine hardcoded Farben in Components (kein `bg-blue-600`, kein `#1E3A8A`, kein inline `style={}`)
- [ ] Fonts kommen via `next/font` und Utilities `font-heading` / `font-body`
- [ ] Button-Shape/Padding/Radius matchen `design.md`
- [ ] Anti-Muster aus `design.md` eingehalten

### Custom Components
- [ ] Jede Section ist ein eigenes File in `components/sections/` (flach, kein variant-N)
- [ ] Semantisches HTML (section/article/header/footer statt generischer divs)
- [ ] Keine Pill-Badges für Orte/Labels
- [ ] Keine fremden Block-Libraries importiert

### Homepage
- [ ] Homepage rendert sauber auf `/`
- [ ] Alle 10 Sections in richtiger Reihenfolge
- [ ] Keine variant-2 / variant-3 Routen
- [ ] Kein VariantSwitcher

### Conversion
- [ ] CTA above fold
- [ ] CTA repeated after social proof
- [ ] Phone clickable (tel:)
- [ ] NAP in footer matches schema

### Technical
- [ ] `npm run build` passes
- [ ] `lang` attribute korrekt
- [ ] Metadata export in layout.tsx / page.tsx

---

## Part 2 — Playwright Visual Validation (MANDATORY)

**Playwright MCP ist für diese Phase Pflicht.** Claude führt die Screenshots selbst aus und gleicht visuell gegen `design.md` ab.

### Step 1 — Dev/Build starten

```bash
# Dev-Server reicht für Screenshot-Abgleich:
npm run dev
# oder für produktions-ähnlichen Check:
npm run build && npm run start
```

Warten bis Server bereit ist (Port 3000 default).

### Step 2 — Screenshot-Loop für die Homepage

Für die Homepage (`/`):

1. **Desktop Screenshot**
   - Playwright MCP: navigate zu `http://localhost:3000/`
   - Viewport: 1440×900
   - Full-page Screenshot
   - Console-Messages abfragen → Errors protokollieren
   - Network-Requests abfragen → 404/5xx protokollieren

2. **Above-the-fold Screenshot** (zusätzlich ohne Scroll)
   - Viewport: 1440×900 — **nur der sichtbare Bereich**
   - Zweck: Hero-Bild muss darauf sichtbar sein

3. **Mobile Screenshot**
   - Viewport: 375×812
   - Full-page Screenshot
   - Above-the-fold Check auch auf Mobile: Hero-Bild sichtbar?

4. **Abgleich gegen `design.md`** (visuell analysieren):
   - **Farben:** Primary/Accent/Background visuell erkennbar und konsistent mit `design.md`?
   - **Typografie-Hierarchie:** H1 dominiert? H2 klar kleiner aber lesbar? Body-Text angenehm? Scale matcht `design.md`?
   - **Section-Spacing:** konsistent — keine Section die zu eng wirkt?
   - **Atmosphäre:** Whitespace/Shadows/Radius entsprechen dem was `design.md` vorgibt?
   - **Hero:** Bild above-the-fold sichtbar? Proportion (`aspect-[4/5]` / `aspect-[3/4]`) ok? Keine `[Image …]`-Text-Platzhalter im DOM?
   - **Bilder:** Jede Section hat ein Bild? Min. 5 Bilder auf der Homepage sichtbar? Kein leerer Space?
   - **Buttons:** Auf JEDEM Section-Hintergrund lesbar? Keine unsichtbaren oder zu kleinen Buttons?
   - **Pill-Badges:** Keine sichtbar? Orte/Labels clean als Text?
   - **Content-Menge:** Fühlt sich eine Section zu text-lastig an (Ziel 1500-2000 Wörter total)?

5. **Abweichungen fixen — IMMER im Code, NIEMALS in `design.md`**
   - Für jede Abweichung das verantwortliche File identifizieren und sofort anpassen
   - Häufige Fixes:
     - Buttons zu klein → Button-Component variants anpassen (`px-8 py-3` → grösser)
     - Heading zu wenig dominant → Scale-Utilities in der Component erhöhen
     - Section fühlt sich eng → Container-Padding oder Section-Spacing erhöhen
     - Hero hat kein above-the-fold Bild → Hero-Komponente auf Split- oder Full-Bleed-Layout umbauen
     - `[Image …]` Text im DOM → Content von Phase 6 bereinigen, Image-Metadaten gehören in `docs/pages/home.md`
   - **NICHT** hardcoded Farben hinzufügen.
   - **NICHT** `design.md` anpassen — wenn der Code vom Design abweicht, ist der Code falsch, nicht die Vorlage.

6. **Re-Screenshot nach Fix** → bis Match.

### Step 3 — Console & Network Report

Am Ende kurz auflisten:
- Console-Errors (sollte 0 sein)
- Failed Requests (sollte 0 sein)
- Missing Images (404 auf Image-URLs)

Wenn nicht 0 → fixen.

---

## Part 3 — SEO Audit

**If `/seo` skill is available — YOU MUST RUN THESE. No exceptions.**

### Step 1 — Run SEO analysis
```
/seo page [localhost URL oder built homepage]
```

### Step 2 — Run schema validation
```
/seo schema [localhost URL oder built homepage]
```

### Step 3 — Run content check
```
/seo content [localhost URL oder built homepage]
```

### Step 4 — FIX EVERYTHING the audit finds

**Do NOT just report issues. FIX them immediately.**

For each issue:
1. Read the issue
2. Identify which file needs to change
3. Edit the file to fix it
4. Verify the fix

Common fixes:
- Meta title too long → shorten in layout.tsx metadata
- Missing alt text → add to image component
- Schema validation error → fix in `components/seo/schema-script.tsx`
- Keyword density off → adjust content in section component files
- H2 missing geo-term → edit content in the relevant section file

### Step 5 — Re-run audit after fixes

If critical issues were found and fixed, re-run `/seo page` to verify.

---

## Scoring

Count passed items from Part 1 + Part 2 findings (0 Abweichungen nach Fix) + Part 3 results:

`[passed] / [total] — [percentage]%`

- ≥ 95%: EXCELLENT — ready to present
- 90-94%: GOOD — minor tweaks
- < 90%: FIX before proceeding to Phase 11

**Do NOT proceed to Phase 11 with unfixed critical issues or with unresolved Playwright-Abweichungen.**
