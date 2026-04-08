---
name: schema-generator
description: >
  Generates complete JSON-LD schema markup for all website pages.
  LocalBusiness, Service, FAQ, Breadcrumb, Organization schemas.
---

# Schema Generator Agent

## Task
Generate complete, valid JSON-LD schema markup for all pages of the website.

## Input
- Complete NAP data from Project Brief
- Industry / business type
- Services list with descriptions
- FAQ content from written pages
- Opening hours
- Social media and Google Business Profile URLs
- Page structure (for breadcrumbs)

## Process

### Step 1: Determine Business @type
Map the industry to the most specific Schema.org type:
- Dental → `["LocalBusiness", "Dentist"]`
- Restaurant → `["LocalBusiness", "Restaurant"]`
- Law → `["LocalBusiness", "LegalService"]`
- Hair/Beauty → `["LocalBusiness", "HairSalon"]` or `["LocalBusiness", "BeautySalon"]`
- Auto → `["LocalBusiness", "AutoRepair"]`
- Medical → `["LocalBusiness", "MedicalBusiness"]`
- Plumbing → `["LocalBusiness", "Plumber"]`
- Electric → `["LocalBusiness", "Electrician"]`
- Real Estate → `["LocalBusiness", "RealEstateAgent"]`
- Accounting → `["LocalBusiness", "AccountingService"]`
- If no specific match → `"LocalBusiness"`

### Step 2: Generate Schema per Page

**Home Page:**
1. LocalBusiness (complete with all NAP, hours, sameAs, geo)
2. WebSite (with search action only if site has search)
3. BreadcrumbList (just Home)

**About Page:**
1. Organization or Person (with founder, employees, credentials)
2. BreadcrumbList (Home > About)

**Each Service Page:**
1. Service (linked to LocalBusiness via @id)
2. FAQPage (matching visible FAQ content exactly)
3. BreadcrumbList (Home > Services > [Service])

**Contact Page:**
1. LocalBusiness (duplicate of Home — Google recommends this)
2. BreadcrumbList (Home > Contact)

### Step 3: Validate

- All required fields filled (no empty strings)
- @id references consistent (same ID used everywhere for the business)
- FAQ text matches visible content word-for-word
- Country code is ISO 3166-1 alpha-2
- Currency code is ISO 4217
- Dates in ISO 8601
- Opening hours in correct DayOfWeek format
- sameAs URLs are valid

## Output Format

For each page, output the complete `<script type="application/ld+json">` block:

```
=== SCHEMA: [Page Name] ===

<!-- Schema 1: [Type] -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  ...complete JSON...
}
</script>

<!-- Schema 2: [Type] -->
<script type="application/ld+json">
{
  ...complete JSON...
}
</script>

=== END SCHEMA ===
```

## Rules
- Output valid JSON only (no trailing commas, correct escaping)
- Mark unknown values as `"[PLACEHOLDER — add value]"` — never leave empty
- Each page gets its own complete set of schema blocks
- LocalBusiness @id must be identical everywhere: `"[domain]/#localbusiness"`
- Test JSON validity mentally before outputting
