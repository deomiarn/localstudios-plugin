# LocalStudios Plugin

## HARD RULES — read before writing ANY code

### 1. DESIGN SOURCE — design.md + frontend-design Skill sind Pflicht
Alle Farben, Fonts, Buttons, Spacing, Atmosphäre kommen aus **`design.md`** (Projekt-Root).
`design.md` wird in Phase 8 einmal beschafft (via `npx getdesign@latest add <brand>`) ODER vom User als existierende Datei übergeben.

**Vor jedem Design-Planen oder Bauen MUSS der `frontend-design` Skill aus diesem Plugin geladen sein.** Der Skill bringt die Design-Qualitäts-Prinzipien (Direction committen, intentional Composition, Typography-Handwerk, Anti-Patterns, Quality Gate). Ohne den Skill + `design.md` zusammen wird weder geplant noch gebaut.

**design.md ist READ-ONLY.** Niemals editieren, niemals ergänzen, niemals umformulieren. Einzige Ausnahme: ein vom User explizit am Anfang gewünschter Farbwechsel (z.B. „primary von gelb auf blau") — dann NUR die Farb-Tokens ersetzen, alles andere bleibt 1:1.

Claude liest `design.md`, versteht sie (Gefühl, Typografie, Button-Stil, Atmosphäre, Anti-Muster, Bild-Regeln) und baut sie in `globals.css` + Tailwind-Utilities um. Der Inhalt der `design.md` ist **Wahrheit und Orientierung für jede Design-Entscheidung**.

Wenn das Ergebnis später nicht passt (Phase 10) → der **Code** wird angepasst, niemals `design.md`.

**Nichts hardcoden. Niemals.** `globals.css` bleibt schlank: nur CSS-Vars aus `design.md` + Tailwind v4 `@theme` Mapping + minimale Base-Styles. Alles andere als Tailwind-Utilities direkt in den Components.

### 2. CENTERING — every section, always (Tailwind direkt, keine .section-Class)
```tsx
<section className="py-16 md:py-24 lg:py-32">
  <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
    {/* content */}
  </div>
</section>

// Full-Bleed Background:
<section className="bg-primary text-primary-foreground">
  <div className="py-16 md:py-24 lg:py-32 mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
    {/* content */}
  </div>
</section>
```
Wenn Content links-aligned erscheint → Container-Wrapper fehlt. Fix it.

### 3. BUTTONS — Tailwind komponiert im Button-Component
```tsx
import { Button } from "@/components/ui/button"

<Button variant="primary">Termin vereinbaren</Button>
<Button variant="secondary">Mehr erfahren</Button>
<Button variant="outline">044 853 22 74</Button>

// Outline auf DUNKLEN Sections — eigene Variante, damit nicht unsichtbar
<Button variant="outline-on-dark">Öffnungszeiten</Button>
```

Die Varianten sind im Button-Component als Tailwind-Utility-Strings definiert (`bg-primary text-primary-foreground hover:bg-primary/90 px-8 py-3 …`). Keine `.btn-*` Custom-Classes in `globals.css`.

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

### 7. IMAGES — Hero PFLICHT above-the-fold, min 5 auf Homepage
Jede Section bekommt ein Bild (Wärme/Leben). Wenn kein Bild verfügbar → `<ImagePlaceholder label="…" />` mit Gradient aus `--primary`/`--accent`.

**Der Hero MUSS ein Bild above-the-fold sichtbar haben** — Split-Layout oder Full-Bleed-Background mit Overlay-Text. Text-only Hero ist verboten. Hero-Bild-Proportion: `aspect-[4/5]` oder `aspect-[3/4]`, niemals `aspect-video`.

**Keine `[Image #N]`, `[IMG]`, `[Foto]` Text-Platzhalter im JSX-Content.** Image-Metadaten gehören in `docs/pages/home.md`.

Niemals leere Spaces.

### 8. CUSTOM COMPONENTS — keine fremden Block-Libraries, flache Struktur
Pro Section ein eigenes File in `components/sections/<section>.tsx` (flach, kein `variant-N/`).
Semantisches HTML, Tailwind + Utility-Klassen, Content aus Phase 6 hart reingeschrieben.
Keine shadcn-Blocks. Keine shadcnblocks. Keine Tailwind-UI-Kopien. Keine Varianten.

---

## Project Documentation
- `docs/BUSINESS.md` — business facts + Design Source
- `docs/SEO-STRATEGY.md` — keywords
- `docs/pages/home.md` — homepage SEO + Image-Source-Tracking pro Section
- `design.md` — colors, fonts, buttons, spacing, atmosphere (**READ-ONLY Source of Truth**)
- `layout-plan.md` — Layout-Plan für die Homepage (Phase 8)

## NAP Data
`lib/site-config.ts` — single source of truth.
