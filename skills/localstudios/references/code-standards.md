# Code Standards — Next.js + Tailwind + Custom Components

All generated websites use **Next.js App Router** with **Tailwind CSS** and **custom components**.
All styling lives in **globals.css** — never inline styles. All Tokens come from **`design.md`**.

No fremde Block-Libraries. No shadcn/ui. No shadcnblocks. Components sind custom, semantisch, in eigenen Files.

---

## Project Initialization

```bash
npx create-next-app@latest . --typescript --tailwind --app --no-src-dir --import-alias "@/*" --eslint --use-npm
```

Ergebnis: Next.js + Tailwind CSS. Keine weiteren UI-Libraries installieren.

## File Structure

```
project/
├── app/
│   ├── layout.tsx                # Root layout (metadata, fonts, header/footer, schema)
│   ├── page.tsx                  # EINZIGE Homepage
│   ├── globals.css               # CSS-Vars aus design.md + Tailwind v4 @theme inline
│   └── <andere Seiten bei Extend>
├── components/
│   ├── ui/
│   │   ├── button.tsx            # Komponiert Tailwind-Utilities per Variant
│   │   └── image-placeholder.tsx # Gradient-Placeholder für fehlende Bilder
│   ├── layout/
│   │   ├── header.tsx
│   │   └── footer.tsx
│   ├── sections/                 # FLACH — 10 Section-Files, keine variant-N Ordner
│   │   ├── hero.tsx
│   │   ├── trust-bar.tsx
│   │   ├── featured-service-1.tsx
│   │   ├── featured-service-2.tsx
│   │   ├── services-grid.tsx
│   │   ├── about-teaser.tsx
│   │   ├── social-proof.tsx
│   │   ├── local-area.tsx
│   │   ├── cta-section.tsx
│   │   └── faq.tsx               # optional
│   └── seo/
│       └── schema-script.tsx     # JSON-LD Injection
├── lib/
│   ├── cn.ts                     # Minimaler className-Joiner (kein clsx/cva)
│   └── site-config.ts            # NAP, Phone, Email, Maps URL, Social
├── public/
│   └── images/
├── design.md                     # READ-ONLY Source of Truth — aus Phase 8
└── layout-plan.md                # Layout-Plan für die Homepage — aus Phase 8
```

## Styling Rules — schlank, Tailwind-first

### CRITICAL RULE
**`globals.css` bleibt klein — nur Tokens + minimale Base-Styles. Alles andere in Components via Tailwind-Utilities.** Alle Tokens stammen aus `design.md`.

- No `style={}` props on elements
- No `styled-components` / CSS-in-JS / `.module.css`
- **Default: Tailwind-Utilities in JSX** (`bg-primary`, `text-muted-foreground`, `py-16 md:py-24`)
- **Keine hardcoded Farben** in Components (kein `bg-blue-600`, kein `#1E3A8A`)
- Custom Utility-Klassen in globals.css **nur** wenn `design.md` eine spezifische Atmosphäre vorschreibt die sich mit purem Tailwind unschön wiederholt (z.B. Grain-Overlay, spezifische Glass-Morphism Regel)

### globals.css Skelett (shadcn-Pattern, Tailwind v4)

```css
@import "tailwindcss";

/* 1) Tokens aus design.md als CSS-Vars */
@layer base {
  :root {
    --background: <H S L aus design.md>;
    --foreground: <H S L>;
    --primary: <H S L>;
    --primary-foreground: <H S L>;
    --secondary: <H S L>;
    --secondary-foreground: <H S L>;
    --accent: <H S L>;
    --accent-foreground: <H S L>;
    --muted: <H S L>;
    --muted-foreground: <H S L>;
    --border: <H S L>;
    --ring: <H S L>;

    --radius: <aus design.md>;
  }

  * { border-color: hsl(var(--border)); }
  body { @apply bg-background text-foreground antialiased; font-family: var(--font-body); }
  h1, h2, h3, h4 { font-family: var(--font-heading); }
}

/* 2) Tailwind v4 mapping — aktiviert bg-primary / text-accent / border-border etc. */
@theme inline {
  --color-background: hsl(var(--background));
  --color-foreground: hsl(var(--foreground));
  --color-primary: hsl(var(--primary));
  --color-primary-foreground: hsl(var(--primary-foreground));
  --color-secondary: hsl(var(--secondary));
  --color-secondary-foreground: hsl(var(--secondary-foreground));
  --color-accent: hsl(var(--accent));
  --color-accent-foreground: hsl(var(--accent-foreground));
  --color-muted: hsl(var(--muted));
  --color-muted-foreground: hsl(var(--muted-foreground));
  --color-border: hsl(var(--border));
  --color-ring: hsl(var(--ring));

  --radius-sm: calc(var(--radius) - 4px);
  --radius-md: calc(var(--radius) - 2px);
  --radius-lg: var(--radius);
  --radius-xl: calc(var(--radius) + 4px);

  --font-heading: var(--font-heading);
  --font-body: var(--font-body);
}

/* 3) Nur falls design.md es wirklich verlangt — minimale Utilities */
@layer utilities {
  /* Beispiel: Grain-Overlay für ein bestimmtes Design */
  /* .bg-grain { background-image: url("/grain.svg"); } */
}
```

### Before Any Style Change
1. READ `design.md` — gibt es einen Token dafür?
2. Kann man es mit Tailwind-Utilities ausdrücken? → in die Component, **nicht** in globals.css
3. Muss es wirklich eine Class sein (z.B. Grain-Overlay, Spezial-Gradient)? → schmale Utility in `@layer utilities` + in `design.md` dokumentieren
4. NIEMALS im Component-File hardcoded Farben oder Fonts setzen

## Component Rules

### UI Primitives (components/ui/)

Sehr klein. Nur Button + ImagePlaceholder. Keine weiteren UI-Bibliotheken.

```tsx
// components/ui/button.tsx — komponiert Tailwind-Utilities, nutzt CSS-Vars aus globals.css
import { ComponentPropsWithoutRef } from "react"
import { cn } from "@/lib/cn"

type Variant = "primary" | "secondary" | "outline" | "outline-on-dark"

interface ButtonProps extends ComponentPropsWithoutRef<"button"> {
  variant?: Variant
}

const base = "inline-flex items-center justify-center rounded-[var(--radius)] font-semibold transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none"

const variants: Record<Variant, string> = {
  primary:           "bg-primary text-primary-foreground hover:bg-primary/90 px-8 py-3 text-base md:text-lg shadow-md hover:shadow-lg",
  secondary:         "bg-secondary text-secondary-foreground hover:bg-secondary/80 border border-border px-6 py-2.5",
  outline:           "bg-transparent text-foreground border-2 border-primary hover:bg-primary hover:text-primary-foreground px-6 py-2.5",
  "outline-on-dark": "bg-transparent text-white border-2 border-white hover:bg-white hover:text-primary px-6 py-2.5",
}

export function Button({ variant = "primary", className, ...rest }: ButtonProps) {
  return <button className={cn(base, variants[variant], className)} {...rest} />
}
```

```tsx
// components/ui/image-placeholder.tsx
export function ImagePlaceholder({ label, className = "" }: { label: string; className?: string }) {
  return (
    <div className={`aspect-video rounded-[var(--radius)] bg-gradient-to-br from-primary/20 to-accent/20 flex items-center justify-center ${className}`}>
      <span className="text-muted-foreground text-sm">{label}</span>
    </div>
  )
}
```

```ts
// lib/cn.ts
export const cn = (...parts: (string | undefined | false | null)[]) => parts.filter(Boolean).join(" ")
```

### Section Components (components/sections/variant-N/)

**Pro Section ein eigenes File.** Semantisches HTML. Content aus Phase 6 hart reingeschrieben.

```tsx
// components/sections/hero.tsx — reine Tailwind-Utilities, Bild above-the-fold
import Image from "next/image"
import { Button } from "@/components/ui/button"
import { ImagePlaceholder } from "@/components/ui/image-placeholder"
import { siteConfig } from "@/lib/site-config"

export function Hero() {
  return (
    <section className="py-16 md:py-24 lg:py-32">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 grid items-center gap-12 lg:grid-cols-[3fr_2fr]">
        <div>
          <h1 className="text-4xl md:text-5xl lg:text-6xl font-extrabold tracking-tight">
            Zahnarzt Zürich — <span className="text-primary">moderne Praxis</span>
          </h1>
          <p className="mt-6 max-w-xl text-lg text-muted-foreground">
            Seit 2008 im Herzen von Zürich-Seefeld. Sanfte Behandlung, moderne Technik, persönliche Betreuung.
          </p>
          <div className="mt-8 flex flex-wrap gap-4">
            <Button variant="primary">Termin vereinbaren</Button>
            <a href={`tel:${siteConfig.nap.phone}`}>
              <Button variant="outline">{siteConfig.nap.phone}</Button>
            </a>
          </div>
        </div>
        <ImagePlaceholder label="Praxis-Aussenansicht" className="aspect-[4/5]" />
      </div>
    </section>
  )
}
```

Regeln:
- Semantisches HTML: `<section>`, `<header>`, `<h1>/<h2>/<h3>`, `<p>`, `<ul>` — nicht Div-Suppe
- Zentrierung via Tailwind: äusseres `<section>` mit `py-16 md:py-24 lg:py-32`, inneres `<div>` mit `mx-auto max-w-7xl px-4 sm:px-6 lg:px-8`
- Farben nur über CSS-Var-gemappte Tailwind-Utilities (`bg-primary`, `text-muted-foreground`, `border-border`)
- Keine hardcoded Farben, kein `style={}`, keine Custom-CSS-Klassen in Components
- Bilder: `next/image` ODER `<ImagePlaceholder>`
- Buttons via `<Button variant="…" />`
- Content hart reingeschrieben — Phase 6 Content landet 1:1 im JSX. Keine `[Image …]`-Text-Platzhalter.
- **Hero MUSS ein Bild above-the-fold haben** (Split oder Full-Bleed), `aspect-[4/5]`/`aspect-[3/4]`, nie `aspect-video`

## SEO in Next.js

### Metadata API (per page / layout)
```tsx
import type { Metadata } from "next"

export const metadata: Metadata = {
  title: "Primary KW — Company — City",
  description: "Benefit + CTA + Location (max 155 chars)",
  openGraph: {
    title: "OG Title",
    description: "OG Description",
    type: "website",
  },
}
```

### Schema Injection
```tsx
// components/seo/schema-script.tsx
import { siteConfig } from "@/lib/site-config"

export function SchemaScript() {
  const schema = {
    "@context": "https://schema.org",
    "@type": "<industry-specific z.B. Dentist>",
    name: siteConfig.name,
    // … komplett aus Phase 7
  }
  return (
    <script
      type="application/ld+json"
      dangerouslySetInnerHTML={{ __html: JSON.stringify(schema) }}
    />
  )
}
```

## Site Configuration

Zentral in `lib/site-config.ts`:

```tsx
export const siteConfig = {
  name: "Company Name",
  description: "Primary keyword description",
  url: "https://domain.com",
  nap: {
    legalName: "Legal Business Name",
    street: "Street 123",
    postalCode: "8000",
    city: "Zurich",
    region: "ZH",
    country: "CH",
    phone: "+41 44 123 45 67",
    email: "info@domain.com",
  },
  openingHours: "Mo-Fr 08:00-18:00",
  mapsEmbedUrl: "<vom User in Phase 2>",
  social: {
    google: "https://g.page/…",
    instagram: "https://instagram.com/…",
  },
} as const
```

All Components lesen aus `siteConfig` — NAP niemals mehrfach hardcoden.

## Accessibility

- All images: `alt` mit keyword-relevanter Beschreibung
- `next/image` für alle Bilder (auto optimization)
- Form inputs: `<label htmlFor>` + passende `<input id>`
- Buttons: beschreibender Text, nicht nur Icons
- Focus ring sichtbar auf allen interaktiven Elementen (Button-Component hat `focus-visible:ring-2 focus-visible:ring-ring`)
- Color contrast ≥ 4.5:1 (Playwright-QA checkt das in Phase 10)
- Semantic HTML: `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`
- `aria-label` auf Nav-Landmarks und Icon-only Buttons

## Performance

- `next/image` mit `width`, `height`, `priority` für above-fold
- `loading="lazy"` auto für below-fold
- `next/font` für Google Fonts (keine externen Requests)
- Dynamic imports für schwere below-fold Sections
- Keine unused dependencies — Projekt bleibt schlank (kein shadcn, kein blocks-Paket)
