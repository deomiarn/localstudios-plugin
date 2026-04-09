# Phase 8 — Design System

## Kein externer Skill — Design kommt aus dem Design Brief

Der User hat in Phase 2 einen Design Brief gegeben:
- Referenz-Screenshots/URLs
- Adjektive
- Farben
- Typografie-Präferenz
- Anti-Muster

---

### Step 1 — Referenzen analysieren

Wenn der User Referenz-URLs gegeben hat:
- Fetch die URLs (Playwright oder WebFetch)
- Extrahiere: Farbpalette, Layout-Muster, Typografie, Atmosphäre
- Notiere: Was macht diese Seite gut? Was übernehmen wir?

Wenn Screenshots/Bilder statt URLs:
- Analysiere visuell: Farben, Layout, Spacing, Typografie-Stil

### Step 2 — Design System schreiben

Erstelle `design-system.md` im Projekt-Root:

```markdown
# Design System — [Company Name]

## Inspiration
- Referenz: [URL/Screenshot-Beschreibung]
- Was übernommen wird: [Layout-Muster, Farbgefühl, Typografie-Stil]

## Gefühl
[3-5 Adjektive aus Design Brief]

## Farben (HSL für shadcn)
- --primary: [hsl] — [Beschreibung, z.B. "warmes Blau"]
- --primary-foreground: [hsl]
- --secondary: [hsl]
- --accent: [hsl] — [CTA Farbe]
- --accent-foreground: [hsl]
- --background: [hsl]
- --foreground: [hsl]
- --muted: [hsl]
- --muted-foreground: [hsl]
- --border: [hsl]
- --radius: [rem]

## Typografie
- Heading: [Font Name] (Google Fonts)
- Body: [Font Name] (Google Fonts)
- Begründung: [warum diese Kombination zum Gefühl passt]

## Atmosphäre
- Hintergrund: [solid / subtle gradient / texture]
- Schatten: [none / subtle / medium]
- Rundungen: [sharp / medium / rounded]
- Whitespace: [normal / grosszügig / sehr grosszügig]
- Animationen: [minimal / dezent / lebendig]

## Anti-Muster (aus Design Brief)
- [Was der User NICHT will]
- [Was zur Branche nicht passt]

## Bilder
- Minimum 5 Bilder auf der Homepage
- Stil: [passend zu Adjektiven — warm/clean/professionell]
- Placeholder: Gradient mit --primary/--accent, nie leere Spaces
```

### Step 3 — In globals.css übertragen

Alle Werte aus design-system.md werden in Phase 9 exakt in globals.css geschrieben.
design-system.md ist die Quelle, globals.css ist die Umsetzung.

## Output

`design-system.md` im Projekt-Root. Wird von Phase 9 geladen.
