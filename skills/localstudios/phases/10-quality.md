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

**Playwright MCP ist für diese Phase Pflicht.** Claude führt Screenshots selbst aus und prüft ZWEI Dinge:
1. **Compliance**: Stimmen Farben/Typo/Spacing mit `design.md` überein?
2. **Design-Qualität**: Ist jede Section harmonisch, proportional, visuell stark — gemessen an den Prinzipien aus dem `frontend-design` Skill?

Beide müssen passen. Wenn die Seite zwar Token-korrekt aber design-technisch schwach wirkt, wird trotzdem gefixt.

### Step 1 — Dev/Build starten

```bash
# Dev-Server reicht für Screenshot-Abgleich:
npm run dev
# oder für produktions-ähnlichen Check:
npm run build && npm run start
```

Warten bis Server bereit ist (Port 3000 default).

### Step 2 — frontend-design Skill aktivieren

Falls nicht mehr aus Phase 8 geladen → jetzt laden. Die Quality-Gate-Kriterien (klarer visual point of view, intentional Typography + Spacing, Color/Motion supporting statt dekorierend, nicht AI-generic, production-grade) sind die Mess-Latte für Step 4.

### Step 3 — Screenshots sammeln

Für die Homepage (`/`):

1. **Desktop Full-Page** — Viewport 1440×900, full-page Screenshot
2. **Desktop Above-the-Fold** — Viewport 1440×900, nur sichtbarer Bereich (Hero-Check)
3. **Mobile Full-Page** — Viewport 375×812, full-page
4. **Mobile Above-the-Fold** — Viewport 375×812, nur sichtbarer Bereich
5. **Section-by-Section** — für jede der 10 Sections einen engen Screenshot (Section-Bereich durch `section[data-section="…"]` oder via Scroll-Position). Dadurch kann jede Section isoliert auf Design-Qualität beurteilt werden.

Console-Messages + Network-Requests parallel abfragen → 404/5xx protokollieren.

### Step 4 — Zwei-Schicht-Review

#### 4a. Compliance-Review (Token-Match gegen `design.md`)

- **Farben:** Primary/Accent/Background visuell erkennbar und konsistent mit `design.md`?
- **Typografie:** H1 dominiert? H2 klar kleiner aber lesbar? Body-Text angenehm? Scale matcht `design.md`?
- **Section-Spacing:** `py-16 md:py-24 lg:py-32` konsistent?
- **Atmosphäre-Tokens:** Whitespace/Shadows/Radius entsprechen `design.md`?
- **Buttons:** Auf JEDEM Section-Hintergrund lesbar? Shape matcht `design.md`?
- **Anti-Muster aus `design.md`:** nichts verletzt?

#### 4b. Design-Qualität-Review (pro Section — Mess-Latte: `frontend-design` Skill)

Jede Section einzeln durchgehen und bewerten:

**Hero**
- Above-the-fold-Bild sichtbar? Proportion (`aspect-[4/5]`/`aspect-[3/4]`) wirkt nicht zu flach?
- Composition: asymmetrisch-stark oder nur zentriert-langweilig?
- Headline hat visuelle Dominanz (Scale + Weight + Tracking)?
- Hero wirkt wie ein **intentional gestalteter** Opener, nicht wie ein SaaS-Template?

**Trust Bar**
- Zahlen haben hierarchische Dominanz (gross) vs. Labels (klein)?
- Rhythmus harmonisch (3-4 Elemente, gleicher Abstand)?
- Fühlt sich die Section nicht zu dünn/zu schwer an?

**Featured Service 1 + 2**
- Alternierender Rhythmus Bild/Text wirkt bewusst?
- Bild-Proportionen stimmen mit dem Text-Block zusammen?
- CTA sitzt richtig (nicht verloren, nicht zu laut)?

**Services Grid**
- Card-Rhythmus harmonisch (gleiche Höhe, gleicher Content-Flow)?
- Nicht zu dense, nicht zu luftig — passt zum `design.md` Gefühl?
- Hierarchie pro Card klar (Icon/Bild → Name → Beschreibung)?

**About Teaser**
- Portrait + Text balanciert?
- Story-Text nicht zu lang (max. 2-3 kompakte Absätze)?
- Persönliches Gefühl spürbar — nicht generisch corporate?

**Social Proof**
- Testimonials lesbar, nicht zu kompakt?
- Quote-Hierarchie (Zitat > Name > Ort) klar?
- Keine AI-Generic-Karten-Optik?

**Local Area + Map**
- Map-Embed hat richtige Höhe (nicht winzig, nicht übergross)?
- Ort-Liste clean (kein Pill-Badge)?
- Section wirkt als lokaler Anker, nicht als Footer-Vorgänger?

**CTA Section**
- CTA visuell dominierend — der Fokus-Punkt der Section?
- Kontrast auf Section-Background stimmt?
- Kein Informations-Overload — nur 1 klare Aktion?

**Footer**
- Mehrspaltiger Rhythmus ausgewogen?
- NAP auffindbar (nicht versteckt)?
- Typografie-Hierarchie gedämpft aber lesbar?

**Gesamt-Harmonie (ganze Homepage)**
- Alternierender Rhythmus zwischen Sections (breit/schmal, hell/dunkel, dicht/luftig) vorhanden?
- Keine zwei aufeinanderfolgenden Sections die visuell identisch wirken?
- Vertical Rhythm konsistent (Section-Padding, Headline-zu-Body-Abstand)?
- Gesamte Seite hat einen klaren visuellen Standpunkt — nicht „safe average"?
- Quality Gate aus `frontend-design` erfüllt: intentional, nicht generic, production-grade?

### Step 5 — Fixen

**Für jede Abweichung (Compliance ODER Design-Qualität) → SOFORT im Code fixen.**

Beispiele für Design-Qualitäts-Fixes:
- Hero wirkt zentriert-langweilig → auf Split-Layout mit grösserem Bild umbauen
- Trust Bar Zahlen zu klein → Scale der Zahlen erhöhen
- Services Grid Cards uneinheitlich → `grid-auto-rows: 1fr` oder feste Card-Höhe
- About Portrait zu klein → aspect-ratio + min-size anheben
- CTA Section fühlt sich flach → Background-Intensität (primary statt muted) erhöhen oder padding aufblasen
- Zwei aufeinanderfolgende Sections sehen gleich aus → Background-Alternation einführen (`bg-secondary/30`)
- Section-Rhythmus bricht ab → vertikales Padding auf eine Section anpassen
- Page wirkt generic → stärkere Typo-Hierarchie (grössere H1, tighter Tracking), Atmosphäre-Akzent aus `design.md` herausarbeiten (Shadows, Radius, Textur)

**Nie:**
- Hardcoded Farben einführen
- `design.md` anpassen — wenn der Code vom Design abweicht, ist der Code falsch, nicht die Vorlage
- Komplette Sections löschen wenn sie schwach wirken — stattdessen umbauen

### Step 6 — Re-Screenshot → bis beide Reviews (Compliance + Design-Qualität) sauber sind

### Step 7 — Console & Network Report

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
