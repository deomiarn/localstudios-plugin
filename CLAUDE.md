# LocalStudios Plugin

## HARD RULES — read before writing ANY code

### 1. DESIGN SOURCE — design.md ist Pflicht
Alle Farben, Fonts, Buttons, Spacing, Atmosphäre kommen aus **`design.md`** (Projekt-Root).
`design.md` wird in Phase 8 erzeugt (via `npx getdesign@latest add <brand>`) ODER vom User als existierende Datei übergeben.

**Nichts hardcoden. Niemals.** `globals.css` ist die 1:1-Umsetzung von `design.md`. Components lesen über CSS-Vars und Utility-Klassen.

### 2. CENTERING — every section, always
```tsx
// Jede Section entweder mit .section Utility …
<section className="section">
  {/* content */}
</section>

// … oder bei Full-Bleed Background mit innerem Container:
<section className="w-full bg-primary text-primary-foreground">
  <div className="py-16 md:py-24 lg:py-32 mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
    {/* content */}
  </div>
</section>
```
`.section` ist in `globals.css` definiert (`py-16 md:py-24 lg:py-32 mx-auto max-w-7xl px-4 sm:px-6 lg:px-8`).
Wenn Content links-aligned erscheint → Container-Wrapper fehlt. Fix it.

### 3. BUTTONS — via Utility-Klassen, auf Kontrast geprüft
```tsx
import { Button } from "@/components/ui/button"

// Primary auf hellen Sections
<Button variant="primary">Termin vereinbaren</Button>

// Secondary auf hellen Sections
<Button variant="secondary">Mehr erfahren</Button>

// Outline auf hellen Sections (text-foreground + border-primary)
<Button variant="outline">044 853 22 74</Button>

// Outline auf DUNKLEN Sections — eigene Variante, damit nicht unsichtbar
<Button variant="outline-on-dark">Öffnungszeiten</Button>
```

Die Klassen `.btn-primary` / `.btn-secondary` / `.btn-outline` / `.btn-outline-on-dark` sind in `globals.css` definiert und nutzen die CSS-Vars aus `design.md`.

**Für JEDEN Button prüfen: ist der Text gegen den Button-Background UND den Section-Background lesbar?**

### 4. NO PILL BADGES / LABELS
```tsx
// VERBOTEN:
<span className="rounded-full border px-3 py-1">Regensdorf</span>

// STATTDESSEN — clean text:
<p className="text-muted-foreground">Regensdorf · Steinmaur · Oberglatt</p>
```

### 5. COLORS — only CSS variables
```tsx
// NEVER: bg-blue-600, text-gray-500, bg-[#1E3A8A], style={{color:'#333'}}
// ALWAYS: bg-primary, text-muted-foreground, bg-accent, bg-primary/10
```

### 6. FONTS — aus design.md, via next/font
```tsx
// In app/layout.tsx — next/font lädt die Fonts aus design.md
// In Components nutze font-heading / font-body Utilities
<h1 className="font-heading">…</h1>
<p className="font-body text-muted-foreground">…</p>
```
Keine System-Defaults. Keine Inline-`font-family`.

### 7. IMAGES — mindestens eins pro Section, minimum 5 pro Variante
Jede Section bekommt ein Bild (Wärme/Leben). Wenn kein Bild verfügbar → `<ImagePlaceholder label="…" />` mit Gradient aus `--primary`/`--accent`.
Niemals leere Spaces.

### 8. CUSTOM COMPONENTS — keine fremden Block-Libraries
Pro Section ein eigenes File in `components/sections/variant-N/<section>.tsx`.
Semantisches HTML, Tailwind + Utility-Klassen, Content aus Phase 6 hart reingeschrieben.
Keine shadcn-Blocks. Keine shadcnblocks. Keine Tailwind-UI-Kopien.

---

## Project Documentation
- `docs/BUSINESS.md` — business facts + Design Source
- `docs/SEO-STRATEGY.md` — keywords
- `docs/pages/home.md` — homepage SEO
- `design.md` — colors, fonts, buttons, spacing, atmosphere (Single Source of Truth)
- `variant-blueprints.md` — Layout-Pläne pro Variante

## NAP Data
`lib/site-config.ts` — single source of truth.
