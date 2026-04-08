# Phase 9 — Build Homepage

## References
- Load `./references/code-standards.md` for Next.js standards
- Load `./references/ui-rules.md` for STRICT CSS/code rules

## CRITICAL: Design aus Phase 8 anwenden

Lies Phase 8 Output. Extrahiere ALLE Werte (Farben, Fonts, Atmosphäre).
Jede Farbe, jeder Font im Code MUSS aus Phase 8 kommen.

---

## Build Process

### Step 1 — Init Next.js + shadcn
```bash
npx shadcn@latest init --preset b0 --template next
```

### Step 2 — shadcnblocks Registry einrichten

Frage den User nach dem shadcnblocks API Key:
```
shadcnblocks braucht einen API Key für Premium-Blocks.
→ Hol deinen Key bei https://shadcnblocks.com
→ Paste deinen API Key hier:
```

Wenn User den Key gibt, füge die Registry in `components.json` hinzu:
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

Setze den API Key als ENV Variable:
```bash
export SHADCNBLOCKS_API_KEY="[USER_KEY]"
```

**Den Key NIE in Dateien speichern die committed werden.**

Wenn User "skip" sagt → baue Komponenten manuell mit shadcn Primitives.

### Step 3 — globals.css (MUSS Phase 8 matchen)

Schreibe die EXAKTEN Werte aus Phase 8:
```css
:root {
  --primary: [EXACT from Phase 8];
  --secondary: [EXACT from Phase 8];
  --accent: [EXACT from Phase 8];
  --background: [EXACT from Phase 8];
  --foreground: [EXACT from Phase 8];
  --muted: [EXACT from Phase 8];
  --border: [EXACT from Phase 8];
  --radius: [EXACT from Phase 8];
}
```

Fonts, Section-Spacing und Atmosphäre-Styles ebenfalls in globals.css.

### Step 4 — Site Config
`lib/site-config.ts` — NAP aus Project Brief. Single source of truth.

### Step 5 — Blocks auswählen und installieren

Für JEDE der 8 Homepage-Sections:

**5a. Passenden Block suchen:**
Via shadcn MCP oder CLI nach passenden @shadcnblocks Blocks suchen.
Beispiele:
```bash
npx shadcn add @shadcnblocks/hero2
npx shadcn add @shadcnblocks/stats1
npx shadcn add @shadcnblocks/feature3
npx shadcn add @shadcnblocks/testimonial2
npx shadcn add @shadcnblocks/cta1
npx shadcn add @shadcnblocks/footer4
```

**5b. Block bewerten:**
- Passt die Struktur zum Content aus Phase 6?
- Ist das Layout für die Branche angemessen?
- Wenn nicht → anderen Block probieren oder manuell bauen

### Step 6 — Content einsetzen
Ersetze ALLEN Platzhalter-Text in den Blocks mit dem echten Content aus Phase 6.

### Step 7 — MANDATORY: /frontend-design für Feintuning

**Jeden Block mit dem /frontend-design Skill verfeinern.**

Der Block ist der Rohling. /frontend-design macht daraus ein Meisterwerk:
- **Komposition**: Asymmetrie, Whitespace, visuelle Hierarchie überarbeiten
- **Typografie**: Schriftgrössen, Gewichte, Zeilenabstände feintunen
- **Atmosphäre**: Gradients, Texturen, Tiefe hinzufügen wo es passt
- **Motion**: Gezielte Animationen für Scroll-Reveals, Hover-States
- **Micro-Interactions**: Buttons, Cards, Navigation lebendig machen
- **Visueller Rhythmus**: Spacing zwischen Sections harmonisieren

**NICHT generisch lassen.** Der Block muss nach dem Feintuning unverwechselbar aussehen.

### Step 8 — UI Rules Check

Lade `./references/ui-rules.md` und prüfe JEDEN Component:
- [ ] Keine hardcoded Farben (hex/rgb/hsl) → nur shadcn Variablen
- [ ] Keine hardcoded Fonts → nur `font-heading`, `font-body`
- [ ] Keine inline `style={}` Props
- [ ] Alle Hover-States konsistent
- [ ] Image Placeholder wo Bilder fehlen (nie leere Spaces)
- [ ] Border Radius über --radius
- [ ] Schatten über Tailwind shadow Klassen

### Step 9 — Layout
```
components/layout/header.tsx    — Navigation, Logo
components/layout/footer.tsx    — NAP, Links, Öffnungszeiten
```

### Step 10 — Assemble
```
app/layout.tsx      — Root Layout mit Header, Footer, SchemaScript
app/page.tsx        — Homepage: alle Sections + Metadata Export
```

### Step 11 — Schema
```
components/seo/schema-script.tsx  — JSON-LD Injection
```

### Step 12 — Final Check
- [ ] `npm run build` passes
- [ ] Alle Farben aus globals.css (keine hardcoded)
- [ ] /frontend-design wurde auf jeden Block angewendet
- [ ] Kein generisches AI-UI Look
- [ ] Image Placeholder wo Bilder fehlen
- [ ] Do NOT start dev server
- [ ] Do NOT take screenshots
