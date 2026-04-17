# Phase 9 — Build Homepage Variants (Custom Components)

## References
- Load `./references/code-standards.md`
- Load `./references/ui-rules.md`
- Load `design.md` (Projekt-Root — aus Phase 8)
- Load `variant-blueprints.md` (Projekt-Root — aus Phase 8)

## Architecture

```
design.md              → Single Source of Truth (Farben, Fonts, Buttons, Spacing, Atmosphäre, Anti-Muster)
variant-blueprints.md  → N Layout-Strategien pro Section (Blueprint — kein Code)
globals.css            → Ableitung 1:1 aus design.md (CSS Vars, Utility-Klassen, Typografie)
components/sections/…  → Custom Components pro Section pro Variante (semantisches HTML + Tailwind)
ui-rules.md            → Harte Regeln (keine hardcoded Farben, zentriert, Button-Kontrast, …)
```

## CRITICAL: design.md + variant-blueprints.md sind das Rezept

Lies beide Dateien. Weiche NICHT davon ab.
- `design.md` gibt dir WAS du verwendest (Tokens).
- `variant-blueprints.md` gibt dir WIE du es pro Variante anwendest (Layout/Komposition).
- Keine fremden Block-Libraries. Keine vorgefertigten Template-Components.

---

## Build Process

### Step 1 — Next.js initialisieren

```bash
npx create-next-app@latest . --typescript --tailwind --app --no-src-dir --import-alias "@/*" --eslint --use-npm
```

Kein `shadcn init`. Kein Registry-Setup. Keine Block-Library.

### Step 2 — globals.css aus design.md generieren (MINIMAL)

`app/globals.css` bleibt **schlank**. Der Job ist:
- Farb-Tokens als CSS-Vars definieren (shadcn-Stil, HSL)
- Tailwind v4 `@theme` mapping damit `bg-primary` / `text-accent` / `border-border` funktionieren
- Fonts via `next/font` an CSS-Vars binden
- Radius als Token

**Alles andere passiert in den Components via Tailwind-Utility-Klassen.** Kein `.section`, kein `.btn-*` by default. Nur wenn `design.md` eine wirklich wiederkehrende Atmosphäre-Eigenart beschreibt die sich mit purem Tailwind unschön wiederholt → dann eine schmale Custom-Class ergänzen (z.B. `.ambient-grain` für Grain-Overlay) und in `design.md` dokumentieren.

Skelett (shadcn-Pattern, Tailwind v4):

```css
@import "tailwindcss";

/* 1) CSS-Vars aus design.md */
@layer base {
  :root {
    --background: <H S L aus design.md>;          /* z.B. 0 0% 100% */
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

    --radius: <aus design.md>;                    /* z.B. 0.5rem */
  }

  * { border-color: hsl(var(--border)); }
  body { @apply bg-background text-foreground antialiased; font-family: var(--font-body); }
  h1, h2, h3, h4 { font-family: var(--font-heading); }
}

/* 2) Tailwind v4 mapping — damit bg-primary etc. funktioniert */
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
```

Wenn `design.md` spezielle Atmosphäre-Eigenarten definiert die sich mit reinem Tailwind schlecht ausdrücken lassen, schreibe kleine Utility-Klassen drunter — **nur so viele wie nötig**. Beispiel:

```css
@layer utilities {
  /* Nur wenn design.md einen Grain-Overlay vorschreibt: */
  .bg-grain { background-image: url("/grain.svg"); background-blend-mode: overlay; }
}
```

**Regel:** Wenn etwas in Tailwind-Utilities ausdrückbar ist → in die Component. Nicht in globals.css.

**globals.css ist identisch für alle Varianten.** Wenn `design.md` einen Token hat den globals.css nicht mapped, ergänzen. Wenn globals.css einen Token nutzt der in `design.md` fehlt, `design.md` updaten (nicht umgekehrt erfinden).

### Step 3 — Fonts via next/font

In `app/layout.tsx`:

```tsx
import { <HeadingFont aus design.md>, <BodyFont aus design.md> } from "next/font/google"

const heading = <HeadingFont>({ subsets: ["latin"], variable: "--font-heading", display: "swap" })
const body    = <BodyFont>({ subsets: ["latin"], variable: "--font-body",    display: "swap" })

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="de" className={`${heading.variable} ${body.variable}`}>
      <body>{children}</body>
    </html>
  )
}
```

In globals.css via `@theme` an `--font-heading` / `--font-body` gebunden → Tailwind nutzt `font-heading` / `font-body` Utilities automatisch.

### Step 4 — lib/site-config.ts

NAP aus `docs/BUSINESS.md`:

```ts
export const siteConfig = {
  name: "<Company>",
  description: "<Primary KW + Benefit + City>",
  url: "https://<domain>",
  nap: {
    street: "…", postalCode: "…", city: "…", region: "…", country: "CH",
    phone: "…", email: "…",
  },
  hours: { /* … */ },
  mapsEmbedUrl: "<vom User>",
  social: { /* … */ },
} as const
```

---

### Step 5 — Dateistruktur

```
app/
  page.tsx                      → Variant 1 (default)
  variant-2/page.tsx            → Variant 2
  variant-3/page.tsx            → Variant 3
  layout.tsx                    → Shared (Header, Footer, Schema, Fonts)
  globals.css                   → aus design.md
components/
  sections/
    variant-1/                  → Custom Components für Variante 1
      hero.tsx
      trust-bar.tsx
      featured-service-1.tsx
      featured-service-2.tsx
      services-grid.tsx
      about-teaser.tsx
      social-proof.tsx
      local-area.tsx
      cta-section.tsx
    variant-2/                  → Custom Components für Variante 2
      …
    variant-3/
      …
  layout/
    header.tsx                  → Shared
    footer.tsx                  → Shared
    variant-switcher.tsx        → Vergleichs-Nav
  ui/
    button.tsx                  → komponiert Tailwind-Utilities per Variant, nutzt CSS-Vars aus globals.css
    image-placeholder.tsx       → Gradient-Placeholder für fehlende Bilder
  seo/
    schema-script.tsx           → Shared
lib/
  site-config.ts
design.md
variant-blueprints.md
```

**Shared über Varianten:** layout.tsx, header, footer, schema, globals.css, site-config, ui/*
**Verschieden pro Variante:** Section-Components, page.tsx Assembly

---

### Step 6 — Minimal UI Primitives

`components/ui/button.tsx` — komponiert **Tailwind-Utilities direkt**, nutzt die CSS-Vars aus `globals.css` (keine `.btn-*` Custom-Classes):

```tsx
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

Die Button-Styles leben im Component-Code, nicht in `globals.css`. Shape/Padding anpassen wenn `design.md` das vorgibt — aber über Tailwind-Utilities im Component.

`components/ui/image-placeholder.tsx` — auch reines Tailwind:

```tsx
export function ImagePlaceholder({ label, className = "" }: { label: string; className?: string }) {
  return (
    <div className={`aspect-video rounded-[var(--radius)] bg-gradient-to-br from-primary/20 to-accent/20 flex items-center justify-center ${className}`}>
      <span className="text-muted-foreground text-sm">{label}</span>
    </div>
  )
}
```

`lib/cn.ts`:

```ts
export const cn = (...parts: (string | undefined | false | null)[]) => parts.filter(Boolean).join(" ")
```

Keine weiteren UI-Libraries. Cards, Accordions, etc. als Custom Component in der jeweiligen Section inline geschrieben — mit Tailwind-Utilities und CSS-Vars.

---

### Step 7 — Variant Switcher (Vergleichs-Tool)

`components/layout/variant-switcher.tsx`:

```tsx
"use client"
import Link from "next/link"
import { usePathname } from "next/navigation"

export function VariantSwitcher({ variants }: { variants: { href: string; label: string; name: string }[] }) {
  const pathname = usePathname()
  return (
    <div className="fixed bottom-4 left-1/2 -translate-x-1/2 z-50 bg-background/80 backdrop-blur border border-border rounded-full px-4 py-2 flex gap-3 shadow-lg">
      {variants.map((v) => (
        <Link
          key={v.href}
          href={v.href}
          className={`text-sm px-3 py-1 rounded-full transition-colors ${
            pathname === v.href ? "bg-primary text-primary-foreground" : "text-muted-foreground hover:text-foreground"
          }`}
        >
          {v.label}
        </Link>
      ))}
    </div>
  )
}
```

Variants-Array wird in `app/layout.tsx` aus `variant-blueprints.md` (hart) gesetzt. Der Switcher wird vor Delivery entfernt.

---

### Step 8 — Varianten bauen (FÜR JEDE VARIANTE WIEDERHOLEN)

Für jede Variante (1 bis N) aus `variant-blueprints.md`:

#### 8a. Blueprint lesen
Notiere für die aktuelle Variante:
- Persönlichkeits-Name
- Visual Strategy (Rhythmus, Schwerpunkt, Atmosphäre-Akzent)
- Section Layout Plan (welches Layout pro Section)
- Bild-Zuweisung pro Section

#### 8b. Custom Components schreiben (Section für Section)

**Für jede Section ein eigenes File** in `components/sections/variant-N/<section>.tsx`.

Regeln:
1. **Semantisches HTML.** `<section>`, `<article>`, `<header>`, `<footer>`, `<h2>`, `<p>`. Keine anonymen `<div>`-Wrapper wo ein Semantic passt.
2. **Container zentriert** via Tailwind-Utilities direkt:
   ```tsx
   <section className="py-16 md:py-24 lg:py-32">
     <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">…</div>
   </section>
   ```
   Bei Full-Bleed Background das äussere `<section>` mit `bg-primary` o.ä., den inneren Container aber zentriert lassen.
3. **Keine hardcoded Farben.** Nur `bg-primary`, `text-foreground`, `bg-muted/40`, `text-accent-foreground` — also Tailwind-Utilities die aus den CSS-Vars in `globals.css` gemappt sind.
4. **Keine pill-badges für Orte/Labels.** Clean Text / einfache Liste.
5. **Buttons via `<Button variant="…">`.** Auf dunklem Section-Background → `variant="outline-on-dark"`, niemals generische Tailwind-Outline.
6. **Content aus Phase 6 einsetzen** (Text ist identisch über Varianten — nur das Layout ändert sich).
7. **Bild:** Jede Section bekommt ein Bild. Wenn scraped → `next/image`. Wenn nicht → `<ImagePlaceholder label="…" />`.
8. **Clean Code:** Eine Section = eine Component. Keine Inline-Komplex-Logik. Kleine lokale Helper als `function` im selben File, keine neue Files für Trivialitäten.
9. **Accessibility:** `alt` auf Images, `aria-label` auf Icon-only Buttons, `tel:` Links für Telefonnummern.
10. **Keine eigenen Custom-Classes definieren** — wenn du merkst du willst eine schreiben, prüf ob Tailwind-Utilities reichen. Wenn nicht (sehr selten), dokumentiere die Eigenart in `design.md` + füge eine schmale Utility in globals.css hinzu.

Beispiel (Hero für Variant 1) — **reine Tailwind-Utilities**, keine `.section`-Klasse:

```tsx
// components/sections/variant-1/hero.tsx
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
            {/* Primary KW + City aus Phase 6 Content */}
          </h1>
          <p className="mt-6 max-w-xl text-lg text-muted-foreground">
            {/* Subheadline aus Phase 6 */}
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

#### 8c. Section Checks (nach jeder Section)

**CHECK 1 — Zentriert?** Innerer Container mit `mx-auto max-w-7xl px-4 sm:px-6 lg:px-8` vorhanden?

**CHECK 2 — Buttons lesbar auf dem eigenen Section-Hintergrund?**
- Section mit hellem Hintergrund → `variant="primary"` (dark on light)
- Section mit dunklem/accent-Hintergrund → `variant="primary"` (wenn `primary-foreground` dafür lesbar ist) ODER inline `className="bg-white text-primary"` Override
- Secondary auf dunkel: **nie** Tailwind-Default `border`-Utility ohne Farbangabe — immer `variant="outline-on-dark"`

**CHECK 3 — Keine Pill-Badges?** Orte/Kategorien → inline Text oder Liste.

**CHECK 4 — Bild vorhanden?** Kein leerer Space.

**CHECK 5 — Anti-Muster aus `design.md` nicht verletzt?**

#### 8d. Keine fremden Blocks, kein `/frontend-design`

Das Feintuning (Typografie-Scale, Spacing, Hover, Atmosphäre) kommt aus den Tokens in `design.md` und den Varianten-Schwerpunkten im Blueprint. Keine externe Skill-Invocation.

Wenn eine Variante „flach" wirkt → die Anpassungen passieren im Component-Code mit den design.md-Tokens (z.B. stärkere Schatten aus der Atmosphäre-Sektion, tighter Typo aus dem Scale).

---

### Step 9 — Layout (SHARED)

`app/layout.tsx`:

```tsx
import "./globals.css"
import { Header } from "@/components/layout/header"
import { Footer } from "@/components/layout/footer"
import { VariantSwitcher } from "@/components/layout/variant-switcher"
import { SchemaScript } from "@/components/seo/schema-script"
import { siteConfig } from "@/lib/site-config"
import { <HeadingFont>, <BodyFont> } from "next/font/google"

const heading = <HeadingFont>({ subsets: ["latin"], variable: "--font-heading", display: "swap" })
const body = <BodyFont>({ subsets: ["latin"], variable: "--font-body", display: "swap" })

export const metadata = {
  title: "<Meta Title aus Phase 6>",
  description: "<Meta Description aus Phase 6>",
  openGraph: { title: "…", description: "…" },
}

const VARIANTS = [
  { href: "/", label: "Design 1", name: "<Persönlichkeit 1>" },
  { href: "/variant-2", label: "Design 2", name: "<Persönlichkeit 2>" },
  { href: "/variant-3", label: "Design 3", name: "<Persönlichkeit 3>" },
]

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="de" className={`${heading.variable} ${body.variable}`}>
      <body className="bg-background text-foreground font-body">
        <Header />
        {children}
        <Footer />
        <VariantSwitcher variants={VARIANTS} />
        <SchemaScript />
      </body>
    </html>
  )
}
```

### Step 10 — page.tsx Assembly pro Variante

```tsx
// app/page.tsx (Variant 1)
import { Hero } from "@/components/sections/variant-1/hero"
import { TrustBar } from "@/components/sections/variant-1/trust-bar"
import { FeaturedService1 } from "@/components/sections/variant-1/featured-service-1"
import { FeaturedService2 } from "@/components/sections/variant-1/featured-service-2"
import { ServicesGrid } from "@/components/sections/variant-1/services-grid"
import { AboutTeaser } from "@/components/sections/variant-1/about-teaser"
import { SocialProof } from "@/components/sections/variant-1/social-proof"
import { LocalArea } from "@/components/sections/variant-1/local-area"
import { CtaSection } from "@/components/sections/variant-1/cta-section"

export default function Home() {
  return (
    <main>
      <Hero />
      <TrustBar />
      <FeaturedService1 />
      <FeaturedService2 />
      <ServicesGrid />
      <AboutTeaser />
      <SocialProof />
      <LocalArea />
      <CtaSection />
    </main>
  )
}
```

`app/variant-2/page.tsx` + `app/variant-3/page.tsx` importieren aus den jeweiligen `variant-2/` bzw. `variant-3/` Ordnern.

Metadata ist IDENTISCH über Varianten-Pages (gleicher SEO Content).

### Step 11 — Schema

`components/seo/schema-script.tsx` — shared LocalBusiness JSON-LD aus Phase 7. Wird in `layout.tsx` eingebunden.

### Step 12 — Final Check (vor Phase 10)

**Tokens:**
- [ ] globals.css spiegelt alle Tokens aus `design.md`
- [ ] Keine hardcoded Farben in irgendeiner Component
- [ ] Keine `style={}` props
- [ ] Fonts via `next/font`, Utility via `font-heading` / `font-body`

**Varianten:**
- [ ] N Varianten fertig, jede mit Persönlichkeits-Namen aus Blueprint
- [ ] Kein Section-Layout wiederholt sich über Varianten (Hero von V1 ≠ Hero von V2 ≠ Hero von V3)
- [ ] Content identisch über Varianten
- [ ] Jede Section hat ein Bild (Placeholder wenn nötig) — mind. 5 Bilder pro Variante

**Build:**
- [ ] `npm run build` passes
- [ ] Variant Switcher zeigt alle N Varianten
- [ ] Keine Anti-Muster aus `design.md` verletzt

---

## Präsentation an User

Nach dem Build:
```
=== N DESIGN VARIANTS READY ===

Starte den Dev-Server mit `npm run dev` und vergleiche:

/ .............. Variant 1: [Persönlichkeits-Name]
/variant-2 ..... Variant 2: [Persönlichkeits-Name]
/variant-3 ..... Variant 3: [Persönlichkeits-Name]

Nutze den Variant-Switcher am unteren Rand zum Wechseln.

Sag mir:
- Welche Variante gefällt dir am besten?
- Willst du Elemente mixen? (z.B. "Hero von Variant 2, Services von Variant 1")
- Spezifische Tweaks?

Nach der Wahl werden die anderen Varianten entfernt.
```
