# Phase 9 — Build Homepage

## References
- Load `./references/code-standards.md` for Next.js standards
- Load `./references/ui-rules.md` for STRICT CSS/code rules
- Load `design-system/MASTER.md` (created by ui-ux-pro-max in Phase 8)

## Architecture

```
ui-ux-pro-max    → LEITET den Prozess (Design System, Style, Anti-Patterns)
shadcnblocks     → FUNDAMENT (Block-Struktur als Startpunkt)
/frontend-design → FEINTUNING (Komposition, Atmosphäre, Motion)
ui-rules.md      → KONTROLLE (keine hardcoded Farben, alles über Variables)
```

## CRITICAL: Design System aus Phase 8 ist Gesetz

Lies `design-system/MASTER.md`. Extrahiere:
- Style-Name (z.B. "Soft UI Evolution")
- Alle Farben (hex → HSL für shadcn)
- Font-Pairing
- Effects (Schatten, Transitions, Hover)
- Anti-Patterns
- Landing Page Pattern

**Jede Zeile Code MUSS dem MASTER.md entsprechen.**

---

## Build Process

### Step 1 — Init Next.js + shadcn
```bash
npx shadcn@latest init --preset b0 --template next
```

### Step 2 — shadcnblocks Registry

**CRITICAL: API Key gehört in .env.local, NIEMALS direkt in components.json.**

**Step 2a** — Key in `.env.local` speichern (falls nicht schon in Phase 0 geschehen):
```bash
echo 'SHADCNBLOCKS_API_KEY=[key from Phase 0]' >> .env.local
```
Stelle sicher dass `.env.local` in `.gitignore` steht.

**Step 2b** — Registry in `components.json` mit ENV-Referenz:
```json
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
`${SHADCNBLOCKS_API_KEY}` wird automatisch aus der Umgebungsvariable gelesen.
Der echte Key steht NUR in `.env.local`.

**Step 2c** — Beim Installieren von Blocks die ENV-Variable laden:
```bash
source .env.local && npx shadcn add @shadcnblocks/[name]
```

**VERBOTEN:**
- Key direkt in components.json als String (`"Bearer sk_live_..."`)
- Key in irgendeiner Datei die committed wird
- Key loggen oder in Output anzeigen

### Step 3 — globals.css (EXAKT aus MASTER.md)

Alle Werte aus `design-system/MASTER.md`:
```css
:root {
  --primary: [EXACT from MASTER.md, converted to HSL];
  --secondary: [EXACT from MASTER.md];
  --accent: [EXACT CTA color from MASTER.md];
  --background: [EXACT from MASTER.md];
  --foreground: [EXACT from MASTER.md];
  --muted: [derived from MASTER.md palette];
  --border: [derived from MASTER.md palette];
  --radius: [from MASTER.md effects];
}
```

Effects aus MASTER.md auch in globals.css:
- Shadow values
- Transition durations
- Hover state styles
- Section spacing

### Step 4 — Site Config
`lib/site-config.ts` — NAP aus Project Brief.

---

### Step 5 — Blocks INTELLIGENT auswählen

Für JEDE Section einzeln:

#### 5a. Was braucht die Section?
Lies Content aus Phase 6 UND Landing Page Pattern aus MASTER.md.
- Wie viele Elemente?
- Welches Layout empfiehlt MASTER.md?
- Welche Atmosphäre/Style?

#### 5b. Gezielt suchen
```
shadcn search_items_in_registries @shadcnblocks
```
Spezifische Queries basierend auf MASTER.md Pattern:
```
GUT: "hero centered gradient medical"
GUT: "stats cards 4 columns clean"
GUT: "testimonial minimal single quote"
SCHLECHT: "hero" (zu generisch)
```

#### 5c. Bewerten gegen MASTER.md
- Passt die Struktur zum empfohlenen Landing Page Pattern?
- Passt der Block-Style zum gewählten UI Style?
- Ist der Block kompatibel mit den Anti-Patterns? (z.B. kein "AI purple gradient" wenn MASTER.md das verbietet)

#### 5d. Weitersuchen wenn nötig
Andere Keywords, verwandte Begriffe. Erst installieren wenn überzeugt.

#### 5e. Installieren
```bash
export SHADCNBLOCKS_API_KEY="[key]" && npx shadcn add @shadcnblocks/[name]
```

---

### Step 6 — Blocks VERWENDEN (nicht neu coden)

**Die installierte Block-Datei IST dein Component.**

Für jeden Block:
1. **LESE** die Block-Datei
2. **EDITIERE** direkt:
   - Platzhalter-Text → Phase 6 Content
   - Hardcoded Farben → shadcn CSS Variablen (--primary, --accent etc.)
   - Hardcoded Fonts → font-heading, font-body
3. **BEHALTE**: Layout, Grid, Responsive, Accessibility

**VERBOTEN**: Neue Datei erstellen die den Block ersetzt.

---

### Step 7 — /frontend-design Feintuning

Invoke `/frontend-design` auf die editierten Block-Dateien.

Aber: **/frontend-design arbeitet INNERHALB der ui-ux-pro-max Vorgaben.**
- Style: wie in MASTER.md definiert
- Farben: nur die aus globals.css
- Anti-Patterns: respektieren was MASTER.md verbietet

Was /frontend-design verbessert:
- **Komposition**: Asymmetrie, Whitespace, Hierarchie
- **Typografie**: Grössen, Gewichte, Abstände feintunen
- **Atmosphäre**: Gradients, Texturen, Tiefe (im Rahmen des Styles)
- **Motion**: Hover-States, Transitions (mit MASTER.md Werten)
- **Visueller Rhythmus**: Spacing zwischen Sections

---

### Step 8 — ui-ux-pro-max Review

Nach dem Feintuning: ui-ux-pro-max nochmal für Review nutzen:

```
/ui-ux-pro-max review homepage
```

Prüft:
- Entspricht das Ergebnis dem gewählten Style?
- Sind Anti-Patterns eingehalten?
- Accessibility (Kontrast, Touch-Targets, Focus States)
- Responsive Breakpoints (375, 768, 1024, 1440px)

**Fixes aus dem Review sofort umsetzen.**

---

### Step 9 — UI Rules Check

Load `./references/ui-rules.md`:
- [ ] Keine hardcoded Farben
- [ ] Keine hardcoded Fonts
- [ ] Keine inline `style={}`
- [ ] Image Placeholder wo Bilder fehlen
- [ ] Alle Hover-States konsistent
- [ ] Border Radius über --radius

### Step 10 — Layout
```
components/layout/header.tsx
components/layout/footer.tsx
```
Auch diese: wenn shadcnblocks navbar/footer Blocks passen → verwenden.
Sonst selbst bauen, aber mit MASTER.md Design System.

### Step 11 — Assemble
```
app/layout.tsx — Root Layout mit Header, Footer, SchemaScript
app/page.tsx   — Homepage: alle Block-Components + Metadata
```

### Step 12 — Schema
```
components/seo/schema-script.tsx
```

### Step 13 — Final Check
- [ ] `npm run build` passes
- [ ] Farben matchen MASTER.md exakt
- [ ] Blocks wurden verwendet (nicht ersetzt)
- [ ] /frontend-design wurde angewendet
- [ ] ui-ux-pro-max Review bestanden
- [ ] Keine Anti-Patterns aus MASTER.md verletzt
- [ ] Image Placeholder wo nötig
