# Extend Phase 6 — Technical Files

## sitemap.xml — `app/sitemap.ts`

Auto-generated from all pages + Sanity content:

```tsx
import { client } from '@/lib/sanity'
import { MetadataRoute } from 'next'

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const baseUrl = siteConfig.url

  // Static pages
  const staticPages = [
    '', '/ueber-uns', '/team', '/leistungen', '/regionen',
    '/faq', '/blog', '/kontakt', '/impressum', '/datenschutz',
  ].map(route => ({
    url: `${baseUrl}${route}`,
    lastModified: new Date(),
    changeFrequency: 'monthly' as const,
    priority: route === '' ? 1 : 0.8,
  }))

  // Dynamic pages from Sanity
  const services = await client.fetch(allServiceSlugsQuery)
  const team = await client.fetch(allTeamSlugsQuery)
  const regions = await client.fetch(allRegionSlugsQuery)
  const posts = await client.fetch(allPostSlugsQuery)
  const categories = await client.fetch(allCategorySlugsQuery)

  const dynamicPages = [
    ...services.map(s => ({ url: `${baseUrl}/leistungen/${s.slug}`, priority: 0.9 })),
    ...team.map(t => ({ url: `${baseUrl}/team/${t.slug}`, priority: 0.6 })),
    ...regions.map(r => ({ url: `${baseUrl}/regionen/${r.slug}`, priority: 0.8 })),
    ...posts.map(p => ({ url: `${baseUrl}/blog/${p.slug}`, priority: 0.7 })),
    ...categories.map(c => ({ url: `${baseUrl}/blog/kategorie/${c.slug}`, priority: 0.5 })),
  ]

  return [...staticPages, ...dynamicPages]
}
```

## robots.txt — `app/robots.ts`

```tsx
import { MetadataRoute } from 'next'

export default function robots(): MetadataRoute.Robots {
  return {
    rules: [
      {
        userAgent: '*',
        allow: '/',
        disallow: ['/api/', '/studio/'],
      },
    ],
    sitemap: `${siteConfig.url}/sitemap.xml`,
  }
}
```

## llms.txt — `public/llms.txt`

```
# [Company Name]

> [One-line description with primary keyword + city]

## About
[2-3 sentences about the company]

## Services
- [Service 1]: [short description]
- [Service 2]: [short description]
...

## Contact
- Address: [full address]
- Phone: [phone]
- Email: [email]
- Website: [url]

## Pages
- [url]/: Homepage
- [url]/leistungen: Services
- [url]/ueber-uns: About
- [url]/kontakt: Contact
- [url]/blog: Blog
```

## llms-full.txt — `public/llms-full.txt`

Same as llms.txt but with full service descriptions, team bios, FAQ content. More detailed for AI crawlers that want depth.

## 404 Page — `app/not-found.tsx`

Branded, helpful, matches design:
- H1: "Seite nicht gefunden"
- Friendly message
- Search or popular links
- CTA back to home
- Uses design system colors/fonts
