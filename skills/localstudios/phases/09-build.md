# Phase 9 — Build Homepage (Custom Components)

## References
- Load `./references/code-standards.md`
- Load `./references/ui-rules.md`
- Load `design.md` (Projekt-Root — aus Phase 8, **READ-ONLY**)
- Load `layout-plan.md` (Projekt-Root — aus Phase 8)
- **Skill `frontend-design`** sollte aus Phase 8 noch geladen sein — falls nicht, neu laden. Prinzipien (Composition, Typography, Color, Motion, Anti-Patterns, Quality Gate) gelten beim Component-Bau.

## Architecture

```
design.md          → Single Source of Truth (Farben, Fonts, Buttons, Spacing, Atmosphäre, Anti-Muster) — READ-ONLY
layout-plan.md     → Layout-Strategie pro Section (EIN Plan für EINE Homepage)
globals.css        → CSS-Vars aus design.md + Tailwind v4 @theme inline Mapping
components/sections/…  → Custom Components (flach, semantisches HTML + Tailwind-Utilities)
ui-rules.md        → Harte Regeln (keine hardcoded Farben, zentriert, Button-Kontrast, …)
```

## CRITICAL: design.md + layout-plan.md + frontend-design Skill sind das Rezept

Lies alle drei. Weiche NICHT davon ab.
- `design.md` gibt dir WAS diese Brand aussieht (Tokens, Gefühl, Atmosphäre) — **READ-ONLY. Niemals editieren.**
- `layout-plan.md` gibt dir WELCHES Layout pro Section gebaut wird.
- `frontend-design` Skill gibt dir die Design-Qualitäts-Prinzipien (Direction committen, intentional Composition, Typography-Handwerk, Anti-Patterns vermeiden, Quality Gate).
- Keine fremden Block-Libraries. Keine vorgefertigten Template-Components. Keine Varianten.

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

### Step 5 — Dateistruktur (flach, EINE Homepage)

```
app/
  page.tsx                → EINZIGE Homepage (alle Sections assembled)
  layout.tsx              → Header, Footer, Schema, Fonts
  globals.css             → aus design.md abgeleitet (CSS-Vars + @theme inline)
components/
  sections/
    hero.tsx
    trust-bar.tsx
    featured-service-1.tsx
    featured-service-2.tsx
    services-grid.tsx
    about-teaser.tsx
    social-proof.tsx
    local-area.tsx
    cta-section.tsx
    faq.tsx                 → optional (wenn im Content-Outline vorhanden)
  layout/
    header.tsx
    footer.tsx
  ui/
    button.tsx              → komponiert Tailwind-Utilities per Variant, nutzt CSS-Vars
    image-placeholder.tsx   → Gradient-Placeholder für fehlende Bilder
  seo/
    schema-script.tsx
lib/
  cn.ts
  site-config.ts
design.md                   → READ-ONLY (ab Phase 8)
layout-plan.md              → READ-ONLY (ab Phase 8)
```

Kein `variant-N/` Ordner. Kein `app/variant-2/`. Kein `variant-switcher.tsx`. Eine Homepage-Route: `/`.

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

### Step 7 — Sections bauen (einmalig für alle 10 Sections)

Lies `layout-plan.md` für die Section-Layout-Strategie. Alle Tokens/Farben/Fonts kommen aus `design.md` (via globals.css).

#### 7a. Layout-Plan lesen
Für jede der 10 Sections:
- Layout-Ansatz (Split / Full-Bleed / Grid / Zentriert)
- Komposition (was wo)
- Bild-Plan (scraped URL ODER ImagePlaceholder mit welchem Aspect)

#### 7b. Custom Components schreiben (Section für Section)

**Für jede Section ein eigenes File** in `components/sections/<section>.tsx` (flach, ohne variant-Unterordner).

Regeln:
1. **Semantisches HTML.** `<section>`, `<article>`, `<header>`, `<footer>`, `<h2>`, `<p>`. Keine anonymen `<div>`-Wrapper wo ein Semantic passt.
2. **Container zentriert** via Tailwind-Utilities direkt:
   ```tsx
   <section className="py-16 md:py-24 lg:py-32">
     <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">…</div>
   </section>
   ```
   Bei Full-Bleed Background das äussere `<section>` mit `bg-primary` o.ä., den inneren Container aber zentriert lassen.
3. **Keine hardcoded Farben.** Nur `bg-primary`, `text-foreground`, `bg-muted/40`, `text-accent-foreground` — Tailwind-Utilities die aus den CSS-Vars in `globals.css` gemappt sind.
4. **Keine pill-badges für Orte/Labels.** Clean Text / einfache Liste.
5. **Buttons via `<Button variant="…">`.** Auf dunklem Section-Background → `variant="outline-on-dark"`, niemals generische Tailwind-Outline.
6. **Content aus Phase 6 einsetzen** — Text hart reingeschrieben.
7. **Bild:** Jede Section bekommt ein Bild. Wenn scraped → `next/image`. Wenn nicht → `<ImagePlaceholder label="…" />`.
8. **Clean Code:** Eine Section = eine Component. Keine Inline-Komplex-Logik.
9. **Accessibility:** `alt` auf Images, `aria-label` auf Icon-only Buttons, `tel:` Links für Telefonnummern.
10. **Keine eigenen Custom-Classes definieren** — wenn du merkst du willst eine schreiben, prüf ob Tailwind-Utilities reichen.
11. **Keine `[Image #N]`, `[IMG]`, `[Foto]` Platzhalter-Syntax im JSX-Text.** Image-Metadaten gehören in `docs/pages/home.md`, niemals ins JSX-Content. Im JSX: entweder `<Image src="…" alt="…" />` oder `<ImagePlaceholder label="…" />`.

#### 7c. Hero — PFLICHT: Bild above-the-fold

**Der Hero MUSS ein Bild above-the-fold haben.** Text-only Hero ist verboten. Zwei zulässige Layouts:

**Layout A — Split (empfohlen default):**
```tsx
// components/sections/hero.tsx
import Image from "next/image"
import { Button } from "@/components/ui/button"
import { ImagePlaceholder } from "@/components/ui/image-placeholder"
import { siteConfig } from "@/lib/site-config"

export function Hero() {
  return (
    <section className="py-12 md:py-16 lg:py-20">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 grid items-center gap-8 lg:gap-12 lg:grid-cols-[1.1fr_1fr]">
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

        {/* Bild-Proportion: aspect-[4/5] oder aspect-[3/4]. NIE aspect-video (zu flach für Hero). */}
        {/* Mit scraped Bild: <Image src="..." alt="..." width={960} height={1200} priority className="rounded-[var(--radius)] object-cover w-full h-full" /> */}
        <ImagePlaceholder label="Praxis-Aussenansicht" className="aspect-[4/5]" />
      </div>
    </section>
  )
}
```

**Layout B — Full-Bleed Background-Image mit Overlay-Text:**
```tsx
// components/sections/hero.tsx (Alternative, wenn layout-plan.md das vorgibt)
import Image from "next/image"

export function Hero() {
  return (
    <section className="relative isolate overflow-hidden">
      {/* Background Image — MUSS above-the-fold sichtbar sein */}
      <Image
        src="/images/home-hero.webp"
        alt="Praxis-Aussenansicht"
        fill
        priority
        className="object-cover -z-10"
      />
      <div className="absolute inset-0 bg-foreground/50 -z-10" />
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-20 md:py-32 lg:py-40 text-white">
        <h1 className="text-4xl md:text-5xl lg:text-6xl font-extrabold tracking-tight max-w-3xl">
          {/* Primary KW + City */}
        </h1>
        <p className="mt-6 max-w-xl text-lg text-white/80">{/* Subheadline */}</p>
        {/* CTAs via Button-Component */}
      </div>
    </section>
  )
}
```

Wenn Phase 1 ein Bild scraped hat → echtes `next/image` mit `priority`.
Wenn nicht → `<ImagePlaceholder>` mit **mindestens `aspect-[4/5]` oder `aspect-[3/4]`**. NICHT `aspect-video` — der ist zu flach, das Bild wirkt nicht.

#### 7d. Section Checks (nach jeder Section)

**CHECK 1 — Zentriert?** Innerer Container mit `mx-auto max-w-7xl px-4 sm:px-6 lg:px-8` vorhanden?

**CHECK 2 — Buttons lesbar auf dem eigenen Section-Hintergrund?**
- Section mit hellem Hintergrund → `variant="primary"` (dark on light)
- Section mit dunklem/accent-Hintergrund → `variant="primary"` (wenn `primary-foreground` dafür lesbar ist) ODER inline `className="bg-white text-primary"` Override
- Secondary auf dunkel: **nie** Tailwind-Default `border`-Utility ohne Farbangabe — immer `variant="outline-on-dark"`

**CHECK 3 — Keine Pill-Badges?** Orte/Kategorien → inline Text oder Liste.

**CHECK 4 — Bild vorhanden?** Kein leerer Space. Für Hero: Bild MUSS above-the-fold sichtbar sein.

**CHECK 5 — Keine `[Image …]`-Platzhalter-Text-Syntax im JSX?**

**CHECK 6 — Anti-Muster aus `design.md` nicht verletzt?**

#### 7e. Keine fremden Blocks, kein `/frontend-design`

Das Feintuning (Typografie-Scale, Spacing, Hover, Atmosphäre) kommt aus den Tokens in `design.md` und dem Layout-Plan. Keine externe Skill-Invocation.

Wenn die Homepage „flach" wirkt → die Anpassungen passieren im Component-Code mit den design.md-Tokens (z.B. stärkere Schatten, tighter Typo). **Niemals design.md ändern.**

---

### Step 8 — Layout

`app/layout.tsx`:

```tsx
import "./globals.css"
import { Header } from "@/components/layout/header"
import { Footer } from "@/components/layout/footer"
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

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="de" className={`${heading.variable} ${body.variable}`}>
      <body className="bg-background text-foreground font-body">
        <Header />
        {children}
        <Footer />
        <SchemaScript />
      </body>
    </html>
  )
}
```

### Step 9 — page.tsx Assembly (einmalig)

```tsx
// app/page.tsx — EINZIGE Homepage
import { Hero } from "@/components/sections/hero"
import { TrustBar } from "@/components/sections/trust-bar"
import { FeaturedService1 } from "@/components/sections/featured-service-1"
import { FeaturedService2 } from "@/components/sections/featured-service-2"
import { ServicesGrid } from "@/components/sections/services-grid"
import { AboutTeaser } from "@/components/sections/about-teaser"
import { SocialProof } from "@/components/sections/social-proof"
import { LocalArea } from "@/components/sections/local-area"
import { CtaSection } from "@/components/sections/cta-section"
// FAQ optional (wenn im Content-Outline)
// import { Faq } from "@/components/sections/faq"

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
      {/* <Faq /> */}
    </main>
  )
}
```

### Step 10 — Schema

`components/seo/schema-script.tsx` — LocalBusiness JSON-LD aus Phase 7. Wird in `layout.tsx` eingebunden.

### Step 11 — Final Check (vor Phase 10)

**Tokens:**
- [ ] globals.css spiegelt alle Tokens aus `design.md` (READ-ONLY geblieben)
- [ ] Keine hardcoded Farben in irgendeiner Component
- [ ] Keine `style={}` props
- [ ] Fonts via `next/font`, Utility via `font-heading` / `font-body`

**Homepage:**
- [ ] Alle 10 Sections vorhanden und rendern sauber auf `/`
- [ ] **Hero hat above-the-fold Bild** (scraped next/image ODER ImagePlaceholder mit mindestens `aspect-[4/5]`)
- [ ] Keine `[Image …]`-Platzhalter-Strings im JSX
- [ ] Jede Section hat ein Bild — min. 5 Bilder auf der Homepage
- [ ] Keine Anti-Muster aus `design.md` verletzt

**design.md Integrität:**
- [ ] `design.md` unverändert ggü. Phase 8 (nur Farbtoken-Ersetzung falls Farbwechsel aus Phase 2)

**Build:**
- [ ] `npm run build` passes

---

## Präsentation an User

Nach dem Build:
```
=== HOMEPAGE READY ===

Starte den Dev-Server mit `npm run dev` und schau's dir an:
→ http://localhost:3000/

Sag mir:
- Passt das Layout zur `design.md` Vorlage?
- Spezifische Tweaks (z.B. Section-Reihenfolge, Hero-Komposition, mehr Whitespace)?
- Fehlende Bilder die ich ergänzen soll?
```
