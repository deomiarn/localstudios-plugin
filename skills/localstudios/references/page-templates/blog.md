# Blog Page Template

## Route: `/blog` (overview) + `/blog/kategorie/[slug]` + `/blog/[slug]` (post)

## Overview Sections
1. **Hero** — H1: "Blog — [Company]"
2. **Category Filter** — Clean text buttons/tabs (NOT pill badges), filter posts
3. **Posts Grid** — Cards mit Cover Image, Title, Excerpt, Date, Category, Author
4. **Pagination** — Numbered pages or "Load more"

## Category Sections
1. **Hero** — H1: "[Category] — Blog — [Company]"
2. **Posts Grid** — Filtered by category

## Post Sections
1. **Header** — Title, Author + photo, Date, Category, Reading time
2. **Cover Image** — Full width
3. **Content** — Portable text rendered (headings, paragraphs, images, lists)
4. **Author Box** — Bio, photo, link to team page
5. **Related Posts** — 2-3 related articles
6. **CTA** — Subscribe or contact

## SEO
- BlogPosting schema on posts
- BreadcrumbList: Home > Blog > [Post] or Home > Blog > [Category] > [Post]
- Author linked to Person schema
- publishedAt, dateModified
- Unique meta title + description per post

## Technical
- Sanity portable text → React components
- Image optimization via Sanity CDN + next/image
- ISR for blog posts (revalidate on publish)
