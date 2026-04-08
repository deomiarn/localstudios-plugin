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

Die Registry und der API Key wurden in Phase 0 eingerichtet.
Füge die Registry in `components.json` hinzu:
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

### Step 3 — globals.css (MUSS Phase 8 matchen)

Schreibe die EXAKTEN Werte aus Phase 8 in globals.css.
Fonts, Section-Spacing und Atmosphäre-Styles ebenfalls.

### Step 4 — Site Config
`lib/site-config.ts` — NAP aus Project Brief. Single source of truth.

---

### Step 5 — Blocks INTELLIGENT auswählen

**Für JEDE Homepage-Section einzeln durchgehen. Nicht alle auf einmal suchen.**

Pro Section diesen Prozess durchlaufen:

#### 5a. Überlegen was die Section braucht
Lies den Content aus Phase 6 für diese Section. Frage dich:
- Wie viele Text-Elemente hat die Section? (Headline, Subline, Body, etc.)
- Gibt es Karten/Grid-Elemente? Wie viele?
- Braucht es ein Bild? Wo?
- Gibt es einen CTA? Primär oder sekundär?
- Welches Layout passt? (zentriert, split, asymmetrisch)

#### 5b. Gezielt suchen
Suche mit SPEZIFISCHEN Queries die zum Content passen:
```
shadcn search_items_in_registries @shadcnblocks
```

Beispiele für gute vs. schlechte Queries:
```
SCHLECHT: "hero"          → zu generisch, 100 Ergebnisse
GUT:     "hero split image left cta"  → spezifisch zum Layout
GUT:     "hero centered gradient background"

SCHLECHT: "testimonial"   → zu generisch
GUT:     "testimonial cards grid 3 columns"
GUT:     "testimonial single quote large"

SCHLECHT: "feature"       → zu generisch
GUT:     "feature cards icons 6 grid"
GUT:     "feature list alternating image"
```

#### 5c. Ergebnisse bewerten
Für jedes Suchergebnis prüfen:
- Passt die Struktur zum Content? (Anzahl Items, Layout)
- Passt es zur Branche? (Medical → clean, nicht verspielt)
- Hat es die richtigen Slots? (Bild, Text, CTA wo nötig)

#### 5d. Nicht zufrieden? Weitersuchen.
Wenn kein Ergebnis passt:
- Andere Keywords probieren
- Verwandte Begriffe nutzen ("stats" statt "trust", "pricing" statt "services")
- Kombinationen testen ("about team photo" statt "about")

**Erst installieren wenn du ÜBERZEUGT bist dass der Block passt.**

#### 5e. Block installieren
```bash
export SHADCNBLOCKS_API_KEY="[key]" && npx shadcn add @shadcnblocks/[block-name]
```

---

### Step 6 — Blocks VERWENDEN (nicht neu coden!)

**DAS IST DIE WICHTIGSTE REGEL:**

Der installierte Block liegt jetzt als Datei im Projekt (z.B. `components/blocks/hero7.tsx`).

**Diese Datei IST dein Component. Du schreibst KEINEN neuen.**

Für jeden installierten Block:

1. **LESE die Block-Datei** — verstehe die Struktur
2. **BEARBEITE die Block-Datei direkt** mit dem Edit Tool:
   - Ersetze Platzhalter-Text mit echtem Content aus Phase 6
   - Ersetze hardcoded Farben mit shadcn CSS Variablen
   - Passe die Daten an (Namen, Beschreibungen, Zahlen)
3. **BEHALTE:**
   - Das Layout und die Struktur des Blocks
   - Die responsive Breakpoints
   - Die Component-Komposition (Imports, Sub-Components)
   - Die Accessibility-Attribute

**VERBOTEN:**
- Eine neue Datei wie `components/sections/hero.tsx` erstellen die den Block ignoriert
- Den Block lesen und dann "inspiriert davon" etwas Neues schreiben
- Den Block-Code kopieren und in eine neue Datei einfügen
- Den Block-Import in page.tsx durch einen eigenen Component ersetzen

**Der Block bleibt die Datei. Du EDITIERST sie. Fertig.**

---

### Step 7 — /frontend-design Feintuning

**Nach dem Content-Einsetzen: Jeden Block-File mit /frontend-design verfeinern.**

Invoke `/frontend-design` und bearbeite die Block-Dateien:
- **Komposition**: Whitespace, visuelle Hierarchie verbessern
- **Typografie**: Schriftgrössen, Gewichte feintunen
- **Atmosphäre**: Gradients, Tiefe, Texturen wo passend
- **Motion**: Gezielte Hover-States, Transitions
- **Visueller Rhythmus**: Spacing zwischen Sections harmonisieren

**Auch hier: Die Block-Datei EDITIEREN, keine neue Datei erstellen.**

---

### Step 8 — UI Rules Check

Lade `./references/ui-rules.md` und prüfe JEDEN Block:
- [ ] Keine hardcoded Farben → nur shadcn Variablen
- [ ] Keine hardcoded Fonts → nur `font-heading`, `font-body`
- [ ] Keine inline `style={}` Props
- [ ] Alle Hover-States konsistent
- [ ] Image Placeholder wo Bilder fehlen (nie leere Spaces)
- [ ] Border Radius über --radius
- [ ] Schatten über Tailwind shadow Klassen

---

### Step 9 — Layout + Header/Footer

Header und Footer sind KEINE shadcnblocks — die baust du selbst:
```
components/layout/header.tsx    — Navigation, Logo
components/layout/footer.tsx    — NAP, Links, Öffnungszeiten
```

Oder wenn passende shadcnblocks navbar/footer Blocks gefunden wurden → auch diese verwenden und anpassen (gleicher Prozess wie Step 5-6).

### Step 10 — Assemble
```
app/layout.tsx      — Root Layout mit Header, Footer, SchemaScript
app/page.tsx        — Homepage: importiert alle Block-Components + Metadata
```

In `page.tsx` die Blocks in der richtigen Reihenfolge importieren — die Dateinamen sind die shadcnblocks Namen (z.B. `hero7`, `feature1`, `testimonial8`).

### Step 11 — Schema
```
components/seo/schema-script.tsx  — JSON-LD Injection
```

### Step 12 — Final Check
- [ ] `npm run build` passes
- [ ] Alle Farben aus globals.css (keine hardcoded)
- [ ] Alle Components sind die ORIGINALEN Block-Dateien (editiert, nicht ersetzt)
- [ ] Keine selbst-gecodeten Section-Components neben ungenutzten Blocks
- [ ] /frontend-design wurde auf jeden Block angewendet
- [ ] Image Placeholder wo Bilder fehlen
- [ ] Do NOT start dev server
- [ ] Do NOT take screenshots
