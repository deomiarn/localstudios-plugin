# Phase 8 — Schema Markup

## Reference
Load `./references/schema-templates.md` for JSON-LD templates.

## Agent
Spawn `schema-generator` agent.

## Schema per Page

| Page | Schema Types |
|------|-------------|
| Home | LocalBusiness, WebSite, BreadcrumbList |
| About | Organization/Person, BreadcrumbList |
| Service (each) | Service, FAQPage, BreadcrumbList |
| Contact | LocalBusiness (duplicate), BreadcrumbList |

## LocalBusiness Requirements
- Industry-specific @type (Dentist, Restaurant, Plumber, etc.)
- Complete NAP — no empty fields
- Opening hours in DayOfWeek format
- `sameAs` array: Google Business Profile, social media URLs
- `areaServed` with geo coordinates if known
- Consistent `@id`: `[domain]/#localbusiness`

## Validation Rules
- No empty required fields (mark unknowns as `[PLACEHOLDER]`)
- @id consistent across all pages
- FAQ text matches visible content exactly
- ISO country codes (3166-1 alpha-2)
- ISO currency codes (4217)
- ISO 8601 dates and times
- Valid JSON (no trailing commas)

## Output
Complete `<script type="application/ld+json">` blocks per page, ready to embed.
