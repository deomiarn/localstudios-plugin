# Schema Markup Templates

All schema markup uses JSON-LD format. Generate complete, valid JSON-LD for each page.

---

## LocalBusiness Schema (Home + Contact Page)

```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "@id": "[domain]/#localbusiness",
  "name": "[Company Name]",
  "description": "[1-2 sentences with primary keyword and location]",
  "url": "[domain]",
  "telephone": "[phone]",
  "email": "[email]",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "[street and number]",
    "postalCode": "[postal code]",
    "addressLocality": "[city]",
    "addressRegion": "[region/state/canton]",
    "addressCountry": "[country code, e.g. CH, DE, US, GB]"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "[lat — placeholder if unknown]",
    "longitude": "[lng — placeholder if unknown]"
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
      "opens": "[HH:MM]",
      "closes": "[HH:MM]"
    }
  ],
  "priceRange": "[e.g. $$, CHF 50-200, etc.]",
  "image": "[logo or business photo URL]",
  "sameAs": [
    "[Google Business Profile URL]",
    "[Facebook URL]",
    "[Instagram URL]",
    "[LinkedIn URL]"
  ],
  "founder": {
    "@type": "Person",
    "name": "[founder name if known]"
  },
  "foundingDate": "[YYYY]",
  "areaServed": {
    "@type": "GeoCircle",
    "geoMidpoint": {
      "@type": "GeoCoordinates",
      "latitude": "[lat]",
      "longitude": "[lng]"
    },
    "geoRadius": "[service radius in km]"
  }
}
```

### Industry-Specific @type
Use more specific types when applicable:
- Dentist → `"@type": ["LocalBusiness", "Dentist"]`
- Restaurant → `"@type": ["LocalBusiness", "Restaurant"]`
- Law firm → `"@type": ["LocalBusiness", "LegalService"]`
- Hair salon → `"@type": ["LocalBusiness", "HairSalon"]`
- Auto repair → `"@type": ["LocalBusiness", "AutoRepair"]`
- Plumber → `"@type": ["LocalBusiness", "Plumber"]`
- Electrician → `"@type": ["LocalBusiness", "Electrician"]`
- Real estate → `"@type": ["LocalBusiness", "RealEstateAgent"]`
- Medical → `"@type": ["LocalBusiness", "MedicalBusiness"]`
- If unsure, use `"LocalBusiness"` only

---

## BreadcrumbList Schema (Every Page)

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "[domain]"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "[Current Page Name]",
      "item": "[domain]/[page-slug]"
    }
  ]
}
```

For service pages, add an intermediate level:
```json
{
  "@type": "ListItem",
  "position": 2,
  "name": "Services",
  "item": "[domain]/services"
}
```

---

## Service Schema (Service Pages)

```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "name": "[Service Name]",
  "description": "[Service description with keyword]",
  "provider": {
    "@type": "LocalBusiness",
    "@id": "[domain]/#localbusiness"
  },
  "areaServed": {
    "@type": "City",
    "name": "[City]"
  },
  "serviceType": "[service category]",
  "offers": {
    "@type": "Offer",
    "priceCurrency": "[currency code]",
    "price": "[price or price range if known]",
    "priceSpecification": {
      "@type": "PriceSpecification",
      "priceCurrency": "[currency code]",
      "minPrice": "[min]",
      "maxPrice": "[max]"
    }
  }
}
```

---

## FAQPage Schema (Pages with FAQ Sections)

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "[Question text exactly as in content]",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "[Answer text exactly as in content]"
      }
    }
  ]
}
```

Rules:
- Questions and answers must match the visible content exactly
- Include ALL FAQ items from the page, not a subset
- Keep answers concise (under 300 characters each for best snippet display)

---

## Organization / Person Schema (About Page)

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "[Company Name]",
  "url": "[domain]",
  "logo": "[logo URL]",
  "foundingDate": "[YYYY]",
  "founder": {
    "@type": "Person",
    "name": "[Founder Name]",
    "jobTitle": "[Title]"
  },
  "employee": [
    {
      "@type": "Person",
      "name": "[Team Member]",
      "jobTitle": "[Role]"
    }
  ],
  "award": ["[award or certification]"],
  "memberOf": ["[association or membership]"]
}
```

---

## WebSite Schema (Home Page — for sitelinks search box)

```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "[Company Name]",
  "url": "[domain]",
  "potentialAction": {
    "@type": "SearchAction",
    "target": "[domain]/search?q={search_term_string}",
    "query-input": "required name=search_term_string"
  }
}
```

Only include SearchAction if the site actually has search functionality.

---

## Validation Rules

Before outputting any schema:
- Ensure all required fields are filled (no empty strings)
- Mark unknown values clearly: `"[PLACEHOLDER — add actual value]"`
- Ensure @id references are consistent across pages
- Validate that FAQ content matches visible page content exactly
- Use ISO 8601 format for dates and times
- Use correct country codes (ISO 3166-1 alpha-2)
- Use correct currency codes (ISO 4217)
