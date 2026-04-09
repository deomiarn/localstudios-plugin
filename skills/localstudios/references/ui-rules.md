# UI Rules — Strikte CSS/Code Regeln

Diese Regeln gelten IMMER. Kein Code wird geschrieben ohne sie einzuhalten.

---

## Layout & Zentrierung

### ALLE Sections zentriert
```css
/* In globals.css — PFLICHT */
.section {
  @apply py-16 md:py-24 lg:py-32;
  @apply mx-auto max-w-7xl px-4 sm:px-6 lg:px-8;
}
```

**Jede Section MUSS zentriert sein.** Kein links-aligned Content.
Wenn ein shadcnblocks Block nicht zentriert ist → Container-Wrapper hinzufügen:
```tsx
<section className="section">
  {/* Block-Inhalt hier */}
</section>
```

### Container Standard
```tsx
className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8"
```
Das ist der Standard-Container für ALLE Sections. Keine Ausnahmen ausser Full-Width Hero-Bilder.

---

## Buttons — zum Style passend, grosszügig, sichtbar

### Button-Shape: Claude entscheidet basierend auf Design Brief
- Pill (rounded-full) → für weiche, moderne, freundliche Designs
- Rounded (rounded-lg) → für professionelle, seriöse Designs
- Sharp (rounded-sm) → für brutale, industrielle Designs

**Claude wählt die Shape passend zum Gefühl aus dem Design Brief.**

### Button-Varianten in globals.css
```css
/* Primary CTA — auffällig, einladend */
.btn-primary {
  @apply bg-primary text-primary-foreground font-semibold;
  @apply px-8 py-3;  /* grosszügiges Padding */
  @apply hover:bg-primary/90 transition-all duration-200;
  @apply shadow-md hover:shadow-lg;
  @apply text-base md:text-lg;
  border-radius: var(--radius-btn);  /* aus Design System */
}

/* Secondary — klar sichtbar, kontrastreich */
.btn-secondary {
  @apply bg-secondary text-secondary-foreground font-medium;
  @apply px-6 py-2.5;
  @apply border-2 border-border;
  @apply hover:bg-accent hover:text-accent-foreground transition-all duration-200;
  border-radius: var(--radius-btn);
}

/* Outline */
.btn-outline {
  @apply bg-transparent text-foreground font-medium;
  @apply px-6 py-2.5;
  @apply border-2 border-primary;
  @apply hover:bg-primary hover:text-primary-foreground transition-all duration-200;
  border-radius: var(--radius-btn);
}
```

### Button-Regeln
- **Grosszügiges Padding**: min `px-6 py-2.5`, CTA noch mehr
- **Secondary MUSS sichtbar sein** — kontrastreicher Text, sichtbarer Border
- **Hover**: sichtbare Veränderung (Farbe, Shadow, oder Scale)
- **Icons**: links vom Text, mit `gap-2`

### VERBOTEN
```tsx
className="bg-white text-white"      /* unsichtbar! */
className="px-3 py-1"               /* zu klein */
className="bg-gray-100 text-gray-400" /* sieht disabled aus */
```

---

## Typografie — Charakter statt generisch

### Fonts MÜSSEN zum Style passen
Wähle Fonts die zum Design Brief passen. KEINE generischen System-Fonts.

**Gute Kombinationen nach Branche:**
- Medizin/Dental: Plus Jakarta Sans + Inter (modern, vertrauenswürdig)
- Legal/Finanzen: Playfair Display + Source Sans 3 (seriös, elegant)
- Kreativ: Clash Display + Satoshi (mutig, frisch)
- Gastronomie: Fraunces + DM Sans (warm, einladend)
- Handwerk: Space Grotesk + Inter (robust, klar)
- Premium: Cormorant Garamond + Outfit (luxuriös)

### Schrift-Hierarchie klar definieren
```css
/* In globals.css */
h1 {
  @apply text-4xl md:text-5xl lg:text-6xl font-extrabold tracking-tight;
  letter-spacing: -0.02em;
}
h2 {
  @apply text-3xl md:text-4xl font-bold tracking-tight;
  letter-spacing: -0.015em;
}
h3 {
  @apply text-xl md:text-2xl font-semibold;
}
p {
  @apply text-base md:text-lg leading-relaxed;
  color: var(--muted-foreground);
}
```

---

## Farben

### ALLE Farben über shadcn CSS Variablen
```tsx
/* IMMER: */
className="bg-primary"
className="text-muted-foreground"
className="bg-accent"
className="bg-primary/10"  /* opacity OK */

/* NIEMALS: */
className="bg-[#1E3A8A]"
className="text-blue-600"
className="bg-white"       /* → bg-background */
className="text-black"     /* → text-foreground */
style={{ color: '#333' }}
```

---

## Labels & Tags — KEINE Pill-Badges

### Keine generischen Label/Tag/Badge Elemente verwenden.
Stattdessen: clean, simple, direkt in den Text integriert.

```tsx
/* VERBOTEN — generische Pill-Badges: */
<span className="rounded-full border px-3 py-1 text-sm">Regensdorf</span>
<span className="rounded-full bg-primary/10 px-4 py-1.5 text-sm">Regensdorf</span>

/* STATTDESSEN — clean inline: */
<p className="text-muted-foreground">
  Regensdorf · Steinmaur · Oberglatt · Niederhasli
</p>

/* Oder als einfache Liste: */
<ul className="flex flex-wrap gap-x-6 gap-y-1 text-sm text-muted-foreground">
  <li>Regensdorf</li>
  <li>Steinmaur</li>
  <li>Oberglatt</li>
</ul>
```

**Prinzip: Clean und simple. Kein visuelles Rauschen durch unnötige Badge-Elemente.**

---

## Dienstleistungen — Highlight-Sections

### 2-3 Hauptdienstleistungen als eigene Sections
Nicht nur eine Service-Grid Karte — die wichtigsten Services bekommen eine **eigene Feature-Section** mit:
- Grosses Bild
- Ausführlicher Text (3-4 Sätze)
- Eigener CTA
- Alternierend links/rechts Layout

### Service-Grid für die restlichen
Die übrigen Services als kompaktere Karten im Grid.

### Jeder Service ein Bild
Wenn möglich: eigenes Bild pro Dienstleistung.
Wenn nicht: stilvoller Placeholder mit Service-Icon.

---

## Google Maps — korrekte Einbindung

### Maps URL wird im Interview gefragt
Phase 2 muss fragen: "Google Maps Embed URL für den Standort?"

### Einbindung
```tsx
<iframe
  src="[USER_PROVIDED_MAPS_URL]"
  className="w-full h-[400px] rounded-xl border-0"
  allowFullScreen
  loading="lazy"
  referrerPolicy="no-referrer-when-downgrade"
  title="Standort [Company] auf Google Maps"
/>
```

**NIEMALS** eine generische Maps URL erfinden. Den User nach der korrekten Embed-URL fragen.

---

## Spacing & Sections

### Section Spacing konsistent
```css
.section {
  @apply py-16 md:py-24 lg:py-32;
  @apply mx-auto max-w-7xl px-4 sm:px-6 lg:px-8;
}
```

### Alternating Backgrounds
Jede zweite Section mit leichtem Hintergrund:
```tsx
<section className="section bg-secondary/30">
```

---

## Schatten & Tiefe

```tsx
className="shadow-sm"    /* subtil */
className="shadow-md"    /* Standard für Karten */
className="shadow-lg"    /* für Hover-States oder CTAs */
```

---

## Hover & Focus

```tsx
className="hover:bg-primary/90 transition-all duration-200"
className="hover:shadow-lg hover:-translate-y-0.5 transition-all duration-300"
className="focus-visible:ring-2 focus-visible:ring-ring"
```

---

## Image Placeholder

NIEMALS leere Spaces. IMMER stilvolle Placeholder:
```tsx
<div className="aspect-video rounded-xl bg-gradient-to-br from-primary/20 to-accent/20 flex items-center justify-center">
  <span className="text-muted-foreground text-sm">Praxisfoto</span>
</div>
```

Placeholder: gleiche Proportionen, Rundungen, Farbschema.

---

## shadcnblocks Anpassung

1. Block installieren
2. **Container zentrieren** (mx-auto max-w-7xl wenn nicht vorhanden)
3. Hardcoded Farben → shadcn Variablen
4. Generische Buttons → btn-primary / btn-secondary / btn-outline
5. Fonts → font-heading, font-body
6. Labels/Tags → stilvoll statt generisch
7. Content ersetzen

---

## Verboten

- `style={}` Props
- Hardcoded Farben (hex, rgb, hsl)
- Links-aligned Sections (IMMER zentriert)
- Weisse Buttons mit weissem Text
- Generische kleine eckige Buttons
- Leere Spaces statt Bilder
- `bg-white` / `text-black` (→ `bg-background` / `text-foreground`)
- Generische Pill-Badges mit dünnem Border
- Google Maps URL erfinden statt fragen
- System-Fonts wenn bessere Font-Pairing möglich
