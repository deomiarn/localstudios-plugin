# Extend Phase 4 — Static Pages

## References
- Load `./references/image-strategy.md` for image sourcing, alt text templates, and per-page-type minimums
- Load `./references/content-guidelines.md` for writing rules
- Load `./references/ui-rules.md` for CSS constraints
- Load `./references/code-standards.md` for file structure + custom-component rules
- Load `design.md` (Projekt-Root — Homepage-Tokens)

## Task
Build all static pages. **Der Style der Homepage wird 1:1 weitergeführt.**

### CRITICAL: Style-Konsistenz mit Homepage
- **Gleiche `design.md`** — keine neuen Tokens erfinden
- **Gleiche `globals.css`** — keine Style-Overrides in Page-Files
- **Gleiche Button-Klassen** — `.btn-primary` / `.btn-secondary` / `.btn-outline` / `.btn-outline-on-dark`
- **Gleiche Section-Abstände** — `.section` Utility identisch
- **Gleiche Typografie-Hierarchie** — H1/H2/H3 aus `@layer base`
- **Gleicher Header/Footer** — auf JEDER Page identisch (via `app/layout.tsx`)
- Die Unterseiten müssen aussehen als wären sie TEIL derselben Website

Was sich pro Page-Typ UNTERSCHEIDET: der **Layout-Rhythmus** (welche Section wo, welche Komposition) — nicht die Tokens.

## Pages to Build (parallel where possible)

### Services Overview — `/leistungen/page.tsx`
Load `./references/page-templates/services-overview.md`
- Grid of all services with images, titles, short descriptions
- Featured services highlighted at top
- Link to each `/leistungen/[slug]` detail page
- SEO: "Unsere Leistungen — [Company] [City]"

### About — `/ueber-uns/page.tsx`
Load `./references/page-templates/about.md`
- Company story (founding, mission, values)
- Why this location, local connection
- Credentials, certifications, memberships
- E-E-A-T signals throughout
- Link to Team page
- Min 3 images

### Team Overview — `/team/page.tsx`
Load `./references/page-templates/team.md`
- Grid of team members with photo, name, role
- Link to each `/team/[slug]` detail page
- Warm, personal tone

### Regions Overview — `/regionen/page.tsx`
Load `./references/page-templates/regions.md`
- Map or visual of service area
- List/grid of all regions served
- Link to each `/regionen/[slug]` detail page
- Strong GEO signals

### FAQ — `/faq/page.tsx`
Load `./references/page-templates/faq.md`
- Accordion-Sections nach Kategorien (Custom-Component mit `<details>` oder controlled state — keine externe Accordion-Library)
- FAQPage schema markup
- Voice-search friendly questions
- Internal links zu relevanten Service-Pages

### Blog Overview — `/blog/page.tsx`
Load `./references/page-templates/blog.md`
- All posts with cover image, title, excerpt, date
- Category filter (Tabs via Custom Component — keine Pill-Badges)
- Clean grid layout
- Pagination oder infinite scroll

### Contact — `/kontakt/page.tsx`
Load `./references/page-templates/contact.md`
- NAP prominent
- Contact form (Fields aus Interview) — Custom Form, native `<input>`/`<textarea>`/`<label>`
- Google Maps embed (URL aus Interview)
- Opening hours
- Additional contact methods
- ContactPage schema

### Impressum — `/impressum/page.tsx`
- Legal company info aus Interview
- Standard Swiss/German/Austrian Format
- Keine Design-Finesse nötig — clean, lesbar (nutzt Homepage-Typo)

### Datenschutz — `/datenschutz/page.tsx`
- Privacy policy basierend auf Tools/Hosting aus Interview
- GDPR/DSG compliant template
- Sections: data collection, cookies, analytics, rights, contact

### 404 — `app/not-found.tsx`
- Branded, matcht design.md Tokens
- Hilfreich: Link zu Home, Kontakt, populären Pages
- Nicht generisch — reflektiert die Brand-Persönlichkeit

## Build Process — Custom Components (same approach as Homepage)

Jede Page wird als **Komposition von Custom Sections** gebaut. Keine fremden Block-Libraries.

### 1. Layout-Plan für die Page
Bevor du Code schreibst: kurzer Plan pro Page, welche Sections in welcher Reihenfolge, mit welchem Layout-Rhythmus. Orientiere dich an `references/page-sections.md` (Minimum-Sections pro Page-Typ) + Interview-Wünschen.

### 2. Section-Components schreiben
Pro Section ein eigenes File in `components/sections/<page>/<section>.tsx` (z.B. `components/sections/about/story.tsx`).

Regeln (aus `ui-rules.md`):
- Semantisches HTML (`<section>`, `<article>`, `<h2>`, `<p>`)
- `.section` Utility für Zentrierung
- CSS-Vars aus globals.css (keine hardcoded Farben)
- Content entsprechend Page-Template
- Buttons via `<Button variant="…" />`
- Jede Section braucht ein Bild (scraped oder `<ImagePlaceholder>`)

### 3. Page Assembly
`app/<page>/page.tsx` importiert und assembliert die Sections.

```tsx
// app/ueber-uns/page.tsx
import { IntroSection } from "@/components/sections/about/intro"
import { StorySection } from "@/components/sections/about/story"
import { CredentialsSection } from "@/components/sections/about/credentials"
import { LocalConnectionSection } from "@/components/sections/about/local-connection"
import { CtaSection } from "@/components/sections/shared/cta"

export const metadata = {
  title: "Über uns — <Company> — <City>",
  description: "…",
}

export default function AboutPage() {
  return (
    <main>
      <IntroSection />
      <StorySection />
      <CredentialsSection />
      <LocalConnectionSection />
      <CtaSection />
    </main>
  )
}
```

### 4. Checks pro Page
- [ ] Zentriert (`.section` oder `mx-auto max-w-7xl`)
- [ ] Buttons lesbar auf jedem Section-Background
- [ ] Keine Pill-Badges
- [ ] Keine hardcoded Farben
- [ ] Bilder-Minimum pro Page-Typ erreicht (siehe Tabelle unten)
- [ ] Metadata export
- [ ] BreadcrumbList schema

## Image Minimums per Page Type (from image-strategy.md)

| Page | Min Images | Must-Have |
|------|-----------|-----------|
| About | 6 | Team photo, owner portrait, certificates, local/community |
| Services Overview | 4 | 1x per service |
| Service Detail | 4 | Hero, process steps, result |
| Contact | 1 | Storefront/exterior + map embed |
| Team Overview | 1x per member | Photos for each team member |
| Regions Overview | 3 | Map, area photos |
| FAQ | 0 | (text-focused, icons OK) |
| Blog Overview | 1x per post | Cover images |
| 404 | 0 | Brand-consistent design |

Alle Bilder: alt + title laut `image-strategy.md`. Styled Placeholder wo Bilder fehlen — niemals leere Spaces.

## Layout-Varianz pro Page-Typ

Damit sich Seiten voneinander unterscheiden, variiert der **Komposition/Rhythmus** — die Tokens bleiben identisch.
Beispiele:
- Services Overview: Grid-dominiert (3-Spalten Cards)
- About: Editorial-Rhythmus (alternierend breit/schmal, grosse Typo)
- Team Overview: Card-Grid mit Portraits (2-4 Spalten)
- Regions Overview: Map-Hero + Liste
- FAQ: einspaltig, lesefokussiert
- Blog Overview: Magazin-Grid (1 Featured gross + 3-4 kleinere)
- Contact: Split (Form links, Map rechts)

**Nicht jeder Hero muss gleich aussehen.** Interview-Wünsche aus Phase 2 pro Page-Typ umsetzen.

## Rules for ALL pages
- Same `globals.css` (design.md Tokens)
- Same header/footer via `app/layout.tsx`
- Sections zentriert
- Buttons readable — `variant="outline-on-dark"` auf dunklen Sections
- No pill badges
- Images per page-type minimums
- Alle Images mit alt + title
- Metadata export mit SEO title + description + OG tags
- BreadcrumbList schema auf jeder Page
