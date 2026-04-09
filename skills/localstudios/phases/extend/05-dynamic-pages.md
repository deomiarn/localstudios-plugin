# Extend Phase 5 — Dynamic Pages (Sanity CMS)

## Task
Build all dynamic pages that pull content from Sanity CMS.

## Build Process — SAME AS HOMEPAGE
Every dynamic page template uses the same build process as static pages:
shadcnblocks suchen → installieren → editieren → /frontend-design → checks.
See Phase 4 "Build Process" section for the exact steps.

## Pages

### Service Detail — `app/leistungen/[slug]/page.tsx`
- Fetch service by slug from Sanity
- H1: [Service] — [Company] [City]
- Sections: Hero + What & Why + Process Steps + Benefits + FAQ + CTA
- Service schema + FAQPage schema + BreadcrumbList
- generateStaticParams for all services
- generateMetadata from Sanity SEO fields
- Internal links to related services + contact

### Team Member — `app/team/[slug]/page.tsx`
- Fetch member by slug from Sanity
- H1: [Name] — [Role] — [Company]
- Sections: Photo + Bio + Qualifications + Contact CTA
- Person schema + BreadcrumbList
- generateStaticParams for all members

### Region Detail — `app/regionen/[slug]/page.tsx`
- Fetch region by slug from Sanity
- H1: [Service] in [Region] — [Company]
- Sections: Description + Services available + Map + CTA
- Strong GEO signals (postal codes, neighborhoods, landmarks)
- LocalBusiness schema with areaServed
- BreadcrumbList
- generateStaticParams for all regions

### Blog Category — `app/blog/kategorie/[slug]/page.tsx`
- Fetch category + posts by category from Sanity
- H1: [Category] — Blog — [Company]
- Grid of filtered posts
- BreadcrumbList

### Blog Post — `app/blog/[slug]/page.tsx`
- Fetch post by slug from Sanity
- Rich content rendering (portable text → React components)
- Author info with link to team member
- Related posts
- BlogPosting schema + BreadcrumbList
- Social sharing (OG tags from Sanity)
- generateStaticParams for all posts
- generateMetadata from Sanity SEO fields

## Technical Notes

### Data Fetching Pattern
```tsx
// app/leistungen/[slug]/page.tsx
import { client } from '@/lib/sanity'
import { serviceBySlugQuery } from '@/lib/queries'

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
Use `@portabletext/react` for rich content from Sanity.
Define custom components for headings, images, links etc.

### Image Handling
Use Sanity image URLs with `next/image`:
```tsx
import imageUrlBuilder from '@sanity/image-url'
const builder = imageUrlBuilder(client)
```
