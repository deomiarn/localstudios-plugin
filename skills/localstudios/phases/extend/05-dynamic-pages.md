# Extend Phase 5 — Dynamic Pages (Sanity CMS)

## References
- Load `./references/image-strategy.md` for image sourcing, alt text templates, and per-page-type minimums
- Load `./references/content-guidelines.md` for writing rules
- Load `./references/ui-rules.md` for CSS constraints
- Load `./references/code-standards.md` for custom-component rules
- Load `design.md` (Homepage-Tokens — identisch für alle Dynamic Pages)

## Task
Build all dynamic pages that pull content from Sanity CMS.

## Style = Homepage Style
Der Homepage-Style wird 1:1 auf alle dynamischen Pages übertragen.
Gleiche `globals.css`, gleiche Buttons, gleiche Typografie, gleiche Atmosphäre.

## Build Process — Custom Components (wie Static Pages)

Jede Dynamic Page wird als **Komposition von Custom Section-Components** gebaut:
1. Layout-Plan (Sections + Rhythmus pro Page-Typ)
2. Section-Components schreiben in `components/sections/<page>/<section>.tsx`
3. Page-File in `app/.../[slug]/page.tsx` mit Sanity-Fetch + Assembly
4. Checks wie in Phase 4

Keine fremden Block-Libraries. Siehe `extend/04-static-pages.md` für den Ablauf im Detail.

## Pages

### Service Detail — `app/leistungen/[slug]/page.tsx`
- Fetch service by slug from Sanity
- H1: [Service] — [Company] [City]
- Sections: Hero + What & Why + Process Steps + Benefits + FAQ + CTA
- Service schema + FAQPage schema + BreadcrumbList
- `generateStaticParams` für alle Services
- `generateMetadata` aus Sanity SEO fields
- Internal Links zu related services + contact

### Team Member — `app/team/[slug]/page.tsx`
- Fetch member by slug from Sanity
- H1: [Name] — [Role] — [Company]
- Sections: Photo + Bio + Qualifications + Contact CTA
- Person schema + BreadcrumbList
- `generateStaticParams` für alle members

### Region Detail — `app/regionen/[slug]/page.tsx`
- Fetch region by slug from Sanity
- H1: [Service] in [Region] — [Company]
- Sections: Description + Services available + Map + CTA
- Starke GEO-Signale (PLZ, Quartiere, Landmarks)
- LocalBusiness schema mit `areaServed`
- BreadcrumbList
- `generateStaticParams` für alle Regions

### Blog Category — `app/blog/kategorie/[slug]/page.tsx`
- Fetch category + posts by category from Sanity
- H1: [Category] — Blog — [Company]
- Grid of filtered posts (Custom Grid-Component, keine Pill-Badges für Tags)
- BreadcrumbList

### Blog Post — `app/blog/[slug]/page.tsx`
- Fetch post by slug from Sanity
- Rich content rendering (portable text → React components)
- Author info mit Link zu Team member
- Related posts
- BlogPosting schema + BreadcrumbList
- Social sharing (OG tags aus Sanity)
- `generateStaticParams` für alle posts
- `generateMetadata` aus Sanity SEO fields

## Technical Notes

### Data Fetching Pattern
```tsx
// app/leistungen/[slug]/page.tsx
import { client } from "@/lib/sanity"
import { serviceBySlugQuery, allServiceSlugsQuery } from "@/lib/queries"
import { siteConfig } from "@/lib/site-config"

type Props = { params: { slug: string } }

export async function generateStaticParams() {
  const services = await client.fetch(allServiceSlugsQuery)
  return services.map((s: { slug: string }) => ({ slug: s.slug }))
}

export async function generateMetadata({ params }: Props) {
  const service = await client.fetch(serviceBySlugQuery, { slug: params.slug })
  return {
    title: `${service.title} — ${siteConfig.name} — ${siteConfig.nap.city}`,
    description: service.description,
  }
}
```

### Portable Text Rendering
`@portabletext/react` für Rich-Content aus Sanity.
Custom Components definieren für headings, images, links — nutzen die Homepage-Typo aus globals.css.

### Image Handling
Sanity Image URLs mit `next/image`:
```tsx
import imageUrlBuilder from "@sanity/image-url"
const builder = imageUrlBuilder(client)
```

Wenn Sanity-Field leer → `<ImagePlaceholder label="…" />` rendern.

### Image Requirements per Dynamic Page Type

| Page | Min Images | Must-Have |
|------|-----------|-----------|
| Service Detail | 4 | Service hero, process steps, result/benefit |
| Team Member | 2 | Portrait, action/context |
| Region Detail | 3 | Area photo, map, local landmark |
| Blog Post | 2 | Cover image, in-content image |
| Blog Category | 1x per post | Cover images in grid |

Alle Images: alt + title laut `image-strategy.md`. Styled Placeholder wo Sanity-Field leer.

### Layout-Varianz
Dynamic-Page-Layouts unterscheiden sich voneinander — nicht die Tokens, aber die Komposition:
- Service Detail Hero ≠ About Hero ≠ Home Hero
- Region Detail Layout anders als Service Detail
- Blog Post: magazin-artige Typo-Dominanz, Blog Grid: Card-dominiert

Der Layout-Rhythmus pro Page-Typ wurde in Phase 2 im Interview besprochen — hier umsetzen.
