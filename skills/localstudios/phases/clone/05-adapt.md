# Clone Phase 5 — Adapt Project

## References
- Load `./references/content-guidelines.md` for writing rules
- Load `./references/image-strategy.md` for image sourcing + alt text
- Load `./references/page-sections.md` for section structure
- Load `./references/ui-rules.md` for CSS constraints
- Load `docs/SEO-STRATEGY.md` for keyword-to-section mapping (from Phase 3)

## Task
Copy the source project and adapt EVERYTHING for the new client.

---

### Step 1 — Copy Source Project
```bash
cp -R [source-path]/ .
rm -rf .git node_modules .next
```

### Step 2 — design.md aktualisieren (nur falls Style geändert)

Lies die Design-Änderungs-Entscheidung aus `docs/BUSINESS.md` / Clone Brief.

#### Fall „Keep" (Style vom Source übernehmen)
- `design.md` aus Source bleibt **unverändert** (READ-ONLY).
- `globals.css` bleibt unverändert.
- Kein getdesign-Call.

#### Fall „Nur Farbwechsel"
- `design.md` bleibt strukturell identisch.
- **Nur** die explizit genannten Farb-Tokens in `design.md` ersetzen (z.B. `--primary` Wert).
- Danach ist `design.md` wieder READ-ONLY.

#### Fall „Anpassen" oder „Neu"
- Lies die Design Source (Command ODER Pfad) aus `docs/BUSINESS.md`.
- **Wenn getdesign Command:**
  ```bash
  <exakter command aus Phase 3>
  ```
  Das von getdesign erzeugte File als `design.md` im Projekt-Root ablegen — **unverändert**.
- **Wenn bestehende design.md Pfad:**
  ```bash
  cp "<Pfad>" ./design.md
  ```
- **design.md NICHT normalisieren.** Keine Ergänzung, keine Umformulierung. Fehlt eine Pflicht-Angabe → User fragen.
- Ab jetzt: `design.md` ist READ-ONLY.

### Step 3 — globals.css regenerieren (nur falls Style geändert)

Alle Tokens aus neuer `design.md` in `app/globals.css` übertragen:
- CSS-Vars in `@layer base :root`
- Tailwind v4 `@theme inline` Mapping
- Fonts via `next/font` im layout.tsx

Struktur aus `references/code-standards.md` folgen.

### Step 4 — site-config.ts updaten

Alle Business-Daten ersetzen:
- Company name, legal name
- Full NAP (address, phone, email)
- Opening hours
- Social links
- Google Maps URL
- Description mit neuem Primary Keyword

### Step 5 — Content in ALLEN Section-Components schreiben

**Load `references/content-guidelines.md` — follow ALL rules.**

Für JEDES Section-Component-File in `components/sections/variant-N/*.tsx`:
1. READ the file
2. REPLACE all text content — FRESH schreiben für neuen Client (kein blosses Find/Replace)
3. Keyword-to-Section Mapping aus `docs/SEO-STRATEGY.md` respektieren
4. Layout, Grid, Responsive behalten — nur Text/Bilder ersetzen
5. CHECK Buttons: auf dem Section-Hintergrund lesbar?
6. CHECK Zentrierung: `mx-auto max-w-7xl` Container vorhanden (via Tailwind-Utilities)?

**Write 1500-2000 Wörter total über alle 10 Sections** (+ optional FAQ 200-300).

**Section-Wörterzahlen (gleich wie generate, siehe page-sections.md):**
- Hero: 150-250 Wörter — H1 mit primary KW + city
- Trust Bar: 50-100 Wörter — 3-4 konkrete Zahlen
- Featured Service 1: 150-250 Wörter — Hauptservice, 3-4 Sätze + CTA
- Featured Service 2: 150-250 Wörter — zweiter Hauptservice
- Services Grid: 150-300 Wörter — weitere Services als Cards
- About: 200-350 Wörter — Gründungsstory, persönliche Motivation
- Social Proof: 150-250 Wörter — 3-4 detaillierte Testimonials
- Local Area: 150-250 Wörter — warum dieser Standort, Gebiete
- CTA: 100-150 Wörter — Werteversprechen-Zusammenfassung
- Footer: 100-150 Wörter — full NAP, hours, links

**Tonalität** (aus Clone Brief):
- Ton aus Interview nutzen (formal / persönlich / freundlich / fachlich)
- An Branche anpassen (medical formeller, kreativ casual)
- Professionell aber persönlich — aus Erfahrung schreiben
- Konkrete Zahlen und Details
- Lokale Referenzen die echt wirken

**VERBOTEN:**
- „Willkommen auf unserer Website"
- „Wir sind ein führendes…"
- „Herzlich willkommen bei…"
- Generische Phrasen ohne Substanz
- Keyword Stuffing

**SEO Rules:**
- H1: primary keyword + city, genau einmal
- Geo-term im ersten Absatz + mindestens einer H2
- Keyword density 1-2%
- Jedes Bild: alt mit keyword + location + description
- Jedes Bild: title mit benefit/CTA

**Meta Tags:**
- Meta Title: Primary KW — Company — City (max 60 chars)
- Meta Description: Benefit + CTA + Location (max 155 chars)
- OG Title + OG Description

### Step 6 — Schema updaten

Edit `components/seo/schema-script.tsx`:
- Neuer Company-Name, NAP, Opening Hours
- Neuer Industry `@type` (Dentist, Restaurant, Plumber, etc.)
- Neue sameAs URLs
- Neue description mit keywords

### Step 7 — Metadata updaten

Edit `app/layout.tsx`:
- Neues meta title (Primary KW — Company — City)
- Neue meta description
- Neue OG tags
- Richtiges `lang` attribute

### Step 8 — Images (Follow image-strategy.md)

**Source Priority (cost-aware):**
1. Scrape von NEW client Website (Phase 2 Scraping — Bilder wiederverwenden)
2. Scrape von Google Business Profile (wenn GBP URL vorhanden)
3. AI Generation via /banana (letzter Ausweg — nur für Hero/Backgrounds)

**Minimum 5 Bilder auf der Homepage** (siehe image-strategy.md):
- Hero: 1x hero (full-width oder split)
- Featured Services: 1x pro Featured Service
- Services Grid: 1x pro Card wenn möglich
- About: 1x team/owner
- Social Proof: optional Profilbilder
- Local Area: Google Maps Embed
- Storefront/Exterior: wenn verfügbar

**Alt Text — Templates aus image-strategy.md:**
| Section | Alt Text Pattern |
|---------|-----------------|
| Hero | [Service] in [City] — [was gezeigt wird] |
| Team | [Name], [Role] at [Company] in [City] |
| Service | [Service] — [brief description] |
| Storefront | [Company] office in [City/District] |

**Source Tracking — in `docs/pages/home.md` dokumentieren:**
```
| Image | Source | Original URL | Status |
|-------|--------|-------------|--------|
| hero.webp | New website | https://… | ✅ Scraped |
| team.webp | GBP | https://… | ✅ Scraped |
| … | — | — | ❌ Placeholder |
```

**Styled Placeholders wo Bilder fehlen — niemals leere Spaces:**
```tsx
<ImagePlaceholder label="Praxisfoto" className="aspect-video" />
```

### Step 9 — docs/ updaten

- Neue `docs/BUSINESS.md` (inklusive Design-Eintrag)
- Neue `docs/SEO-STRATEGY.md` mit keyword mapping
- Neue `docs/pages/home.md` mit image source tracking

### Step 10 — Reinstall Dependencies
```bash
npm install
```

### Step 11 — Build Check
```bash
npm run build
```
Muss passen bevor QA.

### Checks After Adaptation
- [ ] ALL text ist neuer Client-Content (kein alter Client-Text übrig)
- [ ] Content 1500-2000 Wörter (nicht dünn / generisch; FAQ mit 6 Fragen extra erlaubt)
- [ ] Ton matcht Interview-Spezifikation
- [ ] Keywords in den richtigen Sections laut SEO-STRATEGY.md
- [ ] site-config.ts hat neue NAP
- [ ] design.md aktuell (bei Style-Change regeneriert)
- [ ] globals.css matcht design.md (bei Style-Change)
- [ ] Schema hat neue Business-Daten + industry @type
- [ ] Metadata hat neues title/description/OG
- [ ] Buttons auf allen Backgrounds lesbar (kein Tailwind-Default-Outline auf dunkel)
- [ ] Sections zentriert (mx-auto max-w-7xl Container via Tailwind)
- [ ] Kein alter Client-Name irgendwo im Code
- [ ] Min 5 Bilder auf der Homepage (scraped oder `<ImagePlaceholder>`)
- [ ] Hero hat Bild above-the-fold
- [ ] Keine `[Image …]` Text-Platzhalter im JSX
- [ ] Alle Bilder mit alt + title
- [ ] Image Sources in docs/pages/home.md dokumentiert
- [ ] Keine hardcoded Farben in Components
- [ ] Keine Pill-Badges
- [ ] `npm run build` passes
