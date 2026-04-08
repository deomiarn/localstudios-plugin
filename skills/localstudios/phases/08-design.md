# Phase 8 — Design Direction

## Design kommt aus Interview + Branche + /frontend-design Skill

### Process

1. **Aus dem Interview extrahieren:**
   - Gewünschter Stil / Referenz-Website
   - Bestehende Markenfarben (falls vorhanden)
   - Branche und Zielgruppe

2. **Design-Richtung festlegen:**
   Basierend auf der Branche eine klare Richtung wählen. Nicht mischen.
   - Medizin/Dental → clean, vertrauenswürdig, warm
   - Handwerk → robust, ehrlich, bodenständig
   - Gastronomie → einladend, appetitlich, warm
   - Legal/Finanzen → seriös, elegant, nüchtern
   - Kreativ/Design → mutig, visuell, unkonventionell

3. **Farbpalette definieren:**
   - Primary: Hauptfarbe (aus Brand oder Branche)
   - Secondary: Ergänzungsfarbe
   - Accent: CTA-Farbe (muss sich abheben)
   - Background, Foreground, Muted, Border

4. **Typografie wählen:**
   - Heading Font: mit Charakter, passend zur Branche
   - Body Font: gut lesbar, professionell
   - Beide via Google Fonts (`next/font`)

5. **Atmosphäre definieren:**
   - Gradient/Textur für Hintergründe?
   - Schatten-Stufe (subtil vs. stark)
   - Rundungen (sharp vs. rounded)
   - Animations-Niveau (minimal vs. lebendig)

## Output

```
DESIGN DIRECTION
Style: [Richtung]
Mood: [Atmosphäre in 3 Worten]

COLORS (als shadcn HSL Variablen)
--primary: [hsl]
--secondary: [hsl]
--accent: [hsl]
--background: [hsl]
--foreground: [hsl]
--muted: [hsl]
--border: [hsl]
--radius: [rem]

TYPOGRAPHY
Heading: [Font Name]
Body: [Font Name]

ATMOSPHERE
Background: [solid/gradient/texture]
Shadows: [none/subtle/medium]
Roundness: [sharp/medium/full]
Motion: [minimal/moderate/expressive]
```

Pass to Phase 9 — alles wird in globals.css geschrieben.
