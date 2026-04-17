# UI Rules — Strikte CSS/Code Regeln

Diese Regeln gelten IMMER. Kein Code wird geschrieben ohne sie einzuhalten.
Alle Styles kommen aus `design.md` → `globals.css`. Keine hardcoded Farben. Keine fremden Block-Libraries.

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
Bei Full-Bleed Backgrounds: äusseres `<section>` mit Background, inneres `<div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">` für den Content.

### Container Standard
```tsx
className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8"
```
Standard-Container für ALLE Sections. Keine Ausnahmen ausser Full-Width Hero-Bilder (das Bild selbst darf bleeden, der Text-Content nicht).

---

## Buttons — via Utility-Klassen aus globals.css

Buttons werden NIEMALS mit Inline-Tailwind für Farben gebaut. Sie nutzen die `.btn-*` Utility-Klassen aus globals.css (die wiederum CSS-Vars aus design.md nutzen).

```tsx
import { Button } from "@/components/ui/button"

// primary — bg-primary text-primary-foreground
<Button variant="primary">Termin vereinbaren</Button>

// secondary — bg-secondary text-secondary-foreground + border
<Button variant="secondary">Mehr erfahren</Button>

// outline — transparent + border-primary + text-foreground (auf hellem bg)
<Button variant="outline">Öffnungszeiten</Button>

// outline-on-dark — für dunkle Section-Backgrounds (WICHTIG — sonst unsichtbar)
<Button variant="outline-on-dark">044 853 22 74</Button>
```

### Button-Shape
Shape kommt aus `design.md` (Button-Sektion: pill / rounded / sharp). Wird zentral via `--radius-btn` gesetzt. Components erben das automatisch.

### Button-Regeln
- **Grosszügiges Padding** — die `.btn-primary` Utility enthält `px-8 py-3`. Nicht kleiner machen.
- **Secondary MUSS sichtbar sein** — die Utility-Klasse kümmert sich darum (border-2 border-border).
- **Auf dunklem Section-Background** IMMER `variant="outline-on-dark"` (NIE generisches `border border-white/20` oder Tailwind-Default).
- **Icons** links vom Text, mit `gap-2`.

### VERBOTEN
```tsx
<button className="bg-blue-600 text-white">…</button>         // hardcoded
<button className="bg-[#1E3A8A]">…</button>                    // hardcoded
<button style={{ backgroundColor: "#333" }}>…</button>         // inline style
<button className="px-3 py-1">…</button>                       // zu klein
```

---

## Typografie — aus design.md, via next/font

### Fonts sind in design.md festgelegt
- `next/font/google` Loader in `app/layout.tsx`
- CSS Vars `--font-heading` / `--font-body`
- Utilities `font-heading` / `font-body` in Components

```tsx
<h1 className="font-heading">…</h1>
<p className="font-body text-muted-foreground">…</p>
```

### Schrift-Hierarchie in globals.css
Scale kommt aus `design.md`. Typische Startwerte:
```css
@layer base {
  h1 { @apply text-4xl md:text-5xl lg:text-6xl font-extrabold tracking-tight; letter-spacing: -0.02em; }
  h2 { @apply text-3xl md:text-4xl font-bold tracking-tight; letter-spacing: -0.015em; }
  h3 { @apply text-xl md:text-2xl font-semibold; }
  p  { @apply text-base md:text-lg leading-relaxed; color: hsl(var(--muted-foreground)); }
}
```

Passe die Skala wenn `design.md` andere Werte vorgibt.

---

## Farben — ausschliesslich CSS Vars

```tsx
/* IMMER: */
className="bg-primary"
className="text-muted-foreground"
className="bg-accent"
className="bg-primary/10"   /* opacity OK */

/* NIEMALS: */
className="bg-[#1E3A8A]"
className="text-blue-600"
className="bg-white"        /* → bg-background */
className="text-black"      /* → text-foreground */
style={{ color: '#333' }}
```

---

## Labels & Tags — KEINE Pill-Badges

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

Prinzip: Clean und simple. Kein visuelles Rauschen.

---

## Dienstleistungen — Highlight-Sections

### 2-3 Hauptdienstleistungen als eigene Sections
Nicht nur Grid-Karten — die wichtigsten Services bekommen eine **eigene Feature-Section** mit:
- Grosses Bild
- Ausführlicher Text (3-4 Sätze)
- Eigener CTA
- Alternierend links/rechts Layout

### Service-Grid für die restlichen
Die übrigen Services als kompaktere Karten im Grid.

### Jeder Service ein Bild
Wenn möglich: eigenes Bild pro Dienstleistung.
Wenn nicht: stilvoller `<ImagePlaceholder>` mit Service-Label.

---

## Google Maps — korrekte Einbindung

### Maps URL wird im Interview gefragt
Phase 2 muss fragen: „Google Maps Embed URL für den Standort?"

### Einbindung
```tsx
<iframe
  src={siteConfig.mapsEmbedUrl}
  className="w-full h-[400px] rounded-xl border-0"
  allowFullScreen
  loading="lazy"
  referrerPolicy="no-referrer-when-downgrade"
  title={`Standort ${siteConfig.name} auf Google Maps`}
/>
```

**NIEMALS** eine generische Maps URL erfinden.

---

## Spacing & Sections

### Section-Spacing konsistent
```css
.section {
  @apply py-16 md:py-24 lg:py-32;
  @apply mx-auto max-w-7xl px-4 sm:px-6 lg:px-8;
}
```

### Alternating Backgrounds
Jede zweite Section mit leichtem Hintergrund:
```tsx
<section className="section bg-secondary/30">…</section>
```

### Full-Bleed Backgrounds
```tsx
<section className="w-full bg-primary text-primary-foreground">
  <div className="py-16 md:py-24 lg:py-32 mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">…</div>
</section>
```

---

## Schatten & Tiefe

Aus `design.md` Atmosphäre-Sektion:
```tsx
className="shadow-sm"    /* subtil */
className="shadow-md"    /* Standard */
className="shadow-lg"    /* Hover/CTAs */
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

NIEMALS leere Spaces. IMMER stilvolle Placeholder über die zentrale Component:

```tsx
import { ImagePlaceholder } from "@/components/ui/image-placeholder"

<ImagePlaceholder label="Praxisfoto" className="aspect-video" />
```

Die Component nutzt Gradient aus `--primary` / `--accent` mit `--radius` — gleiche Proportionen und Farbschema wie echte Bilder.

---

## Custom Components — Regeln

1. **Pro Section ein eigenes File** in `components/sections/variant-N/<section>.tsx`.
2. **Semantisches HTML** — `<section>`, `<article>`, `<header>`, `<footer>`, `<h1>`-`<h3>`, `<p>`, `<ul>`. Keine div-Suppe.
3. **Content als Props oder direkt inline** — Phase 6 Content hart reingeschrieben (identisch über Varianten).
4. **Kein style={}** — niemals.
5. **Keine hardcoded Farben** — niemals.
6. **Keine fremden Block-Libraries** — keine shadcn-Blocks, keine getemplateten Sections aus externen Registries.
7. **Jede Section braucht ein Bild** — scraped (next/image) oder `<ImagePlaceholder>`.
8. **Accessibility** — `alt` auf Images, `aria-label` auf Icon-only Buttons, `tel:` auf Telefon-Links.

---

## Verboten

- `style={}` Props
- Hardcoded Farben (hex, rgb, hsl-Literale in Components)
- Links-aligned Sections (IMMER zentriert)
- Weisse Buttons mit weissem Text (oder andere unlesbare Kombinationen)
- Generische kleine eckige Buttons
- Leere Spaces statt Bilder
- `bg-white` / `text-black` (→ `bg-background` / `text-foreground`)
- Pill-Badges für Orte/Labels
- Google Maps URL erfinden statt fragen
- System-Fonts wenn `design.md` andere Fonts vorgibt
- Fremde Block-Libraries (shadcn-Blocks, Tailwind UI Templates als Kopie, etc.)
