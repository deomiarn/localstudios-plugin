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
│   ├── page.tsx                  # Home — Variant 1
│   ├── variant-2/page.tsx        # Variant 2
│   ├── variant-3/page.tsx        # Variant 3
│   ├── globals.css               # ALL styles — single source of truth, abgeleitet aus design.md
│   └── <andere Seiten bei Extend>
├── components/
│   ├── ui/
│   │   ├── button.tsx            # Minimaler Wrapper um .btn-* Utility-Klassen
│   │   └── image-placeholder.tsx # Gradient-Placeholder für fehlende Bilder
│   ├── layout/
│   │   ├── header.tsx            # Shared
│   │   ├── footer.tsx            # Shared
│   │   └── variant-switcher.tsx  # Vergleichs-Nav (wird vor Delivery entfernt)
│   ├── sections/
│   │   ├── variant-1/            # Custom Components für Variante 1 (10 Files)
│   │   ├── variant-2/            # Custom Components für Variante 2
│   │   └── variant-3/            # Custom Components für Variante 3
│   └── seo/
│       └── schema-script.tsx     # JSON-LD Injection
├── lib/
│   ├── cn.ts                     # Minimaler className-Joiner (kein clsx/cva)
│   └── site-config.ts            # NAP, Phone, Email, Maps URL, Social
├── public/
│   └── images/
├── design.md                     # Single Source of Truth — aus Phase 8
└── variant-blueprints.md         # Layout-Pläne pro Variante — aus Phase 8
```

## Styling Rules — globals.css

### CRITICAL RULE
**ALL styles live in `app/globals.css`. No exceptions.** Alle Tokens stammen aus `design.md`.

- No `style={}` props on elements
- No `styled-components` / CSS-in-JS / `.module.css`
- Tailwind utility classes in JSX sind OK (kompilieren aus globals.css)
- Custom Styles kommen als Utility-Klassen oder `@layer base` in globals.css

### globals.css Skelett

```css
@import "tailwindcss";

@theme {
  /* Farben aus design.md — als HSL */
  --color-background: hsl(<aus design.md>);
  --color-foreground: hsl(<aus design.md>);
  --color-primary: hsl(<aus design.md>);
  --color-primary-foreground: hsl(<aus design.md>);
  --color-accent: hsl(<aus design.md>);
  --color-accent-foreground: hsl(<aus design.md>);
  --color-muted: hsl(<aus design.md>);
  --color-muted-foreground: hsl(<aus design.md>);
  --color-border: hsl(<aus design.md>);
  --color-ring: hsl(<aus design.md>);

  /* Fonts (gesetzt durch next/font im layout.tsx) */
  --font-heading: var(--font-heading);
  --font-body: var(--font-body);

  /* Radius */
  --radius: <aus design.md>;
  --radius-btn: <aus design.md>;
}

@layer base {
  body { @apply bg-background text-foreground font-body antialiased; }
  h1 { @apply <aus design.md>; }
  h2 { @apply <aus design.md>; }
  h3 { @apply <aus design.md>; }
  p  { @apply <aus design.md>; }
}

@layer components {
  .section      { @apply py-16 md:py-24 lg:py-32 mx-auto max-w-7xl px-4 sm:px-6 lg:px-8; }
  .section-full { @apply py-16 md:py-24 lg:py-32 w-full; }

  .btn-primary         { @apply bg-primary text-primary-foreground font-semibold px-8 py-3 text-base md:text-lg
                                hover:bg-primary/90 transition-all duration-200 shadow-md hover:shadow-lg;
                         border-radius: var(--radius-btn); }
  .btn-secondary       { @apply bg-secondary text-secondary-foreground font-medium px-6 py-2.5
                                border-2 border-border hover:bg-accent hover:text-accent-foreground
                                transition-all duration-200;
                         border-radius: var(--radius-btn); }
  .btn-outline         { @apply bg-transparent text-foreground font-medium px-6 py-2.5
                                border-2 border-primary hover:bg-primary hover:text-primary-foreground
                                transition-all duration-200;
                         border-radius: var(--radius-btn); }
  .btn-outline-on-dark { @apply bg-transparent text-white border-2 border-white px-6 py-2.5
                                hover:bg-white hover:text-primary transition-all duration-200;
                         border-radius: var(--radius-btn); }
}
```

### Before Any Style Change
1. READ `design.md` — der Ziel-Wert muss hier stehen
2. READ `globals.css` — Token/Utility evtl. schon vorhanden
3. Update `globals.css` (Token ergänzen / Utility anpassen)
4. NIEMALS im Component-File hardcoded Farben oder Fonts setzen

## Component Rules

### UI Primitives (components/ui/)

Sehr klein. Nur Button + ImagePlaceholder. Keine weiteren UI-Bibliotheken.

```tsx
// components/ui/button.tsx
import { ComponentPropsWithoutRef } from "react"
import { cn } from "@/lib/cn"

type Variant = "primary" | "secondary" | "outline" | "outline-on-dark"

interface ButtonProps extends ComponentPropsWithoutRef<"button"> {
  variant?: Variant
}

export function Button({ variant = "primary", className, ...rest }: ButtonProps) {
  const cls = {
    primary: "btn-primary",
    secondary: "btn-secondary",
    outline: "btn-outline",
    "outline-on-dark": "btn-outline-on-dark",
  }[variant]
  return <button className={cn(cls, className)} {...rest} />
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
// components/sections/variant-1/hero.tsx
import Image from "next/image"
import { Button } from "@/components/ui/button"
import { ImagePlaceholder } from "@/components/ui/image-placeholder"
import { siteConfig } from "@/lib/site-config"

export function Hero() {
  return (
    <section className="section grid items-center gap-12 lg:grid-cols-[3fr_2fr]">
      <div>
        <h1 className="font-heading">Zahnarzt Zürich — <span className="text-primary">moderne Praxis</span></h1>
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
      <div>
        <ImagePlaceholder label="Praxis-Aussenansicht" className="aspect-[4/5]" />
      </div>
    </section>
  )
}
```

Regeln:
- `<section>` (oder `<header>` für Page-Top) + `<h1/h2/h3>` + `<p>` + `<ul>` — semantic
- `.section` Utility für Zentrierung + Padding
- Keine hardcoded Farben, kein `style={}`
- Bilder: `next/image` ODER `<ImagePlaceholder>`
- Buttons via `<Button variant="…" />`
- Content hart reingeschrieben (identisch über Varianten, Layout unterscheidet sich)

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
- Focus ring sichtbar auf allen interaktiven Elementen (via `.btn-*` Utilities / Default)
- Color contrast ≥ 4.5:1 (Playwright-QA checkt das in Phase 10)
- Semantic HTML: `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`
- `aria-label` auf Nav-Landmarks und Icon-only Buttons

## Performance

- `next/image` mit `width`, `height`, `priority` für above-fold
- `loading="lazy"` auto für below-fold
- `next/font` für Google Fonts (keine externen Requests)
- Dynamic imports für schwere below-fold Sections
- Keine unused dependencies — Projekt bleibt schlank (kein shadcn, kein blocks-Paket)
