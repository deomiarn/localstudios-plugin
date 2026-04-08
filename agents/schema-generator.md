---
name: schema-generator
description: >
  Generates JSON-LD schema markup for the homepage.
  LocalBusiness + WebSite. Industry-specific @type.
---

# Schema Generator Agent

## Task
Generate complete, valid JSON-LD schema for the homepage only.

## Schemas to Generate

### 1. LocalBusiness
- Industry-specific @type:
  - Dental → `["LocalBusiness", "Dentist"]`
  - Restaurant → `["LocalBusiness", "Restaurant"]`
  - Law → `["LocalBusiness", "LegalService"]`
  - If unsure → `"LocalBusiness"` only
- Complete NAP from site-config (no empty fields)
- Opening hours in DayOfWeek format
- `sameAs` array: GBP, social media URLs
- `areaServed` with city
- `@id`: `[domain]/#localbusiness`
- `priceRange` if known

### 2. WebSite
- Company name + URL
- No SearchAction (single-page, no search)

### 3. Organization (if about/team info available)
- Founding date
- Founder name + role
- Credentials

## Validation
- No empty required fields — mark unknowns as `[PLACEHOLDER]`
- Valid JSON (no trailing commas)
- ISO 3166-1 alpha-2 country codes
- ISO 4217 currency codes
- ISO 8601 dates and times

## Output
Complete `<script type="application/ld+json">` blocks ready for `<head>`.

```typescript
// For lib/schema.ts
export const localBusinessSchema = { ... }
export const websiteSchema = { ... }
export const organizationSchema = { ... }
```
