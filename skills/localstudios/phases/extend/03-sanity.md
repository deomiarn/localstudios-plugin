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

## Step 5 — Create Empty Collections (Placeholder)

Create the collection structure in Sanity with minimal placeholder content:
- Globals document with NAP from site-config.ts (this is complete)
- Service documents: title + slug only (content comes in Phase 7)
- Team member documents: name + slug + role only
- Region documents: name + slug only
- Blog category documents: title + slug only

Use Sanity MCP tools:
```
mcp__Sanity__create_documents_from_json — for structured data like globals
mcp__Sanity__create_documents_from_markdown — for rich text content
```

**IMPORTANT:** Full content is written in Phase 7 (Content). After Phase 7, push the written content INTO Sanity:

### Phase 7 → Sanity Push (execute AFTER Phase 7 content is written)

For each content type:
1. **Services:** Use `mcp__Sanity__patch_document_from_markdown` to add longDescription, process steps, FAQ
2. **Team Members:** Patch with longBio, qualifications
3. **Regions:** Patch with longDescription, servicesAvailable references
4. **Blog Posts:** Use `mcp__Sanity__create_documents_from_markdown` for full posts with portable text content
5. **Verify:** Use `mcp__Sanity__query_documents` to confirm all content is populated

This two-step flow (Phase 3: structure → Phase 7: content → push to Sanity) ensures content quality is controlled in Phase 7 before entering the CMS.
