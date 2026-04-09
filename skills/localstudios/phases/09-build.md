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

### Step 5 — Blocks auswählen (VARIIEREN!)

**Do NOT always pick the same blocks.** Search broadly, compare options.

Für JEDE Section einzeln:

**5a.** Lies Content aus Phase 6. Was braucht diese Section?

**5b.** Suche shadcnblocks — MINDESTENS 2 verschiedene Queries pro Section:
```
shadcn search_items_in_registries @shadcnblocks
```
Erste Suche spezifisch:
```
"hero split image gradient"
"testimonial minimal quote large"
"feature alternating image text"
```
Zweite Suche mit anderen Keywords:
```
"hero centered dark overlay"
"review cards stars grid"  
"services showcase icons"
```

**5c.** Vergleiche die Ergebnisse. Wähle den der am besten zum Design Brief passt.
**5d.** Installieren: `source .env.local && npx shadcn add @shadcnblocks/[name]`

---

### Step 6 — Blocks VERWENDEN und FIXEN

**Die Block-Datei IST dein Component. EDITIERE sie direkt.**

1. LESE die Block-Datei
2. EDITIERE:
   - Platzhalter → Phase 6 Content
   - Hardcoded Farben → shadcn CSS Variablen
   - Hardcoded Fonts → font-heading, font-body
3. BEHALTE: Layout, Grid, Responsive, Accessibility

**DANN sofort diese 3 Checks auf JEDEN Block:**

**CHECK 1 — Ist die Section zentriert?**
Jeder Block MUSS einen zentrierten Container haben:
```tsx
<div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
```
Wenn nicht vorhanden → hinzufügen. Kein Block darf links-aligned sein.

**CHECK 2 — Sind Buttons lesbar?**
Für JEDEN Button im Block prüfen:
- Auf dunklem Hintergrund → heller Button mit dunklem Text
- Auf hellem Hintergrund → dunkler Button mit hellem Text
- Secondary Button: hat sichtbaren Border UND lesbaren Text?
- Teste: `bg-X text-Y` — ist Y auf X lesbar? Wenn nein → fixen.
```tsx
// AUF DUNKLEM bg:
className="bg-white text-primary"        // ✅ lesbar
className="bg-white text-white"          // ❌ UNSICHTBAR
className="border-white text-white/50"   // ❌ kaum sichtbar

// AUF HELLEM bg:
className="bg-primary text-primary-foreground" // ✅ lesbar
```

**CHECK 3 — Keine Pill-Badges?**
Wenn der Block `rounded-full border px-3 py-1` Tags hat → durch clean Text ersetzen:
```tsx
// Raus: <span className="rounded-full border ...">Ort</span>
// Rein: Ort · Ort · Ort (als Text)
```

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
