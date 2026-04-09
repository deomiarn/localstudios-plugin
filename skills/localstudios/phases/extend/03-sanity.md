# Extend Phase 3 — Sanity CMS Setup

## Task
Set up Sanity CMS for all dynamic content.

## Step 1 — Init Sanity
```bash
npx sanity@latest init --project-id [id] --dataset production
```
Or use Sanity MCP to create/connect project.

## Step 2 — Define Schemas

### Globals (singleton document)
```
globals:
  - companyName: string
  - nap: { street, postalCode, city, region, country, phone, email }
  - openingHours: array of { day, open, close }
  - socialLinks: array of { platform, url }
  - googleMapsUrl: string
```

### Services Collection
```
service:
  - title: string
  - slug: slug
  - description: text
  - longDescription: portable text (rich)
  - image: image
  - icon: string (lucide icon name)
  - featured: boolean
  - process: array of { step, title, description }
  - faq: array of { question, answer }
  - order: number
```

### Team Collection
```
teamMember:
  - name: string
  - slug: slug
  - role: string
  - bio: text
  - longBio: portable text
  - image: image
  - qualifications: array of string
  - email: string (optional)
  - order: number
```

### Regions Collection
```
region:
  - name: string
  - slug: slug
  - description: text
  - longDescription: portable text
  - image: image
  - servicesAvailable: array of references to service
  - postalCodes: array of string
```

### Blog Categories
```
blogCategory:
  - title: string
  - slug: slug
  - description: text
```

### Blog Posts
```
blogPost:
  - title: string
  - slug: slug
  - excerpt: text
  - content: portable text (rich)
  - coverImage: image
  - category: reference to blogCategory
  - author: reference to teamMember
  - publishedAt: datetime
  - tags: array of string
  - seo: { metaTitle, metaDescription }
```

## Step 3 — Sanity Client Setup

Create `lib/sanity.ts`:
```tsx
import { createClient } from 'next-sanity'

export const client = createClient({
  projectId: process.env.NEXT_PUBLIC_SANITY_PROJECT_ID!,
  dataset: 'production',
  apiVersion: '2024-01-01',
  useCdn: true,
})
```

Create `lib/queries.ts` with GROQ queries for each collection.

## Step 4 — Environment Variables
Add to `.env.local`:
```
NEXT_PUBLIC_SANITY_PROJECT_ID=[project-id]
SANITY_API_TOKEN=[token]
```

## Step 5 — Populate Initial Content
Use Sanity MCP or Studio to add:
- Globals document with NAP from site-config.ts
- All services from interview
- All team members from interview
- All regions from interview
- Blog categories from interview
