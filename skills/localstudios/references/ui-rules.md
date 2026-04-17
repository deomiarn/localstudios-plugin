# UI Rules — Strikte CSS/Code Regeln

Diese Regeln gelten IMMER. Kein Code wird geschrieben ohne sie einzuhalten.

**Prinzip:** Tailwind-first. `globals.css` ist nur Tokens (CSS-Vars aus `design.md` + Tailwind v4 `@theme` Mapping + optional Font-Base). Alle Styles landen als Tailwind-Utilities in den Components. Keine hardcoded Farben. Keine fremden Block-Libraries.

---

## Layout & Zentrierung — via Tailwind-Utilities

Keine `.section`-Custom-Klasse. Zentrierung direkt in den Components:

```tsx
<section className="py-16 md:py-24 lg:py-32">
  <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
    {/* content */}
  </div>
</section>
```

Bei Full-Bleed Backgrounds: äusseres `<section>` mit Background (`bg-primary text-primary-foreground` etc.), inneres `<div>` mit dem Container.

Vertikales Section-Padding ist **immer** `py-16 md:py-24 lg:py-32` — wenn `design.md` einen anderen Rhythmus vorgibt, die Werte konsistent über alle Sections anwenden.

---

## Buttons — Tailwind-Utilities komponiert im Button-Component

Der Button-Component lebt in `components/ui/button.tsx` und komponiert Tailwind-Utilities per Variant — **keine `.btn-*` Custom-Classes in globals.css**:

```tsx
import { Button } from "@/components/ui/button"

<Button variant="primary">Termin vereinbaren</Button>
<Button variant="secondary">Mehr erfahren</Button>
<Button variant="outline">Öffnungszeiten</Button>

// Auf DUNKLEN Section-Backgrounds:
<Button variant="outline-on-dark">044 853 22 74</Button>
```

Die Variant-Definition (siehe `code-standards.md`) nutzt nur Tailwind: `bg-primary text-primary-foreground hover:bg-primary/90 px-8 py-3 …`. Radius kommt aus `--radius` via `rounded-[var(--radius)]`.

### Button-Shape
Shape kommt aus `design.md`. Sie wird im Button-Component über Tailwind-Utilities und `--radius` gesetzt.

### Button-Regeln
- **Grosszügiges Padding** — primary `px-8 py-3`, nicht kleiner
- **Secondary MUSS sichtbar sein** — hat `border border-border` im Component
- **Auf dunklem Section-Background** IMMER `variant="outline-on-dark"` (NIE generisches `border-white/20`)
- **Icons** links vom Text, mit `gap-2`

### VERBOTEN
```tsx
<button className="bg-blue-600 text-white">…</button>         // hardcoded
<button className="bg-[#1E3A8A]">…</button>                    // hardcoded
<button style={{ backgroundColor: "#333" }}>…</button>         // inline style
<button className="px-3 py-1">…</button>                       // zu klein
```

---

## Typografie — via next/font + Tailwind-Utilities

### Fonts sind in `design.md` festgelegt
- `next/font/google` Loader in `app/layout.tsx`
- CSS-Var `--font-heading` / `--font-body` werden auf `<html>` gesetzt
- `@theme inline` in globals.css mapped sie zu Tailwind (`font-heading` / `font-body` Utility)
- `globals.css @layer base` setzt `body { font-family: var(--font-body) }` und `h1-h4 { font-family: var(--font-heading) }` — dadurch erben Headings automatisch

### Scale in der Component, nicht in globals.css
```tsx
<h1 className="text-4xl md:text-5xl lg:text-6xl font-extrabold tracking-tight">…</h1>
<h2 className="text-3xl md:text-4xl font-bold tracking-tight">…</h2>
<h3 className="text-xl md:text-2xl font-semibold">…</h3>
<p  className="text-base md:text-lg leading-relaxed text-muted-foreground">…</p>
```

Wenn `design.md` eine spezielle Letter-Spacing oder Line-Height vorschreibt die sich mit Tailwind nicht ausdrücken lässt → `@layer base` in globals.css für h1/h2 ergänzen. Nur soweit nötig.

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

## Spacing & Sections — Tailwind direkt

### Section-Spacing konsistent (Tailwind-Utilities, keine Custom-Class)
```tsx
<section className="py-16 md:py-24 lg:py-32">
  <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">…</div>
</section>
```

### Alternating Backgrounds
```tsx
<section className="py-16 md:py-24 lg:py-32 bg-secondary/30">
  <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">…</div>
</section>
```

### Full-Bleed Backgrounds
```tsx
<section className="bg-primary text-primary-foreground">
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
