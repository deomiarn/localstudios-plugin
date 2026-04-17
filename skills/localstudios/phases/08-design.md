# Phase 8 — Design System (from getdesign / design.md)

## Outputs
1. **`design.md`** im Projekt-Root — Single Source of Truth (Farben, Fonts, Buttons, Spacing, Atmosphäre, Anti-Muster). Von allen Varianten geteilt.
2. **`variant-blueprints.md`** im Projekt-Root — N verschiedene Layout-Strategien pro Section (ohne Block-Queries, ohne fremde Libraries).

---

## Step 1 — design.md beschaffen

Lies die **Design Source** aus `docs/BUSINESS.md` (in Phase 2 gespeichert).

### Fall A — getdesign CLI Command vorhanden

Beispiel: `npx getdesign@latest add nike`

```bash
# Aus Projekt-Root ausführen
<exakter command aus Phase 2>
```

- Den Command **exakt** ausführen wie vom User angegeben.
- Output prüfen: legt der Command eine `design.md` / `DESIGN.md` / Tokens-File im Projekt ab?
  - Wenn ja → Datei als `design.md` im Projekt-Root verwenden (ggf. umbenennen/verschieben).
  - Wenn der Command stattdessen in ein Unterverzeichnis schreibt → Inhalt in `design.md` im Root konsolidieren.
  - Wenn der Command fehlschlägt → User klar informieren und um Klärung bitten (Command falsch? Brand existiert nicht?).

### Fall B — Pfad zu existierender design.md vorhanden

```bash
cp "<Pfad aus Phase 2>" ./design.md
```

Falls die Datei bereits im Projekt-Root liegt → belassen.

---

## Step 2 — design.md normalisieren

Die `design.md` MUSS folgende Sektionen enthalten, damit Phase 9 sauber `globals.css` ableiten kann.
Fehlt etwas im Output von `getdesign` → ergänzen, aber **niemals** Werte erfinden die im Widerspruch zu den gelieferten Tokens stehen.

```markdown
# Design — [Company Name]

## Source
- Origin: [getdesign command "___"  |  existing design.md path "___"]
- Brand Reference: [nike, apple, luxury, …] (falls getdesign genutzt)

## Gefühl
[Adjektive aus Phase 2 — falls angegeben]

## Farben (HSL für CSS-Variablen)
- --background: [hsl]
- --foreground: [hsl]
- --primary: [hsl]
- --primary-foreground: [hsl]
- --secondary: [hsl]
- --secondary-foreground: [hsl]
- --accent: [hsl]           ← CTA-Farbe
- --accent-foreground: [hsl]
- --muted: [hsl]
- --muted-foreground: [hsl]
- --border: [hsl]
- --ring: [hsl]

## Typografie
- Heading Font: [Name] (Google Fonts loader: `next/font/google`)
- Body Font:    [Name] (Google Fonts loader: `next/font/google`)
- Scale:
  - h1: [z.B. text-4xl md:text-5xl lg:text-6xl, font-extrabold, tracking-tight, letter-spacing -0.02em]
  - h2: [...]
  - h3: [...]
  - p:  [...]

## Buttons (werden als Tailwind-Utilities im Button-Component komponiert)
- Primary:   bg-primary text-primary-foreground, padding [z.B. px-8 py-3], hover [bg-primary/90 + shadow]
- Secondary: bg-secondary text-secondary-foreground, border border-border, hover [bg-secondary/80]
- Outline:   transparent, border-2 border-primary, hover [bg-primary text-primary-foreground]
- Shape / Radius: kommt aus `--radius` (z.B. rounded-lg → 0.5rem; rounded-full → pill-shape)

## Section & Layout
- Section Spacing (Tailwind-Utilities): py-16 md:py-24 lg:py-32
- Container (Tailwind-Utilities):       mx-auto max-w-7xl px-4 sm:px-6 lg:px-8
- Radius-Token: --radius [0.5rem | 0.75rem | ...]

## Atmosphäre
- Background-Stil: [solid | subtle gradient | grain texture]
- Shadows: [none | subtle | medium | dramatic]
- Rundungen: [sharp | medium | rounded]
- Whitespace: [normal | grosszügig | sehr grosszügig]
- Animationen: [minimal | dezent | lebendig]

## Bilder
- Minimum 5 Bilder auf der Homepage pro Variante
- Jede Section bekommt mindestens ein Bild (Wärme/Leben)
- Placeholder: Gradient mit --primary/--accent, nie leere Spaces

## Anti-Muster
- [was der User NICHT will — aus Phase 2]
- [was zur Branche nicht passt]
- Keine Pill-Badges für Orte/Labels
- Keine hardcoded Hex/RGB im Component-Code
- Kein `style={}` inline
- Keine fremden Block-Libraries
```

**Phase 9 liest diese Datei 1:1 in `globals.css` ein.** Was hier nicht steht, existiert im Build nicht.

---

## Step 3 — variant-blueprints.md schreiben

Lies die Varianten-Anzahl aus dem Project Brief (default: 3).
Für jede Variante eine eigenständige Layout-Strategie definieren — **alle Varianten nutzen dieselbe `design.md`**, unterscheiden sich aber in Komposition und Rhythmus.

```markdown
# Variant Blueprints — [Company Name]

## Shared Foundation
- Farben/Fonts/Spacing: design.md (identisch über alle Varianten)
- Content: Phase 6 (identisch)
- Schema: Phase 7 (identisch)

---

## Variant 1: [Persönlichkeits-Name, z.B. "Editorial Boldness"]

### Visual Strategy
- Layout-Rhythmus: [z.B. "Alternierend breite/schmale Sections, asymmetrischer Hero"]
- Visueller Schwerpunkt: [z.B. "Typografie-getrieben — übergrosse Headings"]
- Atmosphäre-Akzent: [z.B. "Dezente Grain-Textur, tiefe Schatten"]

### Section Layout Plan
| Section | Layout-Ansatz | Komposition | Warum zur Persönlichkeit |
|---------|---------------|-------------|--------------------------|
| Hero | Split: Text 60% links, Bild 40% rechts, asymmetrisch | Grosse Typo + CTA + sekundärer CTA inline | Editorial — Typo dominiert |
| Trust Bar | Inline Zahlen, minimal, full-width auf akzentfarbener Sektion | 4 Zahlen + kurzer Begleittext | Kein visuelles Rauschen |
| Featured Service 1 | Full-Width Bild mit Overlay-Text (dunkel) | Headline + 3-4 Sätze + eigener CTA | Cinematisch, Bild dominiert |
| Featured Service 2 | Alternierend: Text links, Bild rechts | Headline + 3-4 Sätze + eigener CTA | Rhythmus-Wechsel |
| Services Grid | 3-Spalten Cards, minimal, viel Whitespace | Icon + Name + 1-2 Sätze pro Service | Swiss-style Ruhe |
| About Teaser | Split mit Owner-Portrait rechts | 2-3 Absätze, Gründungsjahr + Stadt | Persönliche Note |
| Social Proof | Grosse Zitate, zentriert, 1 pro Row | 3-4 Testimonials, Sterne, Ort-Nennung | Typo-Fokus passt |
| Local Area + Map | Text links, Maps-Embed rechts | Stadt/Stadtteile/Seit Jahr | GEO-Signal stark |
| CTA Section | Full-Width Accent-Background | Grosser CTA + Telefon clickable | Abschluss-Akzent |
| Footer | Dezent, mehrspaltig, NAP + Social | Identisch mit Header-Links | Clean end |

### Images pro Section
| Section | Bild-Idee | Scraped? |
|---------|-----------|----------|
| Hero | Owner/Team in Action | ja/nein → Placeholder mit Gradient |
| Trust Bar | — (pure numbers) | |
| Featured Service 1 | Service-Detail-Shot | |
| Featured Service 2 | Service-Detail-Shot | |
| Services Grid | 1x pro Card (Icon-Style) | |
| About Teaser | Owner Portrait | |
| Social Proof | Optional: Kunden-Profilbilder | |
| Local Area | Ort/Storefront + Maps-Embed | |
| CTA | Gradient-Background, optional Foto | |
| Footer | Logo | |

---

## Variant 2: [KOMPLETT ANDERE Persönlichkeit]

Visual Strategy + Section Layout Plan + Images pro Section — alle Felder anders als Variant 1.

---

## Variant 3: [KOMPLETT ANDERE Persönlichkeit]

Visual Strategy + Section Layout Plan + Images pro Section — alle Felder anders als Variant 1 und 2.
```

### Diversity-Regeln (KRITISCH)

1. **Kein Section-Layout darf sich über Varianten wiederholen.**
   - Wenn Variant 1 einen Split-Hero nutzt, MUSS Variant 2 einen anderen Hero-Stil haben (Centered Overlay, Minimal-Text, Full-Bleed Video Placeholder, etc.).
2. **Jede Variante hat einen klaren Persönlichkeits-Namen** (2-3 Worte). Der Name steuert alle Layout-Entscheidungen.
3. **Visueller Schwerpunkt unterscheidet sich.** Eine Variante bild-schwer, eine typografie-fokussiert, eine grid/textur-schwer. Sie sollen aussehen als hätten verschiedene Designer sie gemacht — aber auf derselben `design.md` Basis.
4. **Layout-Rhythmus unterscheidet sich.** Variant 1 alterniert breit/schmal; Variant 2 nutzt durchgehend Full-Width; Variant 3 arbeitet mit engem Grid.
5. **Jede Section hat Bild-Zuweisung.** Phase 9 darf keine leere Section bauen.

---

## Step 4 — Validierung

Bevor Phase 9 startet, prüfen:
- [ ] `design.md` ist im Projekt-Root und hat alle Pflicht-Sektionen (Farben, Typografie, Buttons, Section/Layout, Atmosphäre, Anti-Muster)
- [ ] `variant-blueprints.md` hat exakt N Varianten
- [ ] Kein Section-Layout wiederholt sich über Varianten
- [ ] Jede Variante hat Persönlichkeits-Namen, Visual Strategy, Section Layout Plan, Images pro Section
- [ ] Jede Section pro Variante hat ein Bild-Plan (scraped ODER Placeholder)
- [ ] Anti-Muster aus Phase 2 sind in `design.md` eingetragen

## Output

- `design.md` — im Projekt-Root (shared über alle Varianten)
- `variant-blueprints.md` — im Projekt-Root (N verschiedene Layout-Pläne für Phase 9)
