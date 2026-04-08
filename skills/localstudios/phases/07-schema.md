# Phase 8 — Schema Markup (Homepage only)

## Reference
Load `./references/schema-templates.md` for JSON-LD templates.

## Generate for Homepage

### 1. LocalBusiness Schema
- Industry-specific @type (Dentist, Restaurant, Plumber, etc.)
- Complete NAP — no empty fields
- Opening hours
- `sameAs` array: Google Business Profile, social media URLs
- `areaServed` with city
- `@id`: `[domain]/#localbusiness`

### 2. WebSite Schema
- Company name + URL
- SearchAction only if site has search

### 3. FAQPage Schema (if FAQ section is included)
- Questions and answers matching visible content exactly

## Validation
- No empty required fields (mark unknowns as `[PLACEHOLDER]`)
- Valid JSON
- ISO country/currency codes
- ISO 8601 dates

## Output
Complete `<script type="application/ld+json">` blocks for the homepage `<head>`.
