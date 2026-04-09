# Phase 9 — Build Homepage

## References
- Load `./references/code-standards.md`
- Load `./references/ui-rules.md`
- Load `design-system.md` (created in Phase 8)

## Architecture

```
design-system.md     → QUELLE (Farben, Fonts, Atmosphäre aus Design Brief)
shadcnblocks         → FUNDAMENT (Block-Struktur passend zur Referenz)
/frontend-design     → FEINTUNING (unverwechselbar machen)
ui-rules.md          → KONTROLLE (keine hardcoded Farben)
```

## CRITICAL: design-system.md ist Gesetz

Lies `design-system.md`. Jede Farbe, Font, Spacing-Entscheidung kommt von dort.

---

## Build Process

### Step 1 — Init Next.js + shadcn
```bash
npx shadcn@latest init --preset b0 --template next
```

### Step 2 — shadcnblocks Registry
Key aus `.env.local` (gespeichert in Phase 0):
```json
// components.json
{
  "registries": {
    "@shadcnblocks": {
      "url": "https://shadcnblocks.com/r/{name}",
      "headers": {
        "Authorization": "Bearer ${SHADCNBLOCKS_API_KEY}"
      }
    }
  }
}
```
Key steht NUR in `.env.local`. NIEMALS direkt in components.json.

### Step 3 — globals.css (EXAKT aus design-system.md)

Alle Werte aus `design-system.md` übertragen:
- Farben als HSL in :root CSS Variablen
- Fonts via `next/font` importieren
- Section-Spacing, Shadows, Radius
- Heading-Styles, Body-Styles

### Step 4 — Site Config
`lib/site-config.ts` — NAP aus Project Brief.

---

### Step 5 — Blocks auswählen

Für JEDE Section einzeln:

**5a.** Lies Content aus Phase 6. Was braucht diese Section?
**5b.** Suche shadcnblocks mit spezifischen Queries:
```
shadcn search_items_in_registries @shadcnblocks
```
Queries basierend auf Referenz-Website und Content-Bedarf:
```
GUT:  "hero gradient image right medical"
GUT:  "testimonial cards minimal 3 columns"
SCHLECHT: "hero" (zu generisch)
```
**5c.** Bewerte gegen design-system.md — passt der Block zum Gefühl?
**5d.** Nicht zufrieden → weitersuchen mit anderen Keywords
**5e.** Installieren: `source .env.local && npx shadcn add @shadcnblocks/[name]`

---

### Step 6 — Blocks VERWENDEN

**Die Block-Datei IST dein Component. EDITIERE sie direkt.**

1. LESE die Block-Datei
2. EDITIERE:
   - Platzhalter → Phase 6 Content
   - Hardcoded Farben → shadcn CSS Variablen
   - Hardcoded Fonts → font-heading, font-body
3. BEHALTE: Layout, Grid, Responsive, Accessibility

**VERBOTEN**: Neue Datei erstellen die den Block ersetzt.

---

### Step 7 — /frontend-design Feintuning

**MANDATORY: Invoke /frontend-design auf die editierten Block-Dateien.**

/frontend-design arbeitet INNERHALB der design-system.md Vorgaben:
- **Komposition**: Whitespace, Hierarchie, Asymmetrie
- **Typografie**: Grössen, Gewichte, Spacing feintunen
- **Atmosphäre**: Gradients, Tiefe, Texturen (passend zum Gefühl)
- **Motion**: Hover-States, Transitions, Scroll-Reveals
- **Liquid Glass**: Wo passend, Glassmorphism/Frosted-Glass Effekte einsetzen
- **Visueller Rhythmus**: Section-Spacing harmonisieren

**Nicht generisch lassen. Der Block muss unverwechselbar aussehen.**

---

### Step 8 — UI Rules Check

Load `./references/ui-rules.md`:
- [ ] Keine hardcoded Farben
- [ ] Keine inline `style={}`
- [ ] Image Placeholder wo Bilder fehlen (min. 5 Bilder auf Homepage)
- [ ] Hover-States konsistent
- [ ] Border Radius über --radius

### Step 9 — Layout
Header + Footer: shadcnblocks wenn passend, sonst selbst bauen.
Alles mit design-system.md Farben.

### Step 10 — Assemble
```
app/layout.tsx — Root Layout mit Header, Footer, SchemaScript
app/page.tsx   — Homepage: alle Block-Components + Metadata
```

### Step 11 — Schema
```
components/seo/schema-script.tsx
```

### Step 12 — Final Check
- [ ] `npm run build` passes
- [ ] Farben matchen design-system.md
- [ ] Blocks wurden verwendet (nicht ersetzt)
- [ ] /frontend-design wurde angewendet
- [ ] Min. 5 Bilder (Placeholder wo nötig)
- [ ] Keine Anti-Muster aus design-system.md verletzt
