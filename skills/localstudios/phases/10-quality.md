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
- [ ] 1500-2500 words total
- [ ] E-E-A-T signals present
- [ ] No filler/placeholder text
- [ ] Tone consistent

### Images
- [ ] Jede Section hat mindestens ein Bild oder einen ImagePlaceholder
- [ ] Mindestens 5 Bilder pro Variante
- [ ] All images have alt text (+ title where relevant)
- [ ] Above-fold images have `priority`

### design.md compliance
- [ ] `globals.css` enthält alle Tokens aus `design.md` (Farben, Typografie-Scale, Buttons, Radius, Section-Spacing)
- [ ] Keine hardcoded Farben in Components (kein `bg-blue-600`, kein `#1E3A8A`, kein inline `style={}`)
- [ ] Fonts kommen via `next/font` und Utilities `font-heading` / `font-body`
- [ ] Button-Shape/Padding/Radius matchen `design.md`
- [ ] Anti-Muster aus `design.md` eingehalten

### Custom Components
- [ ] Jede Section ist ein eigenes File in `components/sections/variant-N/`
- [ ] Semantisches HTML (section/article/header/footer statt generischer divs)
- [ ] Keine Pill-Badges für Orte/Labels
- [ ] Keine fremden Block-Libraries importiert

### Variants
- [ ] N Varianten rendern korrekt (`/`, `/variant-2`, `/variant-3`)
- [ ] Kein Section-Layout wiederholt sich über Varianten
- [ ] Content + Farben + Fonts sind identisch über Varianten
- [ ] Variant Switcher sichtbar und funktioniert
- [ ] Jede Variante hat eine klare visuelle Persönlichkeit (matcht Blueprint-Namen)

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

### Step 2 — Screenshot-Loop pro Variante

Für JEDE Variante (`/`, `/variant-2`, `/variant-3`):

1. **Desktop Screenshot**
   - Playwright MCP: navigate zu `http://localhost:3000<path>`
   - Viewport: 1440×900
   - Full-page Screenshot
   - Console-Messages abfragen → Errors protokollieren
   - Network-Requests abfragen → 404/5xx protokollieren

2. **Mobile Screenshot**
   - Viewport: 375×812
   - Full-page Screenshot

3. **Abgleich gegen `design.md`** (visuell analysieren):
   - **Farben:** Primary/Accent/Background visuell erkennbar und konsistent mit `design.md`?
   - **Typografie-Hierarchie:** H1 dominiert? H2 klar kleiner aber lesbar? Body-Text angenehm? Scale matcht `design.md`?
   - **Section-Spacing:** konsistent (`py-16 md:py-24 lg:py-32`) — keine Section die zu eng wirkt?
   - **Atmosphäre:** Whitespace/Shadows/Radius entsprechen dem was `design.md` vorgibt?
   - **Bilder:** Jede Section hat ein Bild? Min. 5 Bilder sichtbar? Kein leerer Space?
   - **Buttons:** Auf JEDEM Section-Hintergrund lesbar? Keine unsichtbaren oder zu kleinen Buttons?
   - **Pill-Badges:** Keine sichtbar? Orte/Labels clean als Text?
   - **Persönlichkeit der Variante:** matcht der Blueprint-Name die visuelle Wirkung? (Variant 1 "Editorial Boldness" sollte nicht identisch zu Variant 2 "Warm Immersion" aussehen)

4. **Abweichungen fixen**
   - Für jede Abweichung das verantwortliche File identifizieren und sofort anpassen
   - Häufige Fixes:
     - Buttons zu klein → Button-Component variants anpassen (`px-8 py-3` → grösser)
     - Heading zu wenig dominant → Scale in globals.css erhöhen (matcht dann auch `design.md`)
     - Section fühlt sich eng → Container-Padding oder Section-Spacing erhöhen
     - Variante sieht wie andere aus → Blueprint-Schwerpunkt stärker implementieren (andere Komposition, anderer Rhythmus)
   - **NICHT** hardcoded Farben hinzufügen. Fixes passieren über `globals.css` (wenn System-weit) oder über Layout-Komposition (wenn varianten-spezifisch).

5. **Re-Screenshot nach Fix** → bis Match.

### Step 3 — Console & Network Report

Am Ende pro Variante kurz auflisten:
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
