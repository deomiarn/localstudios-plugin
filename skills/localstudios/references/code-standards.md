# Code Standards — Next.js + shadcn/ui

All generated websites use **Next.js App Router** with **shadcn/ui** components.
All styling lives in **globals.css** — never inline styles.

---

## Project Initialization

```bash
npx shadcn@latest init --preset b0 --template next
```

This creates a Next.js project with shadcn/ui pre-configured (Tailwind CSS v4, CSS variables, dark mode support).

## File Structure

```
project/
├── app/
│   ├── layout.tsx                # Root layout (metadata, fonts, global providers)
│   ├── page.tsx                  # Home page
│   ├── globals.css               # ALL styles — single source of truth
│   ├── about/
│   │   └── page.tsx
│   ├── services/
│   │   ├── page.tsx              # Services overview (optional)
│   │   └── [service]/
│   │       └── page.tsx          # Individual service page
│   └── contact/
│       └── page.tsx
├── components/
│   ├── ui/                       # shadcn/ui components (auto-generated)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── accordion.tsx
│   │   └── ...
│   ├── layout/
│   │   ├── header.tsx            # Site header with navigation
│   │   ├── footer.tsx            # Footer with NAP data
│   │   └── mobile-nav.tsx        # Mobile navigation drawer
│   ├── sections/
│   │   ├── hero.tsx              # Hero section with CTA
│   │   ├── trust-bar.tsx         # USP numbers/badges
│   │   ├── services-grid.tsx     # Service cards overview
│   │   ├── testimonials.tsx      # Social proof section
│   │   ├── local-trust.tsx       # Map + local signals
│   │   ├── cta-section.tsx       # Reusable CTA block
│   │   ├── faq-section.tsx       # FAQ accordion (uses shadcn Accordion)
│   │   └── contact-form.tsx      # Contact form (uses shadcn Input, Button)
│   └── seo/
│       ├── schema-script.tsx     # JSON-LD schema injection
│       └── breadcrumbs.tsx       # Breadcrumb navigation
├── lib/
│   ├── utils.ts                  # shadcn cn() utility
│   ├── site-config.ts            # Company name, NAP, social links
│   └── keywords.ts               # Keyword mappings per page
├── public/
│   ├── images/                   # Optimized images
│   └── fonts/                    # Local font files if needed
└── schema/
    ├── localbusiness.json        # LocalBusiness JSON-LD
    ├── organization.json         # Organization schema
    └── [page]-schema.json        # Per-page schema
```

## Styling Rules — globals.css

### CRITICAL RULE
**ALL styles live in `app/globals.css`. No exceptions.**

- No `style={}` props on elements
- No `styled-components` or CSS-in-JS
- No separate `.module.css` files
- Tailwind utility classes in JSX are OK (they compile from globals.css)
- Custom styles MUST be added to globals.css as utility classes or component styles

### globals.css Structure

```css
@import "tailwindcss";
@import "tw-animate-css";

@custom-variant dark (&:is(.dark *));

/* ===== DESIGN TOKENS ===== */
:root {
  /* Brand Colors — set from design system */
  --primary: ...;
  --secondary: ...;
  --accent: ...;
  --background: ...;
  --foreground: ...;

  /* Spacing scale */
  --section-padding: 4rem 1rem;
  --section-padding-lg: 6rem 2rem;

  /* Component tokens */
  --header-height: 4rem;
  --container-max: 1200px;
  --radius: 0.625rem;
}

/* ===== BASE STYLES ===== */
body {
  font-family: var(--font-body);
}

h1, h2, h3 {
  font-family: var(--font-heading);
}

/* ===== SECTION STYLES ===== */
.section { ... }
.section-hero { ... }
.section-trust { ... }
.section-services { ... }
.section-testimonials { ... }
.section-cta { ... }

/* ===== COMPONENT OVERRIDES ===== */
/* Override shadcn defaults here, not in component files */

/* ===== RESPONSIVE ===== */
@media (min-width: 768px) { ... }
@media (min-width: 1024px) { ... }
@media (min-width: 1440px) { ... }
```

### Before Any Style Change
1. READ `globals.css` first
2. Check if a relevant style/token already exists
3. Extend or modify in globals.css
4. Never create new CSS files

## Component Rules

### shadcn/ui Components
Install via shadcn MCP or CLI:
```bash
npx shadcn@latest add button card accordion input textarea
```

Use shadcn components as building blocks:
- `Button` for all CTAs
- `Card` for service cards, testimonial cards
- `Accordion` for FAQ sections
- `Input` + `Textarea` for contact form
- `NavigationMenu` for header navigation

### Custom Components
Each section = one component file in `components/sections/`:
- Accept content as props (text, keywords, links)
- Use shadcn primitives internally
- Style via Tailwind classes + globals.css custom classes
- No inline styles ever

### Component Template
```tsx
import { Button } from "@/components/ui/button"

interface HeroProps {
  headline: string
  subheadline: string
  ctaText: string
  ctaHref: string
}

export function Hero({ headline, subheadline, ctaText, ctaHref }: HeroProps) {
  return (
    <section className="section-hero">
      <div className="container mx-auto max-w-[var(--container-max)]">
        <h1 className="text-4xl font-bold tracking-tight md:text-5xl lg:text-6xl">
          {headline}
        </h1>
        <p className="mt-4 text-lg text-muted-foreground md:text-xl">
          {subheadline}
        </p>
        <Button size="lg" className="mt-8" asChild>
          <a href={ctaHref}>{ctaText}</a>
        </Button>
      </div>
    </section>
  )
}
```

## SEO in Next.js

### Metadata API (per page)
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
interface SchemaScriptProps {
  schema: Record<string, unknown>
}

export function SchemaScript({ schema }: SchemaScriptProps) {
  return (
    <script
      type="application/ld+json"
      dangerouslySetInnerHTML={{ __html: JSON.stringify(schema) }}
    />
  )
}
```

Use in layout or page:
```tsx
import { SchemaScript } from "@/components/seo/schema-script"
import localBusiness from "@/schema/localbusiness.json"

export default function RootLayout({ children }) {
  return (
    <html lang="de">
      <body>
        <SchemaScript schema={localBusiness} />
        <Header />
        <main>{children}</main>
        <Footer />
      </body>
    </html>
  )
}
```

## Site Configuration

Centralize all business data in `lib/site-config.ts`:

```tsx
export const siteConfig = {
  name: "Company Name",
  description: "Primary keyword description",
  url: "https://domain.com",
  nap: {
    name: "Legal Business Name",
    street: "Street 123",
    postalCode: "8000",
    city: "Zurich",
    region: "ZH",
    country: "CH",
    phone: "+41 44 123 45 67",
    email: "info@domain.com",
  },
  openingHours: "Mo-Fr 08:00-18:00",
  social: {
    google: "https://g.page/...",
    instagram: "https://instagram.com/...",
  },
}
```

All components read from `siteConfig` — NAP is never hardcoded in multiple places.

## Accessibility

- All images: `alt` prop with keyword-relevant description
- `next/image` for all images (automatic optimization)
- Form inputs: `Label` component with `htmlFor`
- Buttons: descriptive text, not just icons
- Focus ring visible on all interactive elements (shadcn default)
- Color contrast ≥ 4.5:1
- Semantic HTML: `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`
- `aria-label` on nav landmarks

## Performance

- `next/image` with `width`, `height`, `priority` for above-fold images
- `loading="lazy"` handled automatically by Next.js for below-fold
- `next/font` for Google Fonts (no external requests)
- Dynamic imports for below-fold sections if heavy
- No unused dependencies
